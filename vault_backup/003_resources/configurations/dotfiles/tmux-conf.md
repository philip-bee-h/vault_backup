---
title: tmux-conf
created: 2025-01-11
type: resource
category: 
tags:
  - resource
  - dotfiles
  - tmux
related_docs:
---
**Modified:**

# **Resource Overview**
This is my previous `.tmux.conf` file, I opted to swap it out for something more basic but didnt want to loose this



```yml
# 1. Prefix Key
unbind C-b                       # Unbind default prefix
set-option -g prefix C-a         # Set new prefix
bind C-a send-prefix             # Allow sending Ctrl-a to programs (double Ctrl-a)

# 2. Pane Management
# Splitting Panes
bind '\' split-window -h         # Horizontal split
bind '-' split-window -v         # Vertical split

# Navigating Panes
bind -n M-Left select-pane -L    # Navigate left
bind -n M-Right select-pane -R   # Navigate right
bind -n M-Up select-pane -U      # Navigate up
bind -n M-Down select-pane -D    # Navigate down

# Pane Appearance
set-option -g pane-border-style fg=white
set-option -g pane-active-border-style fg=green

# Display pane numbers
set-option -g display-panes-time 1000
bind-key q display-panes          # Show pane numbers for selection

# Kill Pane
bind-key x kill-pane              # Kill the current pane

# 3. Window Management
# Sensible window navigation
bind -n C-a next-window         # Next window
bind -n C-a previous-window     # Previous window

# Session Management
bind-key s choose-session         # List and choose sessions
bind-key S new-session            # Create a new session

# 4. Status Bar Configuration
set -g status-bg black            # Background color
set -g status-fg white            # Foreground color

# Status Bar Content
set -g status-left "#[fg=green,bold]Session: #S #[default]|"
set -g status-right "#[fg=cyan]#(whoami) #[default]| #[fg=yellow]%H:%M:%S #[default]| #[fg=magenta]%Y-%m-%d"

# Window Status Styles
set -g window-status-current-format "#[fg=white,bg=blue] #I:#W #[default]"
set -g window-status-format "#[fg=grey] #I:#W #[default]"

# 5. Mouse Mode
set -g mouse on                   # Enable mouse support

# 6. Scrollback Buffer
set -g history-limit 10000        # Increase scrollback buffer size

# 7. Copy and Paste
bind-key -T copy-mode-vi y send-keys -X copy-pipe-and-cancel "xclip -sel clipboard || wl-copy" # Copy to clipboard (Linux)
bind-key -T copy-mode-vi p paste-buffer                                                      # Paste from clipboard

# 8. Reload Configuration
bind r if-shell "test -f ~/.tmux.conf" \
    "source-file ~/.tmux.conf \; display 'Config Reloaded'" \
    "display 'Config file not found!'"
```
