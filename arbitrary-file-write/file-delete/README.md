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

`${LOCALBASE}` is the path where ports and packages are installed on the system. By default it is `/usr/local`.

* `rm /etc/spwd.db`
	* system broken. tested: `passwd, sudo, login, su`
	* system also broken after reboot. tested: `login`
* `rm /etc/master.passwd`
	* only `passwd` broken. still broken after reboot
* `rm /etc/pwd.db`
	* only `passwd` broken. still broken after reboot
* `rm /etc/group`
	* `passwd, sudo, login, su` keep working as expected, even after reboot
* `rm ${LOCALBASE}/etc/sudoers`
	* only `sudo` broken (also tested empty file). still broken after reboot
