* Interesting file access to research
	* `gnome-shell`
		* `/home/user/.local/share/gnome-shell/application_state`
			* written and read by gnome-shell. Used for the Ubuntu search bar.
			* It's a XML document ([example](https://github.com/cedarli/gnome-shell/blob/master/application_state))
			* parsed by `g_markup_parse_context_parse` https://github.com/GNOME/glib/blob/main/glib/gmarkup.c#L1138
		* `/proc/<pid>/root/.flatpak-info`
			* Used for sandboxed desktop apps
			* config options
				* `[Instance] runtime-path=`
				* `[Application] runtime=`
				* can we abuse this?
		* `/home/user/.icons/Yaru/index.theme`
			* what are gnome shell themes?
	* `gnome-control-center`
		* `/home/user/.local/share/glib-2.0/schemas/gschemas.compiled`
	* `gnome-calculator`
		* `/home/user/.local/share/gnome-calculator/custom-functions`
			* It's accessed every time when you search for a program on Ubuntu.
	* `dbus-daemon`
		* what is dbus again?
		* `/home/user/.local/share/dbus-1/services`
			* why does it look for something in the user folder?
	* `gedit`
		* `/home/user/.local/share/gedit/plugins`
			* how to gedit plugins work?
		* `/home/user/.local/share/gedit/styles`
			* themes?

ideas with root file write:
* SaturnCorgi: `/lib/systemd/system-sleep/foo; gets executed by systemd-sleep`
* javaarchive/smashmaster: `/usr/include/` allows us to inject extrd code when some other code is compiling when it `#include`'s the now malacious file, because the include statement is like an automated copy-paste. 

ideas with root file append:

* javaarchive/smashmaster:
> If the environment variable ENV is set on entry to an interactive shell, or is set in the .profile of a login shell, the shell next reads commands from the file named in ENV...Therefore, a user should place commands that are to be executed only at login time in the .profile file, and commands that are executed for every interactive shell inside the ENV file.

Might be useful for crontab because environment variables were explored. 
Source: [dash manpages again](https://manned.org/dash). 
*  javaarchive/smashmaster: Use the eicar antimalware test file to [trip an antivirus that deletes files](https://github.com/LiveOverflow/security-research/pull/1#discussion_r934387982) thus escalating this to file delete! 
*  javaarchive/smashmaster: Change the system [SystemD](https://wiki.archlinux.org/title/Systemd#Editing_provided_units) units, overwrite and append work for the modifying the file directly, create file works for the replacement functionality, we just change "/usr/lib/systemd/system/unit" to "/etc/systemd/system/unit". User units are uncommon. The main problem is `systemctl daemon-reload` needs to occur or a system restart in order for changes to take effect. 