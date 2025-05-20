---
title: ubuntu-ssh-runbook
created: 2024-10-28
modified: 2024-10-28
type: runbook
system: macos
tags:
  - setup
  - configuration
  - ssh
  - git
  - ubuntu
  - linux
  - runbook
---

# System Setup and Configuration Run Book

## Objective
This run book guides the user through updating the system, installing essential packages, configuring SSH, and cloning a private repository.

## Prerequisites
- Ensure you have administrative access to the system.
- Have your work email ready for SSH key generation.

## Procedures

### Step 1: Setup SSH Configuration
Ensure the SSH directory exists and create a new SSH configuration file.

```bash
mkdir -p ~/.ssh
touch ~/.ssh/config
```

### Step 2: Generate SSH Keys
Generate an SSH key pair using your work email address.

```bash
ssh-keygen -t ed25519 -C "hawrth@uiowa.edu" -f ~/.ssh/id_ed25519
```

### Step 3: Add SSH Key to GitLab
Display the generated public SSH key. You will need to manually add this key to your GitLab account.

```bash
cat ~/.ssh/id_ed25519.pub
```

### Step 4: Start SSH Agent and Add Private Key
Start the SSH agent and add your new SSH private key.

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### Step 5: Secure SSH Directory and Configuration
Set the proper permissions for the SSH directory and configuration file.

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/config
```

### Step 7: Clone Private Repository
Clone the required repository using SSH.

```bash
git clone git@git.uiowa.edu:7999/hawrth/dots.git
echo "Repository cloned successfully!"
```

This format maintains clarity and ensures each step is easy to follow. If you have any specific tweaks or additional requirements, feel free to let me know!