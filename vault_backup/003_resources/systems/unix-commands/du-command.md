---
type: reference
refrenceName: du-command
tags:
  - unix/commands
source: "{{source}}"
date: 2024-05-09
---
The command, `du -h -d 1 | sort -h`, is used to list the sizes of directories within the current directory, including their human-readable sizes, and then sorts them by size. Here's a breakdown of each part:

### 1. `du -h -d 1`
- `du` stands for "disk usage." It is used to estimate file space usage.
- `-h` stands for "human-readable." This option makes the command print sizes in a human-readable format (e.g., K, M, G for kilobytes, megabytes, gigabytes).
- `-d 1` limits the depth of directories the command reports to one level deep. This means it only shows the sizes of directories directly inside the current directory, not deeper.

### 2. `| sort -h`
- The `|` symbol is a pipe. It takes the output of the command on the left (du -h -d 1) and uses it as the input for the command on the right (sort -h).
- `sort` is a command that sorts lines of text.
- `-h` in the context of `sort` stands for "human-numeric sort." This option allows `sort` to understand and correctly order the human-readable sizes (like 1K, 1M, etc.), rather than sorting them alphabetically.

### Putting it all together:
When you run `du -h -d 1 | sort -h` in a Unix terminal:
- The `du` command lists all directories one level deep in the current directory, showing their sizes in a human-readable format.
- This output is then piped to the `sort` command, which sorts these directories by size, from smallest to largest.

This command is particularly useful for quickly identifying which directories are using the most disk space within a certain area of your file system.



