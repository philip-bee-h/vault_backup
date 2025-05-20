---
type: reference
refrenceName: unix-tool-reference
tags: 
source: "{{source}}"
date: 2024-05-17
---
### Basic Unix Tools Reference 

#### 1. **Tool Overview**

- **`cat`**: Displays the content of files.
- **`grep`**: Searches for patterns within files.
- **`awk`**: A powerful text processing tool, especially useful for manipulating data and generating reports.
- **`sed`**: Stream editor for filtering and transforming text.

#### 2. **Combining Tools: Practical Examples**

**Example 1: Display and Search**

Display the contents of a log file and search for error messages:

```bash
cat system.log | grep "error"
```

**Example 2: Data Extraction and Reporting**

Suppose you have a CSV file (`data.csv`) with columns for `User`, `Session`, and `CPU_Time`. You want to find out the total CPU time used by each user.

```bash
awk -F, '{ cpu[$1] += $3 } END { for (user in cpu) print user, cpu[user] }' data.csv
```

**Example 3: Modifying and Filtering Output**

You have a list of job files and need to replace the status `pending` with `processing` and then select only those lines for further review.

```bash
sed 's/pending/processing/' job_status.txt | grep "processing"
```

#### 3. **Advanced Combining**

**Sort and Unique Count**

To view unique error messages sorted by frequency in a log file:

```bash
grep "error" system.log | sort | uniq -c | sort -nr
```

**Extract and Process Logs**

Extract the last 100 lines of a log file, search for a specific error code, and display the corresponding lines with line numbers:

```bash
tail -100 system.log | grep "Error 404" | cat -n
```

### Tips for Maximizing Efficiency

- **Scripting**: For repetitive tasks, consider writing shell scripts that combine these tools.
- **Aliases**: Create aliases for commonly used command sequences in your `.bashrc` or `.bash_profile`.
- **Learn Regular Expressions**: Many of these tools leverage regular expressions. Becoming proficient in regex can dramatically enhance your ability to use these tools effectively.

This reference should help you get started with using and combining basic Unix tools in an HPC environment. Practicing these commands and understanding how they can interact will greatly increase your efficiency and capability when managing HPC resources.