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
    * Say goodbye to your system! on ubuntu 22.04, after system checks filesystem, it hangs and writes a total of 6 "[FAILED] Failed to start Network Name Resolution" messages. The reason for the lack of other errors is due to "quiet splash" being a default for ubuntu installs, quiet -> meaning less log output and splash -> hide messages behind a splash screen. After a few minutes we get one more message: "[FAILED] Failed to start Failure handling of the snapd snap. " which sounds quite confusing. 

### bash shell profile fallbacks
See shell profile section in file-create, deleting one file will cause the shell to look for another file. Use case: the system sets the path in one of the shell profiles so it uses an up to date version of some programming language, but we bypass that by deleting the profile. Both dash and bash fallback to `.profile`
`/home/user/.bash_profile`, `/home/user/.bash_login`, and `/home/user/.profile` for a specific user. Replace `~` with `/home/user` with `/root` to override for root. 


