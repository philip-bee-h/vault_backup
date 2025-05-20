---
title: grep-command-reference-guide
created: 2025-01-01
type: command-reference
system: 
tags:
  - commands
  - linux
  - macos
  - unix
  - shell
related_runbooks:
---
# Grep Command Reference Guide

## Core Concepts

### Basic Syntax

```bash
grep [OPTIONS] PATTERN [FILE...]
```

### Key Options Categories
1. **Search Behavior**
   - `-i` : Case-insensitive search
   - `-F` : Fixed strings (literal search, no regex)
   - `-E` : Extended regex support (equivalent to egrep)
   - `-P` : Perl regex syntax

2. **Output Control**
   - `-n` : Show line numbers
   - `-l` : List matching files only
   - `-c` : Count matches only
   - `-o` : Show only matched parts
   - `--color=auto` : Highlight matches

3. **Search Scope**
   - `-r, -R` : Recursive search
   - `-v` : Invert match
   - `-w` : Match whole words
   - `-x` : Match whole lines

## Common Use Cases

### Log Analysis
```bash
# Real-time log monitoring with highlighting
tail -f /var/log/syslog | grep --color=auto -E "error|warn|critical"

# Extract specific patterns (e.g., IP addresses)
grep -oE "([0-9]{1,3}\.){3}[0-9]{1,3}" access.log

# Count error occurrences by type
grep -i "error" error.log | sort | uniq -c | sort -nr
```

### File System Operations
```bash
# Find files containing specific content
grep -r "TODO" /path/to/project/

# Search and exclude directories
grep -r --exclude-dir={.git,node_modules} "pattern" .

# Search specific file types
find . -name "*.py" -exec grep -l "import tensorflow" {} \;
```

### Pattern Matching
```bash
# Multiple patterns (OR)
grep -E "error|warning|critical" log.txt

# Complex patterns
grep -P "(?<=GET )[^ ]+" access.log  # Extract URLs after GET

# Negative patterns
grep -v "^#" config.txt  # Exclude comments
```

## Advanced Techniques

### Performance Optimization
```bash
# Parallel grep for large directories
find . -type f -print0 | xargs -0 -P 4 grep -H "pattern"

# Memory-efficient large file processing
grep --line-buffered "pattern" large_file.log

# Binary file handling
grep -a "string" binary_file
```

### Integration with Other Tools
```bash
# With sed (find and replace)
grep -rl "old_text" . | xargs sed -i 's/old_text/new_text/g'

# With awk (complex processing)
grep "ERROR" log.txt | awk '{print $1, $4}'

# Remote execution
ssh user@server 'grep "error" /var/log/syslog'
```

## Best Practices

### Performance
- Use `-F` when searching literal strings
- Avoid unnecessary recursive searches
- Consider using ripgrep (rg) for better performance
- Use appropriate regex engine (-E, -P) based on needs

### Maintainability
- Document complex patterns
- Use variables for reusable patterns
- Consider creating aliases for common operations

### Security
- Avoid greping sensitive files
- Be cautious with recursive searches
- Handle file permissions appropriately

## Troubleshooting

### Common Issues
- Pattern not matching: Check case sensitivity
- Too many results: Refine pattern or use context
- Performance issues: Check file size and pattern complexity

### Debug Options
- `-h` : Don't prefix filenames
- `--debug` : Show debug info
- `--line-buffered` : Force line buffering

## See Also
- `man grep`
- `info grep`
- Related tools: awk, sed, ripgrep