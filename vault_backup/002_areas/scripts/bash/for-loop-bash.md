---
title: for-loop-bash
created: 2025-05-12
type: command-reference
system: bash
tags:
  - commands
  - bash
related_runbooks:
---


## Bash `for` Loop: Simple Reference

### **Purpose**

A `for` loop lets you repeat a command for a list of items (like filenames, devices, or hostnames).

---

### **Basic Pattern**

```bash
for item in item1 item2 item3; do
  command "$item"
done
```

---

### **Example: Print Device Names**

```bash
for dev in \
  wwn-0x5000cca2aba62d38 \
  wwn-0x5000cca2aba7678c \
  wwn-0x5000cca2aba79c74; do
  echo "Device: $dev"
done
```

---

### **Use Case: Clear ZFS Errors**

```bash
for dev in \
  wwn-0x5000cca2aba62d38 \
  wwn-0x5000cca2aba7678c \
  wwn-0x5000cca2aba79c74; do
  zpool clear dpool01 "$dev"
done
```

---

### **Tips**

- Use `\` to break long lists across lines (makes it readable).
    
- Quote variables: `"$dev"` — protects against spaces or special characters.
    
- You can replace the list with `$(command)` to loop over command output:
    
    ```bash
for file in $(ls *.log); do
  echo "Processing $file"
done
    ```

variable 
- should be descriptive of its contents