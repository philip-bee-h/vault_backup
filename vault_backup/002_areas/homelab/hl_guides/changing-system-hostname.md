---
title: changing-system-hostname
created: 2024-11-22
type: process
system: 
tags:
  - process
  - procedure
  - uiowa
related_docs:
---
r# **Process Overview**

- Changing the hostname of a server to ensure it aligns with network conventions or organizational requirements.

# **Requirements**

- **Access:** Root or sudo privileges on the server.
- **Tools:** Command-line interface (CLI).
- **References:** `/etc/hostname` and `/etc/hosts` files, `hostnamectl` command documentation.

# **Steps**

1. **Step 1: Check the Current Hostname**  
	- _Verify the server's current hostname._

```bash
hostnamectl
```
 
2. **Step 2: Set the New Hostname**  
	- _Update the hostname to the desired value._  

```bash
sudo hostnamectl set-hostname new-hostname
```

PVE
```sh
hostnamectl set-hostname new-hostname
```

3. **Step 3: Update the Hosts File**  
	- _Ensure the hostname is reflected in the `/etc/hosts` file._
 
```bash
sudo nano /etc/hosts
```
 
_Add or update the line:_
    
```
127.0.1.1   new-hostname
```
 
4. **Step 4: Verify Changes**  
    _Confirm the hostname has been updated._

```bash
hostnamectl
```

5. **Step 5: Reboot the Server (if necessary)**  
    _Restart the server to fully apply changes._

```bash
sudo reboot
```


# **Troubleshooting**

- **Issue:** Hostname does not persist after reboot.
    - **Solution:** Ensure the correct hostname is set in both `/etc/hostname` and `/etc/hosts`. Verify changes using `hostnamectl` and reboot again.
- **Issue:** Unable to edit `/etc/hosts` due to permission errors. 
    - **Solution:** Use `sudo` to gain necessary privileges for editing files.

```bash
sudo nano /etc/hosts
```


monitoring 