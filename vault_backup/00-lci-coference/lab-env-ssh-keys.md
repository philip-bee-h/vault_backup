---
title: lab-env-ssh-keys
created: 2025-02-09
type: process
system: 
tags:
  - process
  - procedure
  - uiowa
  - conference-lci
related_docs:
---


## **1. Generate the `lci_keys` SSH Key Pair**
On your **local machine**, run:

```bash
ssh-keygen -t ed25519 -f ~/.ssh/lci_keys -C "lci_conference"
```

- **`-t ed25519`** → Uses the modern **ED25519** key type (recommended).
- **`-f ~/.ssh/lci_keys`** → Saves the private key as `lci_keys` and the public key as `lci_keys.pub`.
- **`-C "lci_conference"`** → Adds a comment to identify the key.

**If you prefer ECDSA instead, use:**
```bash
ssh-keygen -t ecdsa -b 521 -f ~/.ssh/lci_keys -C "lci_conference"
```

---

## **2. Copy the Public Key to the Head Node**
Send the public key to your **Rocky Linux head node**:

```bash
ssh-copy-id -i ~/.ssh/lci_keys.pub rocky@<head-node-ip>
```
or manually:

```bash
cat ~/.ssh/lci_keys.pub | ssh rocky@<head-node-ip> "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```

---

## **3. Use the `lci_keys` for SSH Access**
Now, explicitly specify the **new key** when connecting:

```bash
ssh -i ~/.ssh/lci_keys rocky@<head-node-ip>
```

To make this the **default** key for this node, add an entry to your SSH config file (`~/.ssh/config`):

```bash
nano ~/.ssh/config
```
Add the following lines:

```plaintext
Host lci-headnode
    HostName <head-node-ip>
    User rocky
    IdentityFile ~/.ssh/lci_keys
```

Now, you can simply connect using:

```bash
ssh lci-headnode
```

---

## **4. Verify and Troubleshoot**
- Run:
  ```bash
  ssh -i ~/.ssh/lci_keys rocky@<head-node-ip>
  ```
  If it works without a password, you're all set!
- If there are issues, check:
  ```bash
  chmod 600 ~/.ssh/lci_keys
  chmod 600 ~/.ssh/lci_keys.pub
  ```

---

