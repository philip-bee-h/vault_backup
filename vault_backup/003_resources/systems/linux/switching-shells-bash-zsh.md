---
type: reference
refrenceName: switching-shells-bash-zsh
tags: 
source: "{{source}}"
date: 2024-10-19
---

## Switching Between `bash` and `zsh` as the Default Shell (Fedora)

### 1. **Check Available Shells**
To view the list of available shells:
```bash
cat /etc/shells
```
Ensure both `/bin/bash` and `/bin/zsh` are listed.

### 2. **Set `zsh` as the Default Shell**
To change the default shell to `zsh`, use:
```bash
chsh -s /bin/zsh
```
*Note:* You need to log out and log back in for the changes to take effect.

### 3. **Set `bash` as the Default Shell**
To revert to `bash` as the default shell:
```bash
chsh -s /bin/bash
```
*Note:* Log out and log back in to apply the change.

### 4. **Verify the Current Shell**
To verify which shell is currently set as the default:
```bash
echo $SHELL
```
This command will output the path of the active shell.
