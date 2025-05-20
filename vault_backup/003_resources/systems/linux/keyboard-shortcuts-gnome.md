---
title: keyboard-shortcuts-gnome
created: 2024-12-03
type: process
system: 
tags:
  - process
  - procedure
  - linux
  - dotfiles
related_docs:
---
# **Process Overview**

- _This guide explains how to create and manage custom keyboard shortcuts in GNOME. Custom shortcuts help streamline access to frequently used applications and commands._

# **Requirements**

- **Access:**
	- GNOME desktop environment with access to system settings.
	- Ability to execute commands in a terminal.
- **Tools:**
	- GNOME **Settings**.
	- Terminal (optional, for advanced configuration).
- **References:**
	- `/usr/share/applications` for application launcher files.

# **Steps**

- **Identify the Application Command**  
	- _Determine the command to launch your application._

- Open a terminal and run:

```bash
grep -r "Exec=" /usr/share/applications/
```

- Locate the `Exec=` line for your desired application.  
	- Example: For Firefox, the command is `firefox`.

- **Create the Custom Shortcut**  
	- _Set up the shortcut in GNOME Settings._
	- Open **Settings** → **Keyboard**.
	- Scroll down to **Custom Shortcuts** and click **Add Shortcut**.
	- Fill out the fields:
		- **Name:** Enter a descriptive name (e.g., "Firefox").
		- **Command:** Enter the application command (e.g., `firefox`).
		- **Shortcut:** Press the desired key combination (e.g., `Ctrl+Alt+F`).

- **Test the Shortcut**  
	- _Verify that the shortcut works._
	- Use the key combination to ensure it launches the application.

- **Advanced Configuration (Optional)**  
	- _View or modify shortcuts using `dconf`._
	- Run this command to see all custom shortcuts:
```bash
dconf dump /org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/
```

# **Troubleshooting**

- **Issue:** _Shortcut does not work._

- **Solution:**
	- Verify the command in the `.desktop` file or terminal.
	- Ensure the key combination is not already in use.
- **Issue:** _Application not launching._

- **Solution:**
	- Check the command syntax.
	- Run the command in a terminal to confirm it works.

This process enables you to set up quick and efficient access to your favorite tools in GNOME!




```sh
❯ dconf dump /org/gnome/settings-daemon/plugins/media-keys/custom-keybindings/
[custom0]
binding='<Super>v'
command='/usr/share/code/code'
name='vs code'

[custom1]
binding='<Super>p'
command='/opt/1Password/1password'
name='1password'

[custom2]
binding='<Super>x'
command='/opt/sublime_text/sublime_text'
name='sublime text 4'

[custom3]
binding='<Shift><Super>Return'
command='alacritty'
name='alacritty'

[custom4]
binding='<Super>Return'
command='ptyxis'
name='ptyxis'

[custom5]
binding='<Super>b'
command='firefox'
name='firefox'

[custom6]
binding='<Super>f'
command='nautilus'
name='files'

[custom7]
binding='<Super>t'
command='gnome-text-editor'
name='text editor'
```