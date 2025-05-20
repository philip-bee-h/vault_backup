---
type: reference
refrenceName: dmesg-refrence-note
tags:
  - unix/commands
source: "{{source}}"
date: 2024-06-26
---
## Description
The `dmesg -H` command is used in Unix and Linux systems to display the kernel ring buffer messages, which include system and hardware logs crucial for diagnosing hardware and driver issues. The `-H` option formats the output in a human-readable form with color highlighting and paging, making it easier to browse through entries.

## Commands / Snippets
```bash
dmesg -H
```

Additional options that can be useful:
- **`-T`**: Display the log timestamps in human-readable format, showing the actual date and time instead of seconds since boot.
- **`-w`**: Continuously watch the dmesg log in real-time.
- **`-l`** (level): Filter messages by priority level (e.g., `err`, `warn`, `info`).

Example using multiple options:
```bash
sudo dmesg -H -T -l warn
```

## Instructions to Run
To use the `dmesg -H` command along with its additional options, follow these steps:

1. Open a terminal in your Linux or Unix system.
2. To view all kernel messages in a human-readable format with color, execute:
   ```bash
   sudo dmesg -H
   ```
3. To include timestamps in a human-readable format, you can combine `-H` with `-T`:
   ```bash
   sudo dmesg -H -T
   ```
4. If you want to filter the messages by a specific priority level, such as warnings, use:
   ```bash
   sudo dmesg -H -l warn
   ```
5. For real-time monitoring of the log, use `-w`:
   ```bash
   sudo dmesg -H -w
   ```
6. Use the arrow keys to navigate through the paginated output, and press `q` to quit the real-time view or pagination mode.