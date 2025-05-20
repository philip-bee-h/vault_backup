---
title: linux-permission-reference
created: 2024-12-17
type: resource
category: 
tags:
  - resource
  - linux
  - security
  - terminal
related_docs:
---
### **Quick Reference: Linux File System Permissions**

#### **Permission Basics**

- **Permission Groups:**
    - **u**: Owner
    - **g**: Group
    - **o**: Others (all users)
- **Permission Types:**
    - **r**: Read (view content)
    - **w**: Write (modify/delete)
    - **x**: Execute (run files, enter directories)

#### **Viewing File Permissions**

1. Navigate to the directory.
2. Use: `$ ls -la`
3. Example Output:
    
    ```
    -rw-r--r-- 1 user group 1234 date file.txt
    ```
    
    - First 10 characters represent file type and permissions.

#### **Understanding Permission Bits**

- **Position Breakdown:**
    - 1st: **File Type** (`-` file, `d` directory, `l` link)
    - 2nd-4th: Owner permissions (`rwx`)
    - 5th-7th: Group permissions (`rwx`)
    - 8th-10th: Others’ permissions (`rwx`)

Example:

- **`drwxr-xr-x`**
    - **d**: Directory
    - **rwx**: Owner has read, write, and execute permissions.
    - **r-x**: Group and others have read and execute permissions.

#### **Modifying Permissions**

1. **Command Format:**
    
    ```
    $ chmod <permissions> <file>
    ```
    - Use symbols: `r`, `w`, `x`, `u`, `g`, `o`
    - Add: `+`, Remove: `-`

1. **Examples:**
    
    - Remove all access for others: `$ chmod o-rwx file.txt`
    - Add execute for group: `$ chmod g+x file.txt`

#### **Changing Ownership**

1. Change owner:
    
    ```
    $ chown <new_owner> <file>
    ```
    
2. Change group:
    
    ```
    $ chown <owner>:<new_group> <file>
    ```
    

#### **Special Permissions**

- **SUID (`u+s`)**: Run as owner.
    - Example: `$ chmod u+s file`
- **Sticky Bit (`+t`)**: Only file owner can modify/delete.
    - Example: `$ chmod +t directory/`

#### **Numeric Permissions**

- Numeric representation:
    
    - **r=4**, **w=2**, **x=1**
    - Add values for combined permissions.
- Common Examples:
    - `777`: **rwxrwxrwx** (full access for all)
    - `755`: **rwxr-xr-x** (owner full, others read/execute)
    - `644`: **rw-r--r--** (owner read/write, others read)
- Usage:
    
    ```
    $ chmod 755 file.txt
    ```
    

#### **Common Numeric Permissions Table**

|Symbolic|Octal|Description|
|---|---|---|
|`---------`|0000|No permissions|
|`--x--x--x`|0111|Execute only|
|`-w--w--w-`|0222|Write only|
|`-wx-wx-wx`|0333|Write and execute|
|`-r--r--r--`|0444|Read only|
|`-r-xr-xr-x`|0555|Read and execute|
|`-rw-rw-rw-`|0666|Read and write|
|`-rwxrwxrwx`|0777|Read, write, execute|

#### **Quick Tips**

- Use `ls -la` to view permissions.
- Practice translating numeric permissions for faster workflow.
- `chmod` changes permissions, `chown` changes ownership.

Keep this as a handy reference while working with Linux file systems!