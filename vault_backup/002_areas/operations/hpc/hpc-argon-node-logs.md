---
title: hpc-argon-node-logs
created: 2025-02-25
type: resource
category: 
tags:
  - resource
  - tool-name
  - uiowa
  - argon
  - hpc
  - logs
related_docs:
---
## Argon HPC Node Logs

Argon compute nodes forward their logs to the **head node** using `rsyslog`. Logs are stored centrally in:

```
/srv/logs/<hostname>/<YYYY-MM-DD>.log
```

### Accessing Argon Node Logs

**1.** SSH into the head node:

```bash
ssh argon-itf-head
```

**2.** Switch to root to access logs (due to permissions):

```bash
sudo su -
```

**3.** Go to the centralized log directory:

```bash
cd /srv/logs/
```

**4.** Each compute node has its own log directory, named after its hostname or IP address:

- Example by hostname:

```bash
cd argon-itf-bx43-04/
```

- Example by IP (if hostname directories aren't present):

```bash
cd 172.29.6.224/
```

**5.** View recent logs or search for errors:

```bash
# See today's logs
less $(date +"%Y-%m-%d").log

# Search for common errors
grep -Ei 'error|panic|oom|segfault|fail' *.log
```

---

**Example path for today's logs:**

```
/srv/logs/argon-itf-bx43-04/2025-02-25.log
```

Use this method to quickly identify and investigate compute node issues.


----

# **Locating the Location of Remote Server Logging**


### Step 1: Identify Remote Logging Destination on Compute Nodes

To determine where logs from compute nodes are sent, log into any live compute node and check the remote logging configuration.

#### Command:

```bash
ssh argon-itf-bx43-04
grep -ri '^\*.*@' /etc/rsyslog.conf /etc/rsyslog.conf /etc/rsyslog.d/*
```

#### Example Output:

```
*.* @argon-itf-head:514
```

This output confirms that logs are being forwarded to the head node (`argon-itf-head`) over UDP port 514.

---

### Step 2: Locate Logs on the Head Node

Once on the head node, determine where incoming logs are stored.

#### Command:

```bash
grep -Ri 'template\|input\|imudp' /etc/rsyslog.conf /etc/rsyslog.d/*
```

#### Example Output:

```
$ModLoad imudp
$template RemoteHost, "/srv/logs/%HOSTNAME%/%$YEAR%-%$MONTH%-%$DAY%.log"
$InputUDPServerBindRuleset remote
```

This output indicates:

- **`imudp`** module is loaded for receiving UDP logs.
- Logs are stored in `/srv/logs/`, organized by hostname and date.

---

### Step 3: Access the Log Files

Switch to the root user to view the logs:

#### Command:

```bash
sudo su
cd /srv/logs/
ls -l
```

Each compute node has its own directory named after its hostname. Inside each directory, logs are structured by date (e.g., `2025-02-25.log`).

To view logs from a specific node:

```bash
cat /srv/logs/argon-itf-bx43-04/2025-02-25.log
```

---

### Summary

- **Compute nodes** forward logs to the head node on UDP port 514.
- **Head node** stores logs under `/srv/logs/{hostname}/{date}.log`.
- Use `grep` to confirm logging configurations.
- Use `sudo` to access and inspect log files.

This process ensures efficient log retrieval and troubleshooting across compute nodes.