---
title: homebrew-brewfile-reference
created: 2024-12-10
type: command-reference
system: 
tags:
  - commands
  - homebrew
  - macos
  - terminal
related_runbooks:
---


## What is a Brewfile?
A Brewfile is a plain text file that lists all Homebrew packages, casks, and Mac App Store apps on your system. It allows you to back up and automate the reinstallation of these tools on another system or during a rebuild.

---

## Steps to Create and Use a Brewfile

### 1. Dump the Current Homebrew Configuration into a Brewfile
This step generates a Brewfile with all currently installed Homebrew packages, casks, and MAS apps.

1. Open your terminal.
2. Run the following command to create the Brewfile:
```bash
brew bundle dump --file=~/dotfiles/scripts/Brewfile
```
   - `--file=~/dotfiles/scripts/Brewfile`: Specifies the location to save the Brewfile. Adjust the path if needed.
- To overwrite the exisiting file run...
```sh
brew bundle dump --file=~/dotfiles/scripts/Brewfile --force
```


**Note**: This command will include all packages, casks, and MAS apps currently installed via Homebrew.

### 2. Install Packages from a Brewfile
To use the Brewfile to install the listed packages, follow these steps:

1. Ensure you have Homebrew installed. If not, install it using:
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

2. Run the following command to install packages from the Brewfile:
   ```bash
   brew bundle --file=~/dotfiles/scripts/Brewfile
   ```
   - This command reads the Brewfile and installs the listed items.

**Optional Flags**:
- `--verbose`: Displays detailed installation progress.
- `--no-upgrade`: Prevents upgrading existing items, only installs missing ones.
- `--cleanup`: Removes items not listed in the Brewfile.

Example:
```bash
brew bundle --file=~/dotfiles/scripts/Brewfile --verbose --cleanup
```

### 3. Ensure MAS (Mac App Store) Apps Are Installed
MAS apps require the `mas` CLI, which should be included in the Brewfile. If not already installed:

1. Install `mas` manually:
   ```bash
   brew install mas
   ```

2. Log in to the Mac App Store using:
   ```bash
   mas signin <your-apple-id>
   ```
   Replace `<your-apple-id>` with your Apple ID email address.

3. When running the `brew bundle` command, it will automatically install the MAS apps listed in the Brewfile.

---

## Summary of Commands

1. **Dump current configuration into a Brewfile**:
   ```bash
   brew bundle dump --file=~/dotfiles/scripts/Brewfile
   ```

2. **Install packages from the Brewfile**:
   ```bash
   brew bundle --file=~/dotfiles/scripts/Brewfile
   ```

3. **Optional flags for installation**:
   ```bash
   brew bundle --file=~/dotfiles/scripts/Brewfile --verbose --cleanup
   ```

4. **Ensure MAS CLI is installed and logged in**:
   ```bash
   brew install mas
   mas signin <your-apple-id>
   ```

