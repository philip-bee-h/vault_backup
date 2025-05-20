---
title: "RS Team Dotfiles Project"
created: 2024-10-27
modified: 2024-10-27
type: project
tags:
  - dotfiles
  - rs-team
  - uiowa
  - active
related_docs:
---

# RS Team Dotfiles Project

This project focuses on creating a sensible, organized dotfiles repository for the Research Services (RS) team to streamline and standardize team environments.

## Directory Structure

- **1 directory**
  - **`.bashrc`**
    - Basic alias
    - Prompt customization / starship option
    - Git info script
  - **`.zshrc`**
    - Basic alias
    - Prompt customization / starship option
    - Git info script
  - **`.vimrc`**
  - **`.tmux.conf`**
  - **`.starship.toml`**
  - **`homebrew.md`**: Instructions for setup on macOS.
  - **`README`**
    - Recommended tools
    - Setup instructions for running scripts
    - Cheat sheet links
  - **bootstrap-scripts**
    - `bash-setup`
    - `zsh-setup`

---

## Custom Zsh Prompts

This guide provides a selection of custom Zsh prompt configurations to enhance your terminal experience. Each prompt includes code snippets and examples to help you choose a configuration that best fits your workflow.

### Prompt Configurations

1. **Minimal Prompt with Git Branch**
   - Shows the current directory and Git branch (if applicable).
   - **Configuration**:
     ```shell
     precmd() {
         if [ -d .git ]; then
             PS1=" %F{cyan}%~ %F{green}[$(git rev-parse --abbrev-ref HEAD 2>/dev/null)]%f %F{cyan}➜ %f "
         else
             PS1=" %F{cyan}%~%f %F{cyan}❯ %f "
         fi
     }
     ```

2. **User@Host Prompt with Git Branch**
   - Displays the user and hostname, along with the current directory and Git branch.
   - **Configuration**:
     ```shell
     precmd() {
         local user_host="%F{magenta}%n@%m%f"
         if [ -d .git ]; then
             PS1=" ${user_host} %F{cyan}%~ %F{green}[$(git rev-parse --abbrev-ref HEAD 2>/dev/null)]%f %F{cyan}➜ %f "
         else
             PS1=" ${user_host} %F{cyan}%~%f %F{cyan}❯ %f "
         fi
     }
     ```

3. **Minimal Prompt with Time**
   - Shows the current directory and current time in hours, minutes, and seconds.
   - **Configuration**:
     ```shell
     function parse_git_branch() {
         git branch 2> /dev/null | sed -n -e 's/^\* \(.*\)/[\1]/p'
     }
     COLOR_DEF=$'%f'
     COLOR_DATE=$'%F{243}'
     COLOR_DIR=$'%F{197}'
     COLOR_GIT=$'%F{39}'
     setopt PROMPT_SUBST
     PROMPT_CWD='${COLOR_DIR}%~ ${COLOR_GIT}$(parse_git_branch)${COLOR_DEF}'
     export NEWLINE=$'\n'
     export PROMPT="${PROMPT_CWD}${NEWLINE}${COLOR_DATE}%D{%H:%M:%S}${COLOR_DEF} $ "
     ```

---

## Resources

- [Y Combinator Discussion on Dotfiles](https://news.ycombinator.com/item?id=30976436)
- [Faster Syntax Highlighting and Zsh Auto Suggestions](https://nickjanetakis.com/blog/add-a-git-branch-to-your-prompt-with-a-few-lines-of-shell-scripting)
- [Tmux Guide by Julius](https://juliu.is/a-simple-tmux/)
- [Video on Custom Tmux Config](https://youtube.com/watch?v=niuOc02Rvrc&si=f9Cf82IwDFG-31MO)
- [Reddit Discussion on Tmux](https://www.reddit.com/r/tmux/s/J9DLVtumx3)
- [Will Hbr’s Dotfiles for Tmux](https://github.com/willhbr/dotfiles/blob/main/tmux/tmux.conf)
- [Developer Hacks on Modern Command-Line Tools](https://medium.com/otto-tech/developer-hacks-modern-command-line-tools-and-advanced-git-commands-2985cf5f1bb8)
- [Reddit Post on Custom Zsh Prompt for macOS](https://www.reddit.com/r/MacOS/s/S4L0zwcBOZ)
Tmux config!!!! https://levelup.gitconnected.com/getting-started-with-tmux-b124138ef58b