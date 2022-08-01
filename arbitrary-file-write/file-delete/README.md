# Delete Arbitrary File
* as root or user you can...
	* delete a file

## Test Notes
* `rm /etc/shadow`
	* system broken. tested: `passwd, sudo, login, su`
* `rm /etc/group`
	* system broken. tested: `sudo, login, su`
* `rm /etc/sudoers`
	* only `sudo` broken (also tested empty file)

### FreeBSD

* `rm /etc/spwd.db`
	* system broken. tested: `passwd, sudo, login, su`
* `rm /etc/master.passwd`
	* only `passwd` broken
* `rm /etc/pwd.db`
	* only `passwd` broken
