---
title: hl-ssh-key-gen-setup
created: 2024-11-16
type: process
system: 
tags:
  - process
  - procedure
  - homelab
  - ssh
related_docs:
---
# Guide

#### **scp/Copy the Keys to the Current Host**
1. Copy the private key:
```bash
scp pbh@192.168.10.6:/home/pbh/.ssh/homelab_key ~/.ssh/
```

2. Copy the public key:
```bash
scp pbh@192.168.10.6:/home/pbh/.ssh/homelab_key.pub ~/.ssh/
```

3. This should be for both
```sh
scp pbh@192.168.10.6:/home/pbh/.ssh/homelab_key{,.pub} ~/.ssh/
```
### **Simple Guide: Generate and Copy SSH Keys**
---

#### **Step 1: SSH into Your Raspberry Pi (Key Generator)**
1. Connect to your Raspberry Pi (192.168.10.6):
```bash
ssh pbh@192.168.10.6
```

---

#### **Step 2: Generate an SSH Key**
1. Generate an Ed25519 SSH key:
```bash
ssh-keygen -t ed25519 -f ~/.ssh/homelab_key -C "philipbhaworth@gmail.com"
```
   - Press **Enter** to accept the default location (`~/.ssh/homelab_key`).
   - Leave the passphrase blank (or set one if needed).

2. Verify the keys:
   - Private key: `~/.ssh/homelab_key`
   - Public key: `~/.ssh/homelab_key.pub`

---

#### **Step 3: Copy the Public Key to Add to GitHub**
1. Display the public key:
```bash
cat ~/.ssh/homelab_key.pub
```

2. Copy the entire output, which will look like this:
```plaintext
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAI...philipbhaworth@gmail.com
```

3. Add the public key to GitHub:
   - Go to **GitHub Settings** > **SSH and GPG Keys** > **New SSH Key**.
   - Paste the public key into the **Key** field.
   - Give it a descriptive title, e.g., `Homelab Key`.

---


#### **Step 5: Set Correct Permissions**
1. Secure the private key:
```bash
chmod 600 ~/.ssh/homelab_key
chmod 700 ~/.ssh
```

2. Ensure the `.ssh` directory has the right permissions:
```bash
chmod 700 ~/.ssh
```

---

#### **Step 6: Add the SSH Key to the Agent**
1. Start the SSH agent:
```bash
eval "$(ssh-agent -s)"
```

2. Add the private key:
```bash
ssh-add ~/.ssh/homelab_key
```

---

#### **Step 7: Test the Key**
1. Verify the SSH key works by connecting to GitHub:
```bash
ssh -T git@github.com
```
   You should see:
```plaintext
Hi philipbhaworth! You've successfully authenticated, but GitHub does not provide shell access.
```

---

This updated guide now includes how to copy the public key for adding it to GitHub. Let me know if there's anything else you'd like clarified!

# **Runbook**

### homelab_key
```sh
scp pbh@192.168.10.21:/home/pbh/.ssh/homelab_key{,.pub} ~/.ssh/
---
ssh-keygen -t ed25519 -f ~/.ssh/homelab_key -C "philipbhaworth@gmail.com"

cat ~/.ssh/homelab_key.pub

chmod 600 ~/.ssh/homelab_key
chmod 700 ~/.ssh

eval "$(ssh-agent -s)"

ssh-add ~/.ssh/homelab_key

ssh -T git@github.com
```

### k8s_key

```sh
scp pbh@192.168.20.10:/home/pbh/.ssh/k8s_key{,.pub} ~/.ssh/
---
ssh-keygen -t ed25519 -f ~/.ssh/k8s_key -C "philipbhaworth@gmail.com"

cat ~/.ssh/k8s_key.pub

chmod 600 ~/.ssh/k8s_key
chmod 700 ~/.ssh

eval "$(ssh-agent -s)"

ssh-add ~/.ssh/k8s_key

ssh -T git@github.com
```
# **Troubleshooting**
---

**Error**
```sh
dotfiles on  main [⇡] 
❯ git push git@github.com: Permission denied (publickey). 
fatal: Could not read from remote repository.
```
**Fix**
```sh
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/homelab_key
```

---

