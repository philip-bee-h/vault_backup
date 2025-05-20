---
title: lazygit-reference-note
created: 2024-12-03
type: command-reference
system: 
tags:
  - commands
  - cli
  - git
  - terminal
related_runbooks:
---
Here's a quick cheat sheet for **lazygit**, covering basic commands and useful practices:

---

### **Basic Navigation and Commands**

1. **Open LazyGit**:  
    Run `lazygit` in your terminal while in a Git repository.
2. **Key Sections in LazyGit Interface**:
    - **Files Panel**: Shows unstaged, staged, and untracked files.
    - **Commits Panel**: Displays local and remote commits.
    - **Branches Panel**: View and switch branches.
    - **Log Panel**: Shows commit history.
    - **Stash Panel**: Manage stashed changes.

---

### **Basic Git Actions**

#### **Fetch**:
- Press **F** to fetch changes from the remote repository.
#### **Pull**:
- Press **Shift + F** to pull changes (fetch + merge).
#### **Add All**:
- Press **a** on a file to stage it.
    - **TIP**: Press **a** in the Files panel to stage all files.
#### **Commit**:
1. Stage the changes (see above).
2. Press **c** to open the commit menu:
    - Write your commit message and press **Enter** to commit.
#### **Push**:
- Press **P** to push commits to the remote repository.

---

### **Other Useful Actions**

#### **Switch Branches**:

- Press **B** to view branches and switch between them.

#### **Create a New Branch**:

- Press **Shift + N** in the Branches panel.

#### **Merge a Branch**:

- Select the target branch and press **M**.

#### **Rebase**:

- Select a branch and press **Shift + R** to rebase.

#### **Stash Changes**:

- Press **S** to stash unstaged changes.
- Press **Shift + S** to pop or apply a stash.

#### **View File Diff**:

- Highlight a file and press **Enter** to view the diff.

#### **Undo Last Action**:

- Press **U** to undo the last commit or stage/unstage action.

#### **Discard Changes**:

- Highlight a file and press **d** to discard changes.

---

### **Workflow Example: Pull, Update, Commit, and Push**

1. **Fetch Changes**: Press **F** to fetch remote changes.
2. **Pull Changes**: Press **Shift + F** to pull them.
3. **Stage Files**: Highlight files and press **a**, or **Shift + A** to stage all.
4. **Commit Changes**: Press **c**, write a message, and commit.
5. **Push to Remote**: Press **P** to push your commits.

---

### **General Tips**

- **Search and Filter**: Press **/** to search in the current panel.
- **Help Menu**: Press **?** to view keybindings and shortcuts in LazyGit.
- **Refresh**: Press **r** to refresh the interface if something feels out of sync.
- **Customizing LazyGit**: You can modify its behavior by editing the `~/.config/lazygit/config.yml`.

---

### **Practice Suggestions**

1. **Staging Changes**: Stage individual hunks instead of full files for fine control.
2. **Rebasing**: Use LazyGit's rebase interface for cleaner history when integrating changes.
3. **Explore Logs**: Use the Log panel to inspect commit history interactively.
4. **Experiment with Stash**: Practice stashing and unstashing to save your work in progress.
