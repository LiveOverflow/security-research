# Create Arbitrary File
* as root or user you can...
	* create an empty file
	* create file and control content (eg. arbitrary file upload)
	* create file and partially control content (eg. inject data into a config file)
		* control filename/path
		* partially control filename or path (eg. fixed extension)

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
#### Login Shells
`--login` flag. 
* `/etc/profile` executed for all users
* First found of `~/.bash_profile`, `~/.bash_login`, and `~/.profile` is then executed
#### Non-login shell
* `/etc/bash.bashrc` executed for all users 
* `~/.bashrc` for user specific.  

### Eicar antimalware testfile
You can try to obtain it [here](https://www.eicar.org/download-anti-malware-testfile/). The most it'll do is trigger deletion by antivirus especially those with real time detection but an be helpful for triggering weird errors in some applications.  

### git hooks
One may be able to sneak in a [git hook](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks#_git_hooks) in the `.git` of a git repo to execute any code of their choosing when a git action happens, however this requires the file to marked executable. 