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