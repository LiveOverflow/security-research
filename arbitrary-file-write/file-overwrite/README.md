# Arbitrary File Overwrite

* as root or user you can overwrite a file and ...
	* fully control content
	* partially control content (eg. parts of a logfile)
	* none (empty file)

### pam
If we know enough about the target os we can overwrite their user authentication system logic with a backdoor. Here's an [example](https://infosecwriteups.com/creating-a-backdoor-in-pam-in-5-line-of-code-e23e99579cd9) that returns a successful login for a specific hardcoded password. 