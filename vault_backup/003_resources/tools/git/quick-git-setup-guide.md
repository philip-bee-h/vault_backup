---
title: "quick-git-setup-guide"
created: 2024-10-28
modified: 2024-10-28
type: resource
category:
  - tool
tags:
  - git
  - setup
  - commands
  - version-control
---


### Quick Git Setup Guide

1. **Install Git**
```bash
sudo apt-get install git
```

2. **Configure Git (First Time Setup)**
```bash
git config --global user.name "Your Name"
git config --global user.email "youremail@example.com"
```

3. **Create a New Directory for Your Project**
```bash
mkdir your_project_name
cd your_project_name
```

4. **Initialize a New Git Repository**
```bash
git init
```

5. **Add Files to the Repository**
```bash
git add .
```

6. **Commit the Files**
```bash
git commit -m "Initial commit"
```

7. **Connect to a Remote Repository (If you have one)**
```bash
git remote add origin <repository URL>
```

8. **Push Your Commit to the Remote Repository**
```bash
git push -u origin main
```

