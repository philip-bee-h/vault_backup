---
type: reference
refrenceName: linux-command-ref-guide
tags:
  - unix/commands
  - linux
  - terminal
  - vim
source: https://www.geeksforgeeks.org/linux-commands-cheat-sheet/
date: 2024-05-29
---
### Basic Linux Commands with Examples

### 1. File and Directory Operations Commands

| Command | Description  | Options | Examples  |
|---------|--------------|---------|-----------|
| `ls`    | List files and directories. | `-l`: Long format listing.<br>`-a`: Include hidden files.<br>`-h`: Human-readable file sizes. | `ls -l`<br>`ls -a`<br>`ls -lh` |
| `cd`    | Change directory. | | `cd /path/to/directory` |
| `pwd`   | Print current working directory. | | `pwd` |
| `mkdir` | Create a new directory. | | `mkdir my_directory` |
| `rm`    | Remove files and directories. | `-r`: Remove directories recursively.<br>`-f`: Force removal without confirmation. | `rm file.txt`<br>`rm -r my_directory`<br>`rm -f file.txt` |
| `cp`    | Copy files and directories. | `-r`: Copy directories recursively. | `cp file.txt destination`<br>`cp -r directory destination` |
| `mv`    | Move/rename files and directories. | | `mv file.txt new_name.txt`<br>`mv file.txt directory` |
| `touch` | Create an empty file or update file timestamps. | | `touch file.txt` |
| `cat`   | View the contents of a file. | | `cat file.txt` |
| `head`  | Display the first few lines of a file. | `-n`: Specify the number of lines. | `head file.txt`<br>`head -n 5 file.txt` |
| `tail`  | Display the last few lines of a file. | `-n`: Specify the number of lines. | `tail file.txt`<br>`tail -n 5 file.txt` |
| `ln`    | Create links between files. | `-s`: Create symbolic (soft) links. | `ln -s source_file link_name` |
| `find`  | Search for files and directories. | `-name`: Search by filename.<br>`-type`: Search by file type. | `find /path/to/search -name "*.txt"` |
| `less`  | View file contents interactively. | | `less file.txt` |

### 2. File Permission Commands

| Command | Description  | Options | Examples  |
|---------|--------------|---------|-----------|
| `chmod` | Change file permissions. | `u`: User/owner permissions.<br>`g`: Group permissions.<br>`o`: Other permissions.<br>`+`: Add permissions.<br>`-`: Remove permissions.<br>`=`: Set permissions explicitly. | `chmod u+rwx file.txt` |
| `chown` | Change file ownership. | | `chown user file.txt` |
| `chgrp` | Change group ownership. | | `chgrp group file.txt` |
| `umask` | Set default file permissions. | | `umask 022` |

### 3. File Compression and Archiving Commands

| Command | Description  | Options | Examples  |
|---------|--------------|---------|-----------|
| `tar`   | Create or extract archive files. | `-c`: Create a new archive.<br>`-x`: Extract files.<br>`-f`: Specify the file name.<br>`-v`: Verbose mode.<br>`-z`: Use gzip.<br>`-j`: Use bzip2. | `tar -czvf archive.tar.gz files/` |
| `gzip`  | Compress files. | `-d`: Decompress files. | `gzip file.txt` |
| `zip`   | Create compressed zip archives. | `-r`: Include directories recursively. | `zip archive.zip file1.txt file2.txt` |

### 4. Process Management Commands

| Command | Description                              | Options                                                                                                                                                                                                                                                                                                                                                                                                                 | Examples                                                                                                                                                   |
| ------- | ---------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ps`    | Display running processes.               | `-aux`: Show all processes.                                                                                                                                                                                                                                                                                                                                                                                             | `ps aux`                                                                                                                                                   |
| `top`   | Monitor system processes in real-time.   |                                                                                                                                                                                                                                                                                                                                                                                                                         | `top`                                                                                                                                                      |
| `kill`  | Terminate a process.                     | `-9`: Forcefully kill a process.                                                                                                                                                                                                                                                                                                                                                                                        | `kill PID`                                                                                                                                                 |
| `pkill` | Terminate processes based on their name. |                                                                                                                                                                                                                                                                                                                                                                                                                         | `pkill process_name`                                                                                                                                       |
| `pgrep` | List processes based on their name.      |                                                                                                                                                                                                                                                                                                                                                                                                                         | `pgrep process_name`                                                                                                                                       |
| `grep`  | Search for specific patterns in text.    | `-i`: Ignore case distinctions.<br>`-v`: Invert the match.<br>`-r`: Recursively search directories.<br>`-l`: Print only names of files containing matches.<br>`-n`: Display line numbers.<br>`-w`: Match whole words only.<br>`-c`: Count matching lines.<br>`-e`: Specify multiple patterns.<br>`-A`: Display lines after the match.<br>`-B`: Display lines before the match.<br>`-C`: Display lines around the match. | `grep -i "hello" file.txt`<br>`grep -v "error" file.txt`<br>`grep -r "pattern" directory/`<br>`grep -l "keyword" file.txt`<br>`grep -n "pattern" file.txt` |

### 5. System Information Commands

In Linux, there are several commands available to gather system information. Here are some commonly used system information commands:

| Command | Description | Options | Examples |
|---------|-------------|---------|----------|
| `uname` | Print system information. | `-a`: All system information. | `uname -a` |
| `whoami` | Display current username. | | `whoami` |
| `df` | Show disk space usage. | `-h`: Human-readable sizes. | `df -h` |
| `du` | Estimate file and directory sizes. | `-h`: Human-readable sizes.<br>`-s`: Display total size only. | `du -sh directory/` |
| `free` | Display memory usage information. | `-h`: Human-readable sizes. | `free -h` |
| `uptime` | Show system uptime. | | `uptime` |
| `lscpu` | Display CPU information. | | `lscpu` |
| `lspci` | List PCI devices. | | `lspci` |
| `lsusb` | List USB devices. | | `lsusb` |

### 6. Networking Commands

In Linux, there are several networking commands available to manage and troubleshoot network connections. Here are some commonly used networking commands:

| Command | Description | Examples |
|---------|-------------|----------|
| `ifconfig` | Display network interface information. | `ifconfig` |
| `ping` | Send ICMP echo requests to a host. | `ping google.com` |
| `netstat` | Display network connections and statistics. | `netstat -tuln` |
| `ss` | Display network socket information. | `ss -tuln` |
| `ssh` | Securely connect to a remote server. | `ssh user@hostname` |
| `scp` | Securely copy files between hosts. | `scp file.txt user@hostname:/path/to/destination` |
| `wget` | Download files from the web. | `wget http://example.com/file.txt` |
| `curl` | Transfer data to or from a server. | `curl http://example.com` |

### 7. IO Redirection Commands

In Linux, IO (Input/Output) redirection commands are used to redirect the standard input, output, and error streams of commands and processes. Here are some commonly used IO redirection commands:

| Command | Description |
|---------|-------------|
| `cmd < file` | Input of cmd is taken from file. |
| `cmd > file` | Standard output (stdout) of cmd is redirected to file. |
| `cmd 2> file` | Error output (stderr) of cmd is redirected to file. |
| `cmd 2>&1` | stderr is redirected to the same place as stdout. |
| `cmd1 <(cmd2)` | Output of cmd2 is used as the input file for cmd1. |
| `cmd > /dev/null` | Discards the stdout of cmd by sending it to the null device. |
| `cmd &> file` | Every output of cmd is redirected to file. |
| `cmd 1>&2` | stdout is redirected to the same place as stderr. |
| `cmd >> file` | Appends the stdout of cmd to file. |

### 8. Environment Variable Commands

In Linux, environment variables are used to store configuration settings, system information, and other variables that can be accessed by processes and shell scripts. Here are some commonly used environment variable commands:

| Command | Description | Examples |
|---------|-------------|----------|
| `export VARIABLE_NAME=value` | Sets the value of an environment variable. | `export PATH=/usr/local/bin:$PATH` |
| `echo $VARIABLE_NAME` | Displays the value of a specific environment variable. | `echo $HOME` |
| `env` | Lists all environment variables currently set in the system. | `env` |
| `unset VARIABLE_NAME` | Unsets or removes an environment variable. | `unset VARIABLE_NAME` |
| `export -p` | Shows a list of all currently exported environment variables. | `export -p` |
| `env VAR1=value COMMAND` | Sets the value of an environment variable for a specific command. | `env LANG=en_US.UTF-8 date` |
| `printenv` | Displays the values of all environment variables. | `printenv` |

### 9. User Management Commands

In Linux, user management commands allow you to create, modify, and manage user accounts on the system. Here are some commonly used user management commands:

| Command | Description |
|---------|-------------|
| `who` | Show who is currently logged in. |
| `sudo adduser username` | Create a new user account on the system with the specified username. |
| `finger` | Display information about all the users currently logged into the system, including their usernames, login time, and terminal. |
| `sudo deluser USER GROUPNAME` | Remove the specified user from the specified group. |
| `last` | Show the recent login history of users. |
| `finger username` | Provide information about the specified user, including their username, real name, terminal, idle time, and login time. |
| `sudo userdel -r username` | Delete the specified user account from the system, including their home directory and associated files. The `-r` option ensures the removal of the user’s files. |
| `sudo passwd -l username` | Lock the password of the specified user account, preventing the user from logging in. |
| `su – username` | Switch to another user account with the user’s environment. |
| `sudo usermod -a -G GROUPNAME USERNAME` | Add an existing user to the specified group. The user is added to the group without removing them from their current groups. |

### 10. Shortcuts Commands

There are many shortcut commands in Linux that can help you be more productive. Here are a few of the most common ones:

#### 10.1: Bash Shortcuts Commands

| Function   | Key Combination | Description                                                       |
| ---------- | --------------- | ----------------------------------------------------------------- |
| Navigation | Ctrl + A        | Move to the beginning of the line.                                |
|            | Ctrl + E        | Move to the end of the line.                                      |
|            | Ctrl + B        | Move back one character.                                          |
|            | Ctrl + F        | Move forward one character.                                       |
|            | Alt + B         | Move back one word.                                               |
|            | Alt + F         | Move forward one word.                                            |
| Editing    | Ctrl + U        | Cut/delete from the cursor position to the beginning of the line. |
|            | Ctrl + K        | Cut/delete from the cursor position to the end of the line.       |
|            | Ctrl + W        | Cut/delete the word before the cursor.                            |
|            | Ctrl + Y        | Paste the last cut text.                                          |
|            | Ctrl + L        | Clear the screen.                                                 |
| History    | Ctrl + R        | Search command history (reverse search).                          |
|            | Ctrl + G        | Escape from history search mode.                                  |
|            | Ctrl + P        | Go to the previous command in history.                            |
|            | Ctrl + N        | Go to the next command in history.                                |
|            | Ctrl + C        | Terminate the current command.                                    |

#### 10.2: Nano Shortcuts Commands

| Function | Key Combination | Description |
|----------|-----------------|-------------|
| File Operations | Ctrl + O | Save the file. |
| | Ctrl + X | Exit Nano (prompt to save if modified). |
| | Ctrl + R | Read a file into the current buffer. |
| | Ctrl + J | Justify the current paragraph. |
| Navigation | Ctrl + Y | Scroll up one page. |
| | Ctrl + V | Scroll down one page. |
| | Alt + \ | Go to a specific line number. |
| | Alt + , | Go to the beginning of the current line. |
| | Alt + . | Go to the end of the current line. |
| Editing | Ctrl + K | Cut/delete from the cursor position to the end of the line. |
| | Ctrl + U | Uncut/restore the last cut text. |
| | Ctrl + 6 | Mark a block of text for copying or cutting. |
| | Alt + 6 | Copy the marked block of text. |
| Search and Replace | Ctrl + W | Search for a string in the text. |
| | Alt + W | Search and replace a string in the text. |
| | Alt + R | Repeat the last search. |

#### 10.3: VI Shortcuts Commands

| Command | Description |
|---------|-------------|
| `cw` | Change the current word (deletes from the cursor position to the end of the current word and switches to insert mode). |
| `dd` | Delete the current line. |
| `x` | Delete the character under the cursor. |
| `R` | Enter replace mode (overwrites characters starting from the cursor position). |
| `o` | Insert a new line below the current line and switch to insert mode. |
| `u` | Undo the last change. |
| `s` | Substitute the character under the cursor and switch to insert mode. |
| `dw` | Delete from the cursor position to the beginning of the next word. |
| `D` | Delete from the cursor position to the end of the line. |
| `4dw` | Delete the next four words from the cursor position. |
| `A` | Switch to insert mode at the end of the current line. |
| `S` | Delete the current line and switch to insert mode. |
| `r` | Replace the character under the cursor with a new character entered from the keyboard. |
| `i` | Switch to insert mode before the cursor. |
| `3dd` | Delete the current line and the two lines below it. |
| `ESC` | Exit from insert or command-line mode and return to command mode. |
| `U` | Restore the current line to its original state before any changes were made. |
| `~` | Switch the case of the character under the cursor. |
| `a` | Switch to insert mode after the cursor. |
| `C` | Delete from the cursor position to the end of the line and switch to insert mode. |

#### 10.4: Vim Shortcuts Commands

| Mode    | Key            | Description                                              |
| ------- | -------------- | -------------------------------------------------------- |
| Normal  | `i`            | Enter insert mode at the current cursor position.        |
|         | `x`            | Delete the character under the cursor.                   |
|         | `dd`           | Delete the current line.                                 |
|         | `yy`           | Copy the current line.                                   |
|         | `p`            | Paste the copied or deleted text below the current line. |
|         | `u`            | Undo the last change.                                    |
|         | `Ctrl + R`     | Redo the last undo.                                      |
| Command | `:w`           | Save the file.                                           |
|         | `:q`           | Quit Vim.                                                |
|         | `:q!`          | Quit Vim without saving changes.                         |
|         | `:wq` or `:x`  | Save and quit Vim.                                       |
|         | `:s/old/new/g` | Replace all occurrences of "old" with "new" in the file. |
| Visual  | `v`            | Enter visual mode to select text.                        |
|         | `y`            | Copy the selected text.                                  |
|         | `d`            | Delete the selected text.                                |
|         | `p`            | Paste the copied or deleted text.                        |

