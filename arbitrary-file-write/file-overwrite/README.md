# Arbitrary File Overwrite

* as root or user you can overwrite a file and ...
	* fully control content
	* partially control content (eg. parts of a logfile)
	* none (empty file)

### pam
If we know enough about the target os we can overwrite their user authentication system logic with a backdoor. Here's an [example](https://infosecwriteups.com/creating-a-backdoor-in-pam-in-5-line-of-code-e23e99579cd9) that returns a successful login for a specific hardcoded password. 

### firefox (linux)
In `~/.mozilla/firefox` by default, `installs.ini` and `profiles.ini` tell firefox which profiles it has and which one to select as default. We can change the profile folder to something we know that probaly exists like the two folders "Crash Reports" and "Pending Rings", and then configure settings there. The hex bit is from a special hash firefox uses called `cityhash` which the install path is processed with. 
#### Sample installs.ini

```
[4F96D1932A9F858E]
Default=yxaliytr.default
Locked=1
```

#### Sample profiles.ini
```
[Install4F96D1932A9F858E]
Default=yxaliytr.default
Locked=1

[Profile0]
Name=default
IsRelative=1
Path=yxaliytr.default
Default=1

[General]
StartWithLastProfile=1
Version=2
```

> The installs.ini file stores a hash of the installation path and locks a profile for this specific path to prevent other Firefox installations from using it. The profiles.ini file still stores all registered profiles and their paths and can include the data stored in installs.ini.

> Profile-per-install uses a hash of the install directory to identify different
installs of Firefox. This exposes the existing cityhash generated hash from
nsXREDirProvider and makes it available on all platforms.



[Source](https://support.mozilla.org/en-US/questions/1262122#:~:text=The%20installs.,the%20data%20stored%20in%20installs.) (need a better one)

[Profile Hashing Source](http://forums.mozillazine.org/viewtopic.php?p=14880915)

[Mozilla Bugzilla mentioning Cityhash](https://bugzilla.mozilla.org/show_bug.cgi?id=1474285)

On most systems the installl location can be guessed (`/usr/bin/firefox` for mine). However additional complexities may occur, for example Ubuntu is using snaps for packaging applications a lot more in the newer versions (TODO: Research firefox path in snap, I'm guessing the config file location will be in at least `$HOME/snap/firefox`).

TODO 2: POC of going from install directory to cityhash-ed hex digest. 