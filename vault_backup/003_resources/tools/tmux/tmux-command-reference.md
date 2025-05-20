---
title: Tmux Command Reference
created: 2024-10-27
modified: 2024-10-27
type: command-reference
tags:
  - command-ref
  - tmux
  - cli
  - active
related_runbooks:
---



## Tmux CLI Commands

| Command                         | Description                       |
| ------------------------------- | --------------------------------- |
| `tmux`                          | Start a new session               |
| `tmux new -s myname`            | Start a new named session         |
| `tmux ls`                       | Show all sessions                 |
| `tmux a`                        | Attach to last session            |
| `tmux a -t myname`              | Attach to a named session         |
| `ite myname`                    | Kill session by name              |
| `tmux kill-ses -a`              | Kill all sessions except current  |
| `tmux kill-ses -a -t myname`    | Kill all sessions except ‘myname’ |
| `tmux info`                     | Show tmux help                    |
| `tmux source-file ~/.tmux.conf` | Reload config                     |
| `tmux show-options -g`          | Show global config                |

---

## Copy Mode Commands

| Shortcut          | Description                |
|-------------------|----------------------------|
| `Ctrl+b [`        | Enter copy mode            |
| `<Space>`         | Start selection            |
| `Enter`           | Copy selection             |
| `q`               | Quit copy mode             |
| `Ctrl+b ]`        | Paste buffer 0             |

---

## Pane Shortcuts

| Shortcut          | Description                          |
|-------------------|--------------------------------------|
| `Ctrl+b "` / `%`  | Split pane horizontally/vertically   |
| `Ctrl+b !`        | Convert pane to window               |
| `Ctrl+b x`        | Kill pane                            |
| `Ctrl+b <Arrow>`  | Navigate between panes               |
| `Ctrl+b <Space>`  | Toggle layouts                       |
| `Ctrl+b { / }`    | Move pane left/right                 |
| `Ctrl+b o`        | Go to next pane                      |
| `Ctrl+b z`        | Toggle fullscreen for pane           |
| `Ctrl+b ;`        | Toggle last active pane              |
| `Ctrl+b q`        | Show pane numbers                    |
| `Ctrl+b q 0...9`  | Go to specific pane                  |

---

## Window Shortcuts

| Shortcut       | Description                |
| -------------- | -------------------------- |
| `Ctrl+b c`     | Create new window          |
| `Ctrl+b p / n` | Go to previous/next window |
| `Ctrl+b w`     | List windows               |
| `Ctrl+b ,`     | Rename window              |
| `Ctrl+b f`     | Find window                |
| `Ctrl+b l`     | Go to last window          |
| `Ctrl+b .`     | Move window                |
| `Ctrl+b &`     | Close window               |
| `Ctrl+b 0...9` | Go to specific window      |

---

## Session Shortcuts

| Shortcut       | Description                 |
| -------------- | --------------------------- |
| `Ctrl+b d`     | Detach from session         |
| `Ctrl+b s`     | Show all sessions           |
| `Ctrl+b $`     | Rename session              |
| `Ctrl+b ( / )` | Go to previous/next session |

---

## Command Mode

| Command                            | Description                                |
|------------------------------------|--------------------------------------------|
| `Ctrl+b :`                         | Enter command mode                         |
| `resize-pane -D 20`                | Resize pane down                          |
| `resize-pane -U 20`                | Resize pane up                            |
| `resize-pane -L 20`                | Resize pane left                          |
| `resize-pane -R 20`                | Resize pane right                         |
| `list-keys`                        | List all commands                         |
| `list-panes`                       | List all panes                            |
| `list-windows`                     | List all windows                          |

---

## Copying and Buffers

| Command                            | Description                                |
|------------------------------------|--------------------------------------------|
| `list-buffers`                     | List all buffers                          |
| `show-buffer`                      | Show buffer 0 contents                    |
| `capture-pane`                     | Copy pane content                         |
| `choose-buffer`                    | Show and paste buffer                     |
| `save-buffer a.txt`                | Save buffer to file                       |
| `delete-buffer -b 1`               | Delete buffer 1                           |

---

## Setting Options

| Command                            | Description                                |
|------------------------------------|--------------------------------------------|
| `set -g OPTION`                    | Set global option                         |
| `setw -g OPTION`                   | Set window option                         |
| `setw -g mode-keys vi`             | Enable vi-mode for navigation             |
| `set -g prefix C-a`                | Change tmux prefix to `Ctrl+a`            |

---

## Miscellaneous Commands

| Command                            | Description                                |
|------------------------------------|--------------------------------------------|
| `swap-pane -s 3 -t 1`              | Swap panes                                |
| `swap-window -t -1`                | Move window to left                       |
| `setw synchronize-panes`           | Synchronize all panes                     |
| `join-pane -t :#`                  | Join pane to another window               |

---

## Resources

- [Tmux Cheat Sheet](https://tmuxcheatsheet.com)
- [Nick Janetakis on Tmux](https://nickjanetakis.com/blog/add-a-git-branch-to-your-prompt-with-a-few-lines-fo-shell-scripting)
- [Comprehensive Guide on Tmux](https://github.com/gpakosz/.tmux)
