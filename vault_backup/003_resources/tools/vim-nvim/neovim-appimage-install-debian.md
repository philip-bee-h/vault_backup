---
title: neovim-appimage-install-debian
created: 2024-11-01
modified: 2024-11-01
type: process
system:
  - linux
tags:
  - process
  - procedure
  - nvim
  - install-guide
  - debian
  - ubuntu
related_docs:
---


### Installation Steps:

1. **Check Curl Installation:**
   Ensure that Curl is installed on your system. If not, install it using:
```bash
sudo apt-get install curl
```
   
2. **Download Neovim AppImage:**
   Use Curl to download the Neovim AppImage:
```bash
curl -LO https://github.com/neovim/neovim/releases/latest/download/nvim.appimage
```

3. **Add Execution Permission:**
   After downloading, grant execution permission to the Neovim AppImage:
```bash
chmod u+x nvim.appimage
```

4. **Test Neovim AppImage:**
   Verify that the Neovim AppImage works:
```bash
./nvim.appimage
```

5. **Move AppImage to /usr/local/bin:**
   Move the Neovim AppImage to the /usr/local/bin directory:
```bash
sudo mv nvim.appimage /usr/local/bin/nvim
```

6. **Open Neovim:**
   You can now open Neovim by simply running:
```bash
nvim
```

### Updating Neovim:

To update Neovim, repeat the installation steps with the latest version of the AppImage.

### Uninstalling Neovim:

To uninstall Neovim, delete the AppImage binary:
```bash
sudo rm -R /usr/local/bin/nvim
```

### Handling Errors:

If you encounter the error "dlopen(): error loading libfuse.so.2", you need to install Fuse and libfuse2:
```bash
# Ubuntu
sudo apt-get install fuse libfuse2

# Fedora
dnf install fuse
```

Ensure to follow the specific instructions for your Linux distribution.


### Now to config.....

[[setting-up-neovim-config]]

