---
title: tmux-status-bars-config
created: 2025-05-17
type: resource
category: 
tags:
  - resource
  - tmux
  - config
  - dotfiles
related_docs:
---


# Configuration 1: Original Simple Status Bar

```tmux
# --- Status Bar ---
set -g status-position top
set -g status-bg black
set -g status-fg white
set -g status-left "#[fg=green,bold]Session: #S #[default]|"
set -g status-right "#[fg=cyan]#(whoami) #[default]| #[fg=yellow]%H:%M:%S #[default]| #[fg=magenta]%Y-%m-%d"
set -g window-status-current-format "#[fg=white,bg=blue] #I:#W #[default]"
set -g window-status-format "#[fg=grey] #I:#W #[default]"
```

# Configuration 2: Better Contrast Status Bar

```tmux
# --- Status Bar ---
set -g status-position top
set -g status-bg black
set -g status-fg white
set -g status-left "#[fg=green,bold]Session: #S #[default]|"
set -g status-right "#[fg=cyan]#(whoami) #[default]| #[fg=yellow]%H:%M:%S #[default]| #[fg=magenta]%Y-%m-%d"
set -g window-status-current-format "#[fg=black,bg=cyan,bold] #I:#W #[default]"
set -g window-status-format "#[fg=grey] #I:#W #[default]"
```

# Configuration 3: Full Enhanced Configuration

```tmux
# --- Pane Management ---
bind '\' split-window -h
bind '-' split-window -v
bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D
set-option -g pane-border-style fg=white
set-option -g pane-active-border-style fg=green
set-option -g display-panes-time 1000
bind-key q display-panes
bind-key x kill-pane

# --- Window & Session Management ---
bind-key s choose-session
bind-key S new-session

# Enhanced Tmux Status Bar - Efficient & Informative
# --- Status Bar ---
set -g status-position top
set -g status-bg black
set -g status-fg white
set -g status-interval 1                                      # Update status bar every second for accurate time

# Left status with session name and window count
set -g status-left-length 30                                  # Allow more space for left status
set -g status-left "#[fg=green,bold]Session: #S #[fg=yellow][#[fg=cyan]#{window_index}/#{session_windows}#[fg=yellow]] #[default]|"

# Right status with system info and time
set -g status-right-length 80                                 # Allow more space for right status
set -g status-right "#[fg=cyan]#(whoami)@#h #[default]| #[fg=blue]#{pane_current_path} #[default]| #[fg=yellow]%H:%M:%S #[default]| #[fg=magenta]%Y-%m-%d"

# Window status formatting with improved contrast
set -g window-status-current-format "#[fg=black,bg=cyan,bold] #I:#W#{?window_zoomed_flag,🔍,} #[default]"
set -g window-status-format "#[fg=grey] #I:#W #[default]"

# Make windows and panes index start at 1 (easier to reach on keyboard)
set -g base-index 1
setw -g pane-base-index 1

# Automatically renumber windows when one is closed
set -g renumber-windows on

# Add visual indicator for activity in other windows
setw -g monitor-activity on
set -g visual-activity off                                    # Don't show message, just highlight in status bar
set -g window-status-activity-style fg=yellow

# --- Mouse & Scrolling ---
set -g mouse on
set -g history-limit 10000

# --- Copy/Paste ---
bind-key -T copy-mode-vi y send-keys -X copy-pipe-and-cancel "xclip -sel clipboard || wl-copy"
bind-key -T copy-mode-vi p paste-buffer

# --- Reload Config ---
bind r if-shell "test -f ~/.tmux.conf" \
    "source-file ~/.tmux.conf \; display 'Config Reloaded'" \
    "display 'Config file not found!'"
```

# Configuration 4: Window Naming Extensions

```tmux
# Automatically set window names based on current directory
setw -g automatic-rename on
setw -g automatic-rename-format '#{b:pane_current_path}'  # Show only the base directory name

# Quick window rename with prefix+R
bind R command-prompt -I "#{window_name}" "rename-window '%%'"  

# Create new window and immediately prompt for name (pre-filled with directory)
bind c new-window -c "#{pane_current_path}" \; command-prompt -I "#{b:pane_current_path}" "rename-window '%%'"

# --- Reload Config ---
# Changed from 'r' to 'C-r' to avoid conflict with rename window
bind C-r if-shell "test -f ~/.tmux.conf" \
    "source-file ~/.tmux.conf \; display 'Config Reloaded'" \
    "display 'Config file not found!'"
```

# Configuration 5: Optimized Status Bar (No Path in Status)

```tmux
# Enhanced Tmux Status Bar (Without Path in Status Bar)
# --- Status Bar ---
set -g status-position top
set -g status-bg black
set -g status-fg white
set -g status-interval 1                                      # Update status bar every second for accurate time

# Left status with session name and window count
set -g status-left-length 30                                  # Allow more space for left status
set -g status-left "#[fg=green,bold]Session: #S #[fg=yellow][#[fg=cyan]#{window_index}/#{session_windows}#[fg=yellow]] #[default]|"

# Right status with system info and time
set -g status-right-length 80                                 # Allow more space for right status
set -g status-right "#[fg=cyan]#(whoami)@#h #[default]| #[fg=yellow]%H:%M:%S #[default]| #[fg=magenta]%Y-%m-%d"

# Window status formatting with improved contrast
set -g window-status-current-format "#[fg=black,bg=cyan,bold] #I:#W#{?window_zoomed_flag,🔍,} #[default]"
set -g window-status-format "#[fg=grey] #I:#W #[default]"
```


# Configuration 6: Optimized status bar current dir name in bar

```tmux
# --- Pane Management ---
bind '\' split-window -h
bind '-' split-window -v
bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D
set-option -g pane-border-style fg=white
set-option -g pane-active-border-style fg=green
set-option -g display-panes-time 1000
bind-key q display-panes
bind-key x kill-pane

# --- Window & Session Management ---
bind-key s choose-session
bind-key S new-session

# Enhanced Tmux Status Bar - Efficient & Informative
# --- Status Bar ---
set -g status-position top
set -g status-bg black
set -g status-fg white
set -g status-interval 1                                      # Update status bar every second for accurate time

# Left status with session name and window count
set -g status-left-length 30                                  # Allow more space for left status
set -g status-left "#[fg=green,bold]Session: #S #[fg=yellow][#[fg=cyan]#{window_index}/#{session_windows}#[fg=yellow]] #[default]|"

# Right status with system info and time
set -g status-right-length 80
set -g status-right "#[fg=blue]#{b:pane_current_path} #[default]| #[fg=yellow]%H:%M:%S #[default]| #[fg=magenta]%Y-%m-%d"

# Window status formatting with improved contrast
set -g window-status-current-format "#[fg=black,bg=cyan,bold] #I:#W#{?window_zoomed_flag,🔍,} #[default]"
set -g window-status-format "#[fg=grey] #I:#W #[default]"

# Make windows and panes index start at 1 (easier to reach on keyboard)
set -g base-index 1
setw -g pane-base-index 1

# Automatically renumber windows when one is closed
set -g renumber-windows on

# Add visual indicator for activity in other windows
setw -g monitor-activity on
set -g visual-activity off                                    # Don't show message, just highlight in status bar
set -g window-status-activity-style fg=yellow

# --- Mouse & Scrolling ---
set -g mouse on
set -g history-limit 10000

# --- Copy/Paste ---

# --- Reload Config ---
bind r if-shell "test -f ~/.tmux.conf" \
    "source-file ~/.tmux.conf \; display 'Config Reloaded'" \
    "display 'Config file not found!'"
```