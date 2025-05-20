---
title: "macos-ssh-key-generation-guide"
created: 2024-10-28
modified: 2024-10-28
type: setup-guide
system: macos
tags:
  - ssh
  - key-generation
  - macos
  - security
---

### Creating SSH Keys on a Mac (User: hawkid)

#### Step 1: Open Terminal
- Locate the Terminal application in your `/Applications/Utilities` folder or use Spotlight search (`Cmd + Space`) and type "Terminal".

#### Step 2: Generate the SSH Key
- In the Terminal, execute the following command to generate a new SSH key:
  ```bash
  ssh-keygen -t ed25519 -C "<hawkID>@uiowa.edu"
  ```
  - **Explanation**: `ssh-keygen` starts the key generation process, `-t ed25519` specifies the type of key, and `-C` adds a label to the key with your email for identification.

#### Step 3: Save the SSH Key
- When prompted to "Enter file in which to save the key," enter:
  ```bash
  /Users/<hawkID>/.ssh/id_ed25519
  ```
  - Press `Enter` to confirm the path.

#### Step 4: Set a Passphrase (Optional but Recommended)
- Enter a passphrase when prompted for enhanced security or press `Enter` to skip.

#### Step 5: Verify the SSH Key Creation
- Confirm the SSH key has been created by listing the contents of your `.ssh` directory:
  ```bash
  ls -la /Users/hawrth/.ssh/
  ```

#### Step 6: Copy the Public Key to Clipboard
- Use the `cat` command to display your new public key and copy it to your clipboard for use:
  ```bash
 cat /Users/<hawkID>/.ssh/id_ed25519.pub | pbcopy
  ```
  - Manually select and copy the output of this command if needed, or use macOS clipboard utilities to copy the output directly to the clipboard.

#### Step 7: Add the SSH Key to the SSH Agent
- Start the SSH agent in the background:
  ```bash
  eval "$(ssh-agent -s)"
  ```
- Add your SSH private key to the SSH agent:
  ```bash
  ssh-add /Users/<hawkID>/.ssh/id_ed25519
  ```

#### Step 8: Deploy the SSH Key
- You can now add the public key to any server that supports SSH key authentication or services like GitHub that use SSH keys for secure access.
- Typically, this involves appending the public key to the `~/.ssh/authorized_keys` file on the server or through user settings on platforms that support SSH key uploads.

### Conclusion
This process creates a secure, encrypted connection method for user `hawrth` without the need to enter a password every time. Remember, the security of your SSH key relies significantly on not sharing your private key (`id_ed25519`) and keeping your passphrase secure if you set one.


### Notes

**Changing passphrase**

```shell
ssh-keygen -p -f ~/.ssh/id_ed25519
```

- follow the prompts (leave empty for no passphrase)-
