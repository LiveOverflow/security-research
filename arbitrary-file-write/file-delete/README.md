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
* `rm /etc/fstab`
    * say goodbye to your system ig. most likely it'll boot to some rescue mode where you'll have to log in over a display or serial. 

### shell profile fallbacks
See shell profile section in file-create, deleting one file will cause the shell to look for another file. Use case: the system sets the path in one of the shell profiles so it uses an up to date version of some programming language, but we bypass that by deleting the profile. 