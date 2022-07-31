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
* javaarchive/smashmaster: `/usr/include/` allows us to inject extrd code when some other code is compiling. 