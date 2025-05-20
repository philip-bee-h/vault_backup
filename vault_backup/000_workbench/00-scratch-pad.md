




https://www.seafile.com/en/home/


https://planka.app/


https://github.com/TriliumNext/Notes

Zed editor

Clean Mac. https://makb.medium.com/how-i-freed-over-60gb-of-system-storage-on-my-mac-and-you-can-too-83f295c2beed


https://github.com/waydabber/BetterDisplay


https://github.com/huginn/huginn


https://huly.io/

https://twenty.com/


https://beej.us/guide/bggit/

### brew clean up

```sh
function bruc() {
  echo "Starting Homebrew maintenance..."

  # Update Homebrew
  if brew update; then
    echo "Homebrew updated successfully."
  else
    echo "Error: Homebrew update failed." >&2
    return 1
  fi

  # Upgrade installed packages
  if brew upgrade; then
    echo "Homebrew packages upgraded successfully."
  else
    echo "Error: Homebrew upgrade failed." >&2
    return 1
  fi

  # Cleanup old versions
  if brew cleanup; then
    echo "Homebrew cleanup completed successfully."
  else
    echo "Error: Homebrew cleanup failed." >&2
    return 1
  fi

  echo "Homebrew maintenance completed."
}




OLD
#!/bin/bash

# Homebrew Update Script
# This script updates Homebrew, upgrades installed formulas, and then cleans up old versions.

# Update Homebrew to get the latest list of available formulas
echo "Updating Homebrew..."
brew update

# Upgrade any installed formulas to their latest versions
echo "Upgrading installed formulas..."
brew upgrade

# Clean up old versions of formulas
echo "Cleaning up old versions..."
brew cleanup

echo "Homebrew update and cleanup completed."
```



https://www.self.so/

https://www.getzola.org/documentation/themes/overview/

https://routinehub.co/

https://www.macstories.net/shortcuts/
---

Cody was going o get exisiting firewall rooms from Brain 

- do we still need what we currently have
- 