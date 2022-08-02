# Create Arbitrary File
These notes apply when you found a bug where:
* as root or user you can...
	* create an empty file
	* create file and control content (eg. arbitrary file upload)
	* create file and partially control content (eg. inject data into a config file)
		* control filename/path
		* partially control filename or path (eg. fixed extension)


### cronjob /etc/cron.daily
* create a file in `/etc/cron.daily/`
* requires `root` permissions
* executed through `crontab` via `run-parts`
	* file **must** be executable.
	* shell script or binary works

### cronjob via /etc/cron.d/pwn
* create a file in `/etc/cron.d/pwn`
* requires `root` permissions
* file syntax the same as in [crontab](../file-append/README.md#crontab)

### logrotate
* requires `root` permissions
* on Ubuntu 22.04 LTS `logrotate` is executed daily through `/etc/cron.daily/logrotate`
* logrotate config files in `/etc/logrotate.d/<anything>`
* consideres time in `/var/lib/logrotate/status` to determin if log should be rotated (you might have to wait for two days for a new file to trigger a first rotate)
* comment lines start with `# a comment`
* newlines are required! Format is strict
* unkown commands are ignored

#### Clean File
```
/etc/debian_version {
       hourly
       copy
       ifempty
       missingok 
       postrotate /usr/bin/touch /tmp/testrotate 
       endscript
}
```

#### Messy File
```
text and <00><ff>binary is okay here, but must not start with a [symbol]!@#

/etc/debian_version {
       thishere must be valid word at the start. symbols can appear [later!@] but not at the start!
       # a comment with # is similar to invalid command. anything can follow until newline \n (even binary)
       hourly
       copy
       ifempty
       missingok 
       postrotate /usr/bin/touch /tmp/testrotate 
       endscript
}

# comment here
xxxx {
asdas
}

[we]! can have symbols and garbage at the end
<00><ff> binary also ok here
```
### Shell Profiles
Note: On Ubuntu 20.04 and 22.04 `/bin/sh` is symlinked to `/bin/dash`. 
#### Login Shells
`--login` flag. 
The following has been researched for bash. 
* First found of `~/.bash_profile`, `~/.bash_login`, and `~/.profile` is then executed.
* For bash `~/.bash_logout` executed on interactive exit OR noninteractive `exit` command is run.
Two bash quotes and one dash quote: 
> When bash is invoked as an interactive login shell, or as a non-interactive shell with the --login option, it first reads and executes commands from the file /etc/profile, if that file exists.  After reading that file, it looks for ~/.bash_profile, ~/.bash_login, and ~/.profile, in that order, and reads and executes commands from the first one that exists and is readable.  The --noprofile option may be used when the shell is started to inhibit this behavior.
> When an interactive login shell exits, or a non-interactive login shell executes the **exit** builtin command, **bash** reads and executes commands from the file _~/.bash_logout_, if it exists.
> A login shell first reads commands from the files /etc/profile and .profile if they exist.
#### Non-login shell
* `/etc/bash.bashrc` executed for all users, does not exist by default. 
* `~/.bashrc` for user specific.  

Sources: [dash manpage](https://manned.org/dash) and [bash manpage](https://manned.org/bash)

### git hooks
One may be able to sneak in a [git hook](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks#_git_hooks) in the `.git` of a git repo to execute any code of their choosing when a git action happens.
#### Limitations
* File needs to be executable
#### Example with commit message
Hooks should be implemented about the same, commit message is just convinience at the moment. TODO: make a pull example
```bash
redacteduser@ubuntu2004desktop:/tmp/RedactedRepo$ ls .git
branches  config  description  HEAD  hooks  index  info  logs  objects  packed-refs  refs
redacteduser@ubuntu2004desktop:/tmp/RedactedRepo$ ls .git/hooks
applypatch-msg.sample  fsmonitor-watchman.sample  pre-applypatch.sample  pre-merge-commit.sample    pre-push.sample    pre-receive.sample
commit-msg.sample      post-update.sample         pre-commit.sample      prepare-commit-msg.sample  pre-rebase.sample  update.sample
redacteduser@ubuntu2004desktop:/tmp/RedactedRepo$ cp .git/hooks/commit-msg.sample .git/hooks/commit-msg
redacteduser@ubuntu2004desktop:/tmp/RedactedRepo$ cat .git/hooks/commit-msg
#!/bin/sh
#
# An example hook script to check the commit log message.
# Called by "git commit" with one argument, the name of the file
# that has the commit message.  The hook should exit with non-zero
# status after issuing an appropriate message if it wants to stop the
# commit.  The hook is allowed to edit the commit message file.
#
# To enable this hook, rename this file to "commit-msg".

# Uncomment the below to add a Signed-off-by line to the message.
# Doing this in a hook is a bad idea in general, but the prepare-commit-msg
# hook is more suited to it.
#
# SOB=$(git var GIT_AUTHOR_IDENT | sed -n 's/^\(.*>\).*$/Signed-off-by: \1/p')
# grep -qs "^$SOB" "$1" || echo "$SOB" >> "$1"

# This example catches duplicate Signed-off-by lines.

test "" = "$(grep '^Signed-off-by: ' "$1" |
	 sort | uniq -c | sed -e '/^[ 	]*1[ 	]/d')" || {
	echo >&2 Duplicate Signed-off-by lines.
	exit 1
}
redacteduser@ubuntu2004desktop:/tmp/RedactedRepo$ echo "echo executing_something" >> .git/hooks/commit-msg
redacteduser@ubuntu2004desktop:/tmp/RedactedRepo$ echo "A file" > file
redacteduser@ubuntu2004desktop:/tmp/RedactedRepo$ git stage *
redacteduser@ubuntu2004desktop:/tmp/RedactedRepo$ git commit
executing_something
Aborting commit due to empty commit message.
redacteduser@ubuntu2004desktop:/tmp/RedactedRepo$ chmod -x .git/hooks/commit-msg
redacteduser@ubuntu2004desktop:/tmp/RedactedRepo$ git commit
hint: The '.git/hooks/commit-msg' hook was ignored because it's not set as executable.
hint: You can disable this warning with `git config advice.ignoredHook false`.
Aborting commit due to empty commit message.
redacteduser@ubuntu2004desktop:/tmp/RedactedRepo$ 

```
Note: I ran this on my main machine which is why it is on 20.04 however I do have a PPA installed that enables me to use to date versions of git. 
### firefox
#### user.js
See [file overwrite](../file-overwrite/README.md#Firefox%20Profile%20Redirection) The difference here is that changes can be made while the browser is open. 