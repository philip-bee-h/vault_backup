---
title: linux-command-reference
created: 2024-12-02
type: command-reference
system: 
tags:
  - commands
  - uiowa
  - linux
  - unix
  - mac
  - terminal
related_runbooks:
---
# Linux Commands Reference

```markdown


## System Information
| Command         | Description                                                   |
|-----------------|---------------------------------------------------------------|
| `uname -a`      | Displays all information about Linux system information       |
| `uname -r`      | Displays the system                                           |
| `uname`         | Displays Linux system information                             |
| `uptime`        | Displays how long the system has been running                 |
| `uptime -p`     | Shows uptime in pretty format                                 |
| `uptime -s`     | Shows uptime start time                                       |
| `hostname`      | Displays host name                                            |
| `hostname -f`   | Displays long host name (FQDN)                                |
| `hostname -i`   | Displays IP addresses for the host name                       |
| `hostname -I`   | Displays all IP addresses for the host                        |
| `last reboot`   | Shows system reboot history                                   |

## Hardware and Resource Information
| Command         | Description                                                   |
|-----------------|---------------------------------------------------------------|
| `lshw`          | Hardware Lister                                               |
| `lsblk -a`      | Lists information about all block devices                     |
| `lscpu`         | Displays information about the CPU architecture               |
| `lscpu -a`      | Prints both online and offline CPUs                           |

## Networking
| Command         | Description                                                   |
|-----------------|---------------------------------------------------------------|
| `ip addr`       | Shows addresses assigned to all network interfaces            |
| `ip route`      | Shows table routes                                            |
| `ifconfig`      | Displays the IP address of the system                         |

## Date and Time
| Command         | Description                                                   |
|-----------------|---------------------------------------------------------------|
| `date`          | Shows system date and timestamp                               |
| `date +"%d"`    | Prints day of the month (01-31)                               |
| `date +"%m"`    | Prints the month of the year (01-12)                          |
| `date +"%y"`    | Prints only the last two digits of Year                       |
| `date +"%H"`    | Prints the hour (00-23)                                       |
| `date +"%M"`    | Prints the Minute of the hour (00-59)                         |
| `date +"%S"`    | Prints the current seconds count (00-60)                      |
| `date +"%D"`    | Prints Date in MM/DD/YY                                       |
| `date +"%F"`    | Prints the full date as YYYY-MM-DD                            |
| `date +"%A"`    | Prints the Day of the Week (Saturday-Sunday)                  |
| `date +"%B"`    | Prints the month (January-December)                           |

## User Information
| Command         | Description                                                   |
|-----------------|---------------------------------------------------------------|
| `who`           | Prints information about users currently logged in            |
| `who -a`        | Prints information about all users currently logged in        |
| `whoami`        | Prints the username of the current effective user ID          |
| `id`            | Prints user and group information for the specified user      |

## Permissions and Limits
| Command         | Description                                                   |
|-----------------|---------------------------------------------------------------|
| `umask`         | Prints permissions bits for newly created files or directories|
| `ulimit`        | View or limit system resource consumption                     |
| `ulimit -a`     | Detailed report of all resource limits for the current user   |
| `ulimit -Sa`    | Detailed soft limits for the current user                     |
| `ulimit -Ha`    | Detailed hard limits for the current user                     |
| `ulimit -u 100` | Limit a user's maximum process number to 100                  |
| `ulimit -f 100` | Limit a user's maximum file size to 100KB                     |
| `ulimit -n 100` | Limit a user's maximum number of open files to 100            |
| `ulimit -v 100` | Limits virtual memory for a process to 100KB                  |

## Disk Usage
| Command         | Description                                                   |
|-----------------|---------------------------------------------------------------|
| `df`            | Report file system disk space usage                           |
| `df -h`         | Report file system disk space usage in human-readable format  |
| `du`            | Summarize disk usage of each file, recursively for directories|
| `du -sh`        | Summarize disk usage in human-readable format                 |

## Process and System Monitoring
| Command         | Description                                                   |
|-----------------|---------------------------------------------------------------|
| `top`           | Dynamic real-time view of a running system with processes     |
| `ps`            | Displays information about active processes                   |
| `ps -ef`        | See every process on the system using standard syntax         |
| `ps -aux`       | See every process on the system using BSD syntax              |

## Miscellaneous
| Command                     | Description                                       |
|-----------------------------|---------------------------------------------------|
| `clear`                     | Clears the screen                                 |
| `sudo yum install <pkg>`    | Installs a package in CentOS                      |
| `sudo yum remove <pkg>`     | Removes a package in CentOS                       |
| `sudo yum info <pkg>`       | Gets information about a package in CentOS        |
| `free`                      | Displays system memory details in KB              |
| `free -m`                   | Displays system memory details in MB              |
| `lsof`                      | Lists all open files belonging to active processes|
| `last`                      | Displays login history from /var/log/wtmp         |

```


# Linux Networking and File System Commands

```markdown
## Networking Commands
| Command                        | Description                                                              |
|--------------------------------|--------------------------------------------------------------------------|
| `ping <hostname>`              | Ping sends ICMP ECHO_REQUEST to network hosts                            |
| `ping -4 <hostname>`           | Ping sends ICMP ECHO_REQUEST to network hosts with IPv4 address          |
| `ping -6 <hostname>`           | Ping sends ICMP ECHO_REQUEST to network hosts with IPv6 address          |
| `ping -c 3 <hostname>`         | Stop after sending 3 count ECHO_REQUEST packets                          |
| `ping -i 3 <hostname>`         | Wait interval 3 seconds between sending each packet                      |
| `telnet <hostname> <port_no>`  | To communicate with another host using the TELNET protocol               |
| `telnet -4 <hostname> <port_no>` | Force IPv4 address resolution                                          |
| `telnet -6 <hostname> <port_no>` | Force IPv6 address resolution                                          |

## File System Commands
| Command                                          | Description                                                              |
|--------------------------------------------------|--------------------------------------------------------------------------|
| `fdisk -l`                                      | List the partition tables for the specified devices                      |
| `fdisk -s <partition>`                          | Displays partition size(s) in blocks                                     |
| `mount`                                         | Displays all file systems including the virtual ones                     |
| `mount <device_name> <directory>`               | To mount a file system in a given location                               |
| `mount -t <file_system_type> <device_name> <directory>` | To mount a file system in a given location with specific filesystem type |
| `umount <device_name>` OR `umount <directory>`  | To detach a mounted file system                                          |
| `umount -l <directory>`                         | Lazy unmount a busy file system as soon as it is not busy anymore        |
| `umount -f <directory>`                         | Force unmount for an unreachable filesystem                              |
```

```markdown
# Linux User Management and File Commands

## User Management Commands
| Command                  | Description                                                                       |
|--------------------------|-----------------------------------------------------------------------------------|
| `useradd`                | To add the user                                                                   |
| `useradd -e`             | Expiration date of the new account                                                |
| `useradd -U`             | Create a group with the same name as the user                                     |
| `useradd -p`             | Encrypted password of the new account                                             |
| `useradd -M`             | Create the user's home directory                                                  |
| `useradd -D`             | Print or change default `useradd` configuration file `/etc/default/useradd`       |
| `useradd -s`             | Login shell of the new account                                                    |
| `useradd -G`             | List of supplementary groups of the new account                                   |
| `usermod`                | To modify the user                                                                |
| `usermod -L`             | Lock the user account                                                             |
| `usermod -U`             | Unlock the user account                                                           |
| `usermod -aG`            | Append the user to supplemental groups without removing the user from other groups|
| `userdel`                | To delete the user                                                                |
| `userdel -f`             | Force some actions that would fail otherwise                                      |
| `userdel -r`             | Remove home directory and mail spool                                              |
| `passwd`                 | To set the user password                                                          |
| `chage -l`               | Shows the account aging information                                               |

## Group Management Commands
| Command                  | Description                                                                       |
|--------------------------|-----------------------------------------------------------------------------------|
| `groupadd`               | To add the group                                                                  |
| `groupadd -p`            | Use this encrypted password for the new group                                     |
| `groups`                 | Displays the groups where the current user belongs                                |
| `groupmod -n`            | Change the name to a new group                                                    |
| `groupdel`               | To delete the group                                                               |
| `groupdel -f`            | Delete the group even if it is the primary group of a user                        |

## Directory and File Navigation Commands
| Command                  | Description                                                                       |
|--------------------------|-----------------------------------------------------------------------------------|
| `ls`                     | To list the files in the current directory                                        |
| `ls -ltr`                | To list files in long format, sorted by modification time in reverse order        |
| `ls -la`                 | To list files in long format, including hidden files                              |
| `pwd`                    | Shows the present working directory                                               |
| `cd`                     | To change the directory                                                           |
| `cd ..`                  | To move one directory above                                                       |
| `cd -`                   | To move into the last working directory                                           |
| `cd /`                   | To change the present directory to the root directory                             |
| `cd ~`                   | To move into the home directory                                                   |

## File and Directory Management Commands
| Command                  | Description                                                                       |
|--------------------------|-----------------------------------------------------------------------------------|
| `touch`                  | To create an empty file                                                           |
| `rm <file1>`             | To remove a file                                                                  |
| `rm -f <file1>`          | To remove a file forcefully                                                       |
| `rm -I <file1>`          | Prompt before every removal                                                       |
| `mkdir <dir1>`           | To create a directory                                                             |
| `rmdir <dir1>`           | To remove a directory                                                             |
| `rmdir -f <dir1>`        | To remove a directory forcefully                                                  |
| `rmdir -r <dir1>`        | To remove a directory recursively                                                 |
| `cp <file1>`             | To copy a file                                                                    |
| `cp <file1> <file2>`     | To copy the content of `file1` to `file2`                                         |
| `cp -r <dir1> <dir2>`    | To copy `dir1` to `dir2` recursively                                              |
| `mv <file1> <file2>`     | To move or rename a file                                                          |

## File Viewing and Editing Commands
| Command                  | Description                                                                       |
|--------------------------|-----------------------------------------------------------------------------------|
| `cat <file1>`            | Displays the content of a file                                                    |
| `cat -n <file1>`         | Number all output lines                                                           |
| `cat -A <file1>`         | Show-all, equivalent to `-vET`                                                    |
| `cat > <file1>`          | To create and fill a file in one command                                          |
| `cat <file1> > <file2>`  | To copy content of one file to another                                            |
| `cat -E <file1>`         | Display `$` at the end of each line                                               |
| `cat -s <file1>`         | Suppress repeated empty output lines                                              |
| `cat -T <file1>`         | Display TAB characters as `^I`                                                    |
| `cat -v <file1>`         | Displays non-printing characters                                                  |
| `less <file1>`           | To read the contents of a large text file one page at a time, with navigation     |
| `more <file1>`           | To read the contents of a file one screenful at a time                            |
| `vi <file1>`             | `vi` is a text editor in Linux to modify files                                    |
| `nano <file1>`           | `nano` is another text editor in Linux                                            |

```

# Linux File Search and Pattern Matching Commands

```markdown
## File Search Commands
| Command                                    | Description                                                                           |
|--------------------------------------------|---------------------------------------------------------------------------------------|
| `locate <file1>`                           | Finds the path where `<file1>` is present                                            |
| `find . -name <file1>`                     | Finds `<file1>` in the current directory                                             |
| `find / -iname <file1>`                    | Finds `<file1>` in `/` directory (case-insensitive)                                  |
| `find . -type d -name <dir1>`              | Finds `<dir1>` in the current directory                                              |
| `find . -type f -name <file1.txt>`         | Finds `file1.txt` in the current directory                                           |
| `find . -type f -perm 777`                 | Finds files with `777` permissions in the current directory                          |
| `find . -type f ! -perm 777`               | Finds files without `777` permissions in the current directory                       |
| `find . -perm /u=r`                        | Finds all read-only files in the current directory                                   |
| `find . -perm /a=x`                        | Finds all executable files in the current directory                                  |
| `find . -perm /a=w`                        | Finds all writable files in the current directory                                    |
| `find . -type f -perm 777 -exec ls -ltr {} \;` | Finds files with `777` permissions and lists them                                   |
| `find / -type d -perm 777 -exec chmod 755 {} \;` | Finds directories with `777` permissions and changes them to `755`                 |
| `find . -type f -empty`                    | Finds all empty files in the current directory                                       |
| `find . -type d -empty`                    | Finds all empty directories in the current directory                                 |
| `find / -user <username> -name <file1>`    | Finds `<file1>` owned by `<username>` in `/` directory                               |
| `find / -user <username>`                  | Finds all files owned by `<username>` in `/` directory                               |
| `find / -group <groupname>`                | Finds all files owned by `<groupname>` in `/` directory                              |
| `find / -mtime 100`                        | Finds files modified 100 days ago in `/` directory                                   |
| `find / -atime 100`                        | Finds files accessed 100 days ago in `/` directory                                   |
| `find / -mtime +50 -mtime -100`            | Finds files modified between 50 and 100 days ago in `/` directory                    |
| `find / -cmin -60`                         | Finds files changed in the last hour in `/` directory                                |
| `find / -mmin -60`                         | Finds files modified in the last hour in `/` directory                               |
| `find / -amin -60`                         | Finds files accessed in the last hour in `/` directory                               |
| `find / -size 100M`                        | Finds all 100MB files in `/` directory                                               |
| `find / -size +50M -size -100M`            | Finds files between 50MB and 100MB in `/` directory                                  |
| `find / -type f -size +100M -exec rm -f {} \;` | Finds files larger than 100MB and deletes them                                      |

## File Viewing Commands
| Command                                    | Description                                                                           |
|--------------------------------------------|---------------------------------------------------------------------------------------|
| `head <file1>`                             | Displays the first 10 lines of `<file1>`                                             |
| `head -n 20 <file1>`                       | Displays the first 20 lines of `<file1>`                                             |
| `head -q <file1> <file2>`                  | Suppresses headers when printing content of multiple files                           |
| `head -v <file1> <file2>`                  | Always prints headers when displaying file content                                   |
| `tail <file1>`                             | Displays the last 10 lines of `<file1>`                                              |
| `tail -n 20 <file1>`                       | Displays the last 20 lines of `<file1>`                                              |
| `tail -q <file1> <file2>`                  | Suppresses headers when printing content of multiple files                           |
| `tail -v <file1> <file2>`                  | Always prints headers when displaying file content                                   |
| `tail -f <file1>`                          | Outputs appended data as `<file1>` grows                                             |
| `tac <file1>`                              | Displays `<file1>` content in reverse (last line first)                              |

## File Linking Commands
| Command                                    | Description                                                                           |
|--------------------------------------------|---------------------------------------------------------------------------------------|
| `ln <original_file> <hard_link>`           | Creates a hard link for `<original_file>`                                            |
| `ln -s <original_file> <soft_link>`        | Creates a soft link for `<original_file>`                                            |

## Pattern Matching with `grep`
| Command                                    | Description                                                                           |
|--------------------------------------------|---------------------------------------------------------------------------------------|
| `grep <pattern> <file1>`                   | Searches for `<pattern>` in `<file1>` and prints matching lines                      |
| `grep <pattern> <file1> <file2>`           | Searches for `<pattern>` in multiple files and prints matching lines                 |
| `grep -l <pattern> <file1> <file2>`        | Prints the filenames containing `<pattern>`                                          |
| `grep -n <pattern> <file1>`                | Prints line numbers along with matching lines in `<file1>`                           |
| `grep -v <pattern> <file1>`                | Prints lines in `<file1>` that do not match `<pattern>`                              |
| `grep ^<pattern> <file1>`                  | Prints lines in `<file1>` starting with `<pattern>`                                  |
| `grep <pattern>$ <file1>`                  | Prints lines in `<file1>` ending with `<pattern>`                                    |
| `grep -r <pattern> /home`                  | Recursively searches `<pattern>` in `/home` and subdirectories                       |
| `grep ^$ <file1>`                          | Prints all empty lines in `<file1>`                                                 |
| `grep -i <pattern> <file1>`                | Case-insensitive search for `<pattern>` in `<file1>`                                 |
| `grep -e <pattern1> -e <pattern2> <file1>` | Searches for multiple patterns in `<file1>`                                         |
| `grep -f <pattern_file> <file1>`           | Uses patterns from `<pattern_file>` to search in `<file1>`                           |
| `grep -c <pattern> <file1>`                | Counts lines containing `<pattern>` in `<file1>`                                    |
| `grep -A 5 <pattern> <file1>`              | Prints 5 lines after matching `<pattern>` in `<file1>`                               |
| `grep -B 5 <pattern> <file1>`              | Prints 5 lines before matching `<pattern>` in `<file1>`                              |
| `grep -C 5 <pattern> <file1>`              | Prints 5 lines before and after matching `<pattern>` in `<file1>`                    |
| `grep -o <pattern> <file1>`                | Shows only the part of lines matching `<pattern>`                                    |
| `grep -w <pattern> <file1>`                | Matches `<pattern>` only as a whole word in `<file1>`                                |
| `grep -x <pattern> <file1>`                | Matches `<pattern>` only as a whole line in `<file1>`                                |

```

# Linux Commands Reference

```markdown


## File Transfer Commands
| Command                                     | Description                                                                                 |
|---------------------------------------------|---------------------------------------------------------------------------------------------|
| `scp <file1> user@hostname:/tmp`            | Copies `<file1>` from local system to the remote host's `/tmp` directory                   |
| `scp user@hostname:/tmp/file1 .`            | Copies `/tmp/file1` from the remote host to the local system                               |
| `scp <file1> <file2> user@hostname:/tmp`    | Copies multiple files from local system to the remote host's `/tmp` directory              |
| `scp -r <dir1> user@hostname:/tmp`          | Recursively copies `<dir1>` from local system to the remote host's `/tmp` directory        |
| `scp -C -r <dir1> user@hostname:/tmp`       | Recursively copies `<dir1>` with compression to the remote host's `/tmp` directory         |
| `scp user@host1:/tmp/file1 user@host2:/tmp/file2` | Copies files between two remote servers                                                |
| `scp -3 user@host1:/tmp/file1 user@host2:/tmp/file2` | Routes the transfer through your system when copying files between two remote servers |

## Rsync Commands
| Command                                     | Description                                                                                 |
|---------------------------------------------|---------------------------------------------------------------------------------------------|
| `rsync -v /home/file1 /tmp`                 | Copies `<file1>` locally with verbose output                                               |
| `rsync -v /home/file1 /home/file2 /tmp`     | Copies multiple files locally with verbose output                                          |
| `rsync -av /home/dir1 /tmp`                 | Recursively copies files and directories                                                   |
| `rsync -av /home/dir1 user@hostname:/tmp`   | Copies a file or directory from local to remote machine                                    |
| `rsync -av /home/dir1 /home/dir2 user@hostname:/tmp` | Copies multiple files or directories to remote machine                                   |
| `rsync -av user@host:/home/dir1 /tmp`       | Copies a file or directory from remote to local machine                                    |
| `rsync -av user@hostname:{/home/dir1,/home/dir21} /tmp` | Copies multiple files or directories to remote machine                                   |
| `rsync -av --progress /home/dir1 /tmp`      | Shows progress during data transfer                                                       |
| `rsync -v --remove-source-files /home/file1 user@host:/tmp` | Deletes source files after transfer                                              |
| `rsync -av --dry-run --delete /home/dir1 user@host:/tmp` | Simulates the deletion of files during transfer                                       |
| `rsync -av --max-size=500k /home/dir1 user@host:/tmp` | Sets maximum file size for transfer                                                     |
| `rsync -av --min-size=500k /home/dir1 user@host:/tmp` | Sets minimum file size for transfer                                                     |
| `rsync -av -f"+ */" -f"- *" /home/dir1 /tmp` | Copies directory structure but skips files                                              |
| `rsync -av /home/dir1 /tmp/dir1$(date +%Y-%m-%d)` | Adds a timestamp to the directory name                                                 |
| `rsync -avu /home/dir1 /tmp/dir2`           | Skips copying if the destination file is newer                                          |
| `rsync -avi /home/dir1 /tmp/dir2`           | Shows differences between source and destination files                                  |

## File Information Commands
| Command                                     | Description                                                                                 |
|---------------------------------------------|---------------------------------------------------------------------------------------------|
| `wc <file1>`                                | Prints the number of lines, words, and bytes in `<file1>`                                   |
| `wc -l <file1>`                             | Prints the number of lines in `<file1>`                                                    |
| `wc -w <file1>`                             | Prints the number of words in `<file1>`                                                    |
| `wc -c <file1>`                             | Prints the number of bytes in `<file1>`                                                    |
| `wc -m <file1>`                             | Prints the number of characters in `<file1>`                                               |
| `wc -L <file1>`                             | Prints the length of the longest line in `<file1>`                                         |

## Sorting and Uniqueness Commands
| Command                                     | Description                                                                                 |
|---------------------------------------------|---------------------------------------------------------------------------------------------|
| `sort <file1>`                              | Sorts lines in `<file1>`                                                                    |
| `sort -r <file1>`                           | Sorts lines in `<file1>` in reverse order                                                  |
| `sort -n <file1>`                           | Sorts lines numerically in `<file1>`                                                       |
| `sort -o <output_file> <file1>`             | Sorts lines and outputs to `<output_file>`                                                 |
| `sort -nr <file1>`                          | Sorts numeric data in reverse order                                                       |
| `sort -k 2n <file1>`                        | Sorts lines based on column number                                                        |
| `sort -c <file1>`                           | Checks if `<file1>` is already sorted                                                     |
| `sort -u <file1>`                           | Removes duplicates while sorting                                                          |
| `sort -M <file1>`                           | Sorts `<file1>` by month                                                                  |
| `uniq <file1>`                              | Omits adjacent duplicate lines in `<file1>`                                               |
| `uniq -c <file1>`                           | Counts occurrences of duplicate lines in `<file1>`                                        |
| `uniq -d <file1>`                           | Prints only repeated lines                                                                |
| `uniq -D <file1>`                           | Prints all duplicates                                                                     |
| `uniq -u <file1>`                           | Prints unique lines                                                                       |
| `uniq -f 2 <file1>`                         | Skips the first 2 fields while comparing uniqueness                                       |
| `uniq -s 2 <file1>`                         | Skips the first 2 characters while comparing uniqueness                                   |
| `uniq -w 2 <file1>`                         | Compares only the first 2 characters for uniqueness                                       |
| `uniq -i <file1>`                           | Ignores case sensitivity while comparing                                                  |
| `uniq -z <file1>`                           | Ends lines with 0 byte instead of newline                                                 |

## Process Management Commands
| Command                                     | Description                                                                                 |
|---------------------------------------------|---------------------------------------------------------------------------------------------|
| `kill <PID>`                                | Sends the kill signal to terminate the specified process                                   |
| `kill -9 <PID>`                             | Forcefully terminates the specified process                                               |

## Cron Job Management Commands
| Command                                     | Description                                                                                 |
|---------------------------------------------|---------------------------------------------------------------------------------------------|
| `crontab -e`                                | Edits or creates a crontab file                                                           |
| `crontab -l`                                | Lists all cron jobs                                                                       |
| `crontab -r`                                | Removes the user's crontab file                                                          |
| `crontab -v`                                | Displays the last edit time for the user's crontab file                                   |

```

# Linux Archiving, Compression, and File Management Commands

```markdown


## Archiving Commands
| Command                                     | Description                                                                                 |
|---------------------------------------------|---------------------------------------------------------------------------------------------|
| `tar xvf test.tar`                          | Extracts an archive                                                                        |
| `tar cvf test.tar <file1> <file2> <file3>`  | Creates an archive with specified files or folders                                         |
| `tar cvzf test.tar <file1> <file2> <file3>` | Creates a compressed archive                                                              |
| `tar tvf test.tar`                          | Lists all files in an archive                                                             |
| `tar cvzf test.tar.gz <file1> <file2>`      | Combines files into a compressed archive                                                  |
| `tar xvzf test.tar.gz`                      | Decompresses and extracts a `.tar.gz` archive                                             |
| `tar cvjf test.tar.bz2 <file1> <file2>`     | Combines files into a compressed archive using bzip2                                      |
| `tar xvjf test.tar.bz2`                     | Decompresses and extracts a `.tar.bz2` archive                                            |

## Compression Commands
| Command                                     | Description                                                                                 |
|---------------------------------------------|---------------------------------------------------------------------------------------------|
| `gzip <file1>`                              | Compresses `<file1>` into `<file1>.gz`                                                     |
| `gunzip <file1>.gz`                         | Decompresses `<file1>.gz`                                                                  |
| `bzip2 <file1>`                             | Compresses `<file1>` into `<file1>.bz2`                                                    |
| `bunzip2 <file1>.bz2`                       | Decompresses `<file1>.bz2`                                                                 |

## Zip Commands
| Command                                     | Description                                                                                 |
|---------------------------------------------|---------------------------------------------------------------------------------------------|
| `zip test.zip <file1> <file2>`              | Combines specified files into a compressed archive                                         |
| `zip -r test.zip <dir1> <dir2> <dir3>`      | Compresses specified directories into an archive                                           |
| `unzip test.zip`                            | Decompresses and extracts a `.zip` archive                                                 |
| `unzip -l test.zip`                         | Lists all files in a `.zip` archive                                                        |

## File Permissions and Ownership
| Command                                     | Description                                                                                 |
|---------------------------------------------|---------------------------------------------------------------------------------------------|
| `chmod 777 <file1>`                         | Grants read(4), write(2), and execute(1) permissions for all                                |
| `chmod -R 755 <dir1>`                       | Recursively grants read(4), write(2), and execute(1) permissions for owner, group, and others|
| `chown swapnil:swapnil <file1>`             | Changes ownership of `<file1>` to `swapnil` for both user and group                        |

## Encryption and Decryption Commands
| Command                                     | Description                                                                                 |
|---------------------------------------------|---------------------------------------------------------------------------------------------|
| `gpg -c <file1>`                            | Encrypts `<file1>`                                                                         |
| `gpg <file1>`                               | Decrypts `<file1>`                                                                         |

## Networking and Data Transfer Commands
| Command                                     | Description                                                                                 |
|---------------------------------------------|---------------------------------------------------------------------------------------------|
| `wget <url>`                                | Downloads a file non-interactively from the specified `<url>`                              |
| `curl <url>`                                | Transfers data from or to a server using a specified protocol                              |
| `curl -kv <url>`                            | Transfers data with verbose output and insecure connection                                 |

## Working with Compressed Files
| Command                                     | Description                                                                                 |
|---------------------------------------------|---------------------------------------------------------------------------------------------|
| `zgrep <grep_options> "pattern" test.gz`    | Searches for expressions within a compressed file                                          |
| `zcat test.gz`                              | Displays the content of a compressed `.gz` file                                           |

```