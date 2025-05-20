---
title: "unix-command-reference-clipboard-utilities-pbcopy-xclip-wl-clipboard"
created: 2024-10-28
modified: 2024-10-28
type: command-reference
system: unix
tags:
  - commands
  - clipboard
  - pbcopy
  - xclip
  - wl-clipboard
  - unix
---
### Unix Command Reference: Clipboard Utilities (`pbcopy`, `xclip`, and `wl-clipboard`)

#### **`pbcopy` (macOS)**
- **Description**: `pbcopy` is a command-line utility native to macOS that allows users to copy text from the standard input into the clipboard, enabling them to paste it elsewhere.
- **Availability**: Built-in with macOS; no installation required.
- **Usage Example**: To copy the contents of a file to the clipboard:
```bash
cat /path/to/file | pbcopy
```
- **Common Use Case**: Often used to copy SSH keys, configuration details, or script outputs directly from the terminal to the clipboard.

#### **`xclip` (Linux, X11)**
- **Description**: `xclip` is a command-line interface to the X11 clipboard. It provides a method to copy text and images from and into the clipboard of a Unix-based system using the X server.
- **Availability**: Not included by default on most Linux distributions; requires installation.
- **Installation**:
- Debian/Ubuntu: `sudo apt-get install xclip`
- Fedora: `sudo yum install xclip`
- Arch Linux: `sudo pacman -S xclip`
- **Usage Example**: To copy the contents of an SSH public key to the clipboard:
```bash
cat ~/.ssh/id_rsa.pub | xclip -selection clipboard
```
- **Common Use Case**: Used for copying files, command outputs, or direct text inputs into the X server's clipboard, which can be pasted into other X applications.

#### **`wl-clipboard` (Linux, Wayland)**
- **Description**: `wl-clipboard` includes `wl-copy` and `wl-paste`, tools for clipboard management in Wayland environments.
- **Availability**: Requires installation; not built-in.
- **Installation**:
- Debian/Ubuntu: `sudo apt install wl-clipboard`
- Fedora: `sudo dnf install wl-clipboard`
- Arch Linux: `sudo pacman -S wl-clipboard`
- **Usage Example**:
- **Copying**: 
```bash
cat ~/.ssh/id_rsa.pub | wl-copy
```
- **Pasting**:
```bash
wl-paste > output.txt
```
- **Common Use Case**: These tools are optimized for Wayland's clipboard management, suitable for copying and pasting text between Wayland applications or terminals.

### Key Differences and Considerations
- **Platform Specificity**:
- `pbcopy` is specific to macOS.
- `xclip` is designed for X11-based Linux systems.
- `wl-clipboard` (including `wl-copy` and `wl-paste`) is specific to Wayland-based Linux systems.
- **Functionality**:
- Each tool is used to bridge command-line operations with graphical interface applications through clipboard management but tailored to different system architectures.
- **Installation**:
- `pbcopy`: No installation required on macOS.
- `xclip` and `wl-clipboard`: Require installation on Linux.

### Conclusion
Understanding the right clipboard utility to use based on your operating system and environment (X11 vs. Wayland) is crucial for efficient clipboard management. Each tool enhances productivity by allowing users to handle clipboard content directly from the command line.
