---
title: fzf-setup-macos
created: 2024-12-06
type: process
system: 
tags:
  - process
  - procedure
  - cli
  - commands
  - terminal
  - zsh
  - bash
  - linux
  - macos
related_docs:
---
### **fzf Setup Guide for macOS, Ubuntu, and Fedora**

This guide will help you install **fzf** and configure useful aliases and scripts for macOS, Ubuntu, and Fedora.

---

## **Installation**

### **macOS**
1. Install **fzf** using Homebrew:
   ```bash
   brew install fzf
   ```

2. Run the fzf install script to enable keybindings and shell completion:
   ```bash
   $(brew --prefix)/opt/fzf/install
   ```

---

### **Ubuntu**
1. Install fzf from the package manager:
   ```bash
   sudo apt update
   sudo apt install fzf
   ```

2. Verify the installation:
   ```bash
   fzf --version
   ```

---

### **Fedora**
1. Install fzf from the package manager:
   ```bash
   sudo dnf install fzf
   ```

2. Verify the installation:
   ```bash
   fzf --version
   ```

---

### **Manual Installation (For All Distros)**
If you prefer the latest version:
1. Clone the fzf repository:
   ```bash
   git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
   ```

2. Run the install script:
   ```bash
   ~/.fzf/install
   ```

3. Select options for keybindings and shell completion during the installation.

---

## **Configuration**

### **Enable fzf Keybindings**
Add the following line to your shell configuration file:

- **For Zsh (`~/.zshrc`)**:
  ```bash
  source <(fzf --zsh)
  ```

- **For Bash (`~/.bashrc`)**:
  ```bash
  source /usr/share/doc/fzf/examples/key-bindings.bash
  ```

Reload your shell:
```bash
source ~/.zshrc   # For Zsh
source ~/.bashrc  # For Bash
```

---

## **Aliases and Scripts**

### **1. Fuzzy File Finder**
Search and preview files:
```bash
alias ff="fzf --preview 'bat --style=numbers --color=always {} || cat {}'"
```
> Requires `bat` for syntax-highlighted previews (install via `brew`, `apt`, or `dnf`).

Search and preview file then open in vim
```shell
alias ff="fzf --preview 'bat --style=numbers --color=always {} || cat {}' --bind 'enter:execute(vim {})'"
```

---

### **2. Directory Navigation**
Search for directories and `cd` into them:
```bash
alias fd="cd \$(find . -type d | fzf)"
```

---

### **3. Git Integration**
- **Checkout a Git Branch**:
  ```bash
  alias fgb="git checkout \$(git branch --all | fzf)"
  ```

- **Search and Open Changed Files**:
  ```bash
  alias gf="git diff --name-only | fzf | xargs -o vim"
  ```

- **Search Git Log**:
  ```bash
  alias gl="git log --oneline | fzf | awk '{print \$1}' | xargs git show"
  ```


---

### **4. Command History**
Search and re-run commands from your shell history:
```bash
alias fh="history | fzf | awk '{print \$2}' | xargs zsh -c"
```

---

### **5. Kill a Process**
Search and kill running processes:
```bash
alias fkill="ps -ef | fzf | awk '{print \$2}' | xargs kill -9"
```

---

### **6. Finder Integration (macOS Only)**
Search for files and open them in Finder:
```bash
alias ffinder="open \$(find . -type f | fzf)"
```

---

### **7. Directory Bookmarks**
Bookmark frequently used directories and navigate to them:
```bash
# Save current directory to a bookmarks file
alias mark="pwd >> ~/.fzf-bookmarks"

# Jump to a bookmarked directory
alias j="cat ~/.fzf-bookmarks | fzf | xargs cd"
```

---

### **8. SSH Connection Manager**
Select and connect to SSH hosts:
```bash
alias fssh="cat ~/.ssh/config | grep 'Host ' | awk '{print \$2}' | fzf | xargs ssh"
```

---

## **fzf Environment Variables**
Set defaults for fzf commands:
```bash
export FZF_DEFAULT_COMMAND="find . -type f"
export FZF_CTRL_T_COMMAND="$FZF_DEFAULT_COMMAND"
export FZF_ALT_C_COMMAND="find . -type d"
```

- **Effect**:
  - `Ctrl+T`: Trigger file search.
  - `Alt+C`: Trigger directory search.

---

## **Applying Changes**
1. Open your shell configuration file:
   - Zsh: `~/.zshrc`
   - Bash: `~/.bashrc`

2. Add the aliases and environment variables from above.

3. Save and reload the shell:
   ```bash
   source ~/.zshrc   # For Zsh
   source ~/.bashrc  # For Bash
   ```

---

## **Optional Tools**
Install these tools for enhanced fzf functionality:
- **`bat`** (Preview files with syntax highlighting):
  - macOS: `brew install bat`
  - Ubuntu: `sudo apt install bat`
  - Fedora: `sudo dnf5 install bat`
- **`rg` (ripgrep)** (Fast text searching):
  - macOS: `brew install ripgrep`
  - Ubuntu: `sudo apt install ripgrep`
  - Fedora: `sudo dnf5 install ripgrep`

---

### **Next Steps**
- Experiment with these aliases and customize them to your workflow.
- For more advanced configurations, check the [fzf GitHub repository](https://github.com/junegunn/fzf).