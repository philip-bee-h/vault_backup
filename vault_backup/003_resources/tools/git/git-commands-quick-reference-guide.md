---
title: "git-commands-quick-reference-guide"
created: 2024-10-28
modified: 2024-10-28
type: command-reference
system: git
tags:
  - commands
  - git
  - quick-reference
  - version-control
---

### Git Commands Quick Reference Guide

#### Key Features

- **Interactive Learning**: Experiment with the commands as you read through.
- **Highlighted Terms**: Key terms are bolded on first mention, with definitions available in the Git glossary.
- **Insightful Descriptions**: Each command explanation provides deeper insights.
- **Further Reading**: Links for more in-depth exploration of complex topics.

---

#### Common Git Commands

1. **Initialize a Repository**
    
    - **Command**: `git init`
    - **Description**: Creates a hidden `.git` folder, enabling Git to track changes, transforming your folder into a "working directory."
2. **Clone a Repository**
    
    - **Command**: `git clone <repository-url>`
    - **Description**: Downloads a repository, including version history, from a URL (e.g., GitHub) to your local machine.
3. **Check Status**
    
    - **Command**: `git status`
    - **Description**: Displays your repository’s status, showing modified files and changes not yet staged.
4. **Create a New Branch**
    
    - **Command**: `git branch <new-branch-name>`
    - **Description**: Creates a new branch, allowing divergence from the main project.
5. **List Branches**
    
    - **Command**: `git branch`
    - **Description**: Lists all branches in the local repository, with `*` indicating the currently active branch.
6. **Switch Branches (New Method)**
    
    - **Command**: `git switch <branch-name>`
    - **Description**: Switches to a different branch without altering the working directory.
    - **Example**: `git switch feature-branch`
7. **Create and Switch to a New Branch**
    
    - **Command**: `git switch -c <new-branch-name>`
    - **Description**: Creates and switches to a new branch in a single command.
8. **View Changes Between Branches**
    
    - **Command**: `git diff <branch-name> <other-branch-name>`
    - **Description**: Shows line-by-line differences between branches.
9. **Stage Changes**
    
    - **Command**: `git add <files>`
    - **Description**: Prepares specific files for commit. `git add .` stages all changes.
10. **Commit Changes**
    

- **Command**: `git commit -m "Your message"`
- **Description**: Saves staged changes to the local repository with a descriptive message.

1. **Push Changes**

- **Command**: `git push origin <branch-name>`
- **Description**: Uploads your branch to a remote repository.

2. **Fetch Latest Repository Info**
    
    - **Command**: `git fetch`
    - **Description**: Downloads updates from the remote repository without merging them.
3. **Merge Changes**
    
    - **Commands**:
        
        - **Description**: Merges updates from the remote `main` branch into your branch.
        
        ```shell
        git switch main
        git fetch
        git merge origin/main
        ```
        
4. **Pull Changes**
    
    - **Command**: `git pull origin main`
    - **Description**: Fetches and merges changes from the remote `main` branch to your branch.

---

## Additional Resources

- **Understanding Merging**: Learn more about merging and conflict resolution from the [official Git merge documentation](https://git-scm.com/docs/git-merge).
- **Git Reference Guide**: For a full list of commands, visit the [Git reference guide](https://git-scm.com/docs).

This guide provides a practical reference for managing Git repositories, covering basic and advanced commands for version control. Let me know if there are any adjustments you’d like!