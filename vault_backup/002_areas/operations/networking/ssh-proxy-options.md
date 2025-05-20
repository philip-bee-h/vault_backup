---
title: ssh-proxy-options
created: 2024-11-15
type: process
system: 
tags:
  - process
  - procedure
  - uiowa
  - ssh
  - proxy
related_docs:
---
### Quick Guide: SSH Proxy Options

---

#### **1. Local Port Forwarding (`-L`):**
Forward a specific port on your local machine to a port on a remote server.

**Command:**
```bash
ssh -L [local_port]:[remote_host]:[remote_port] [user]@[ssh_server]
```

**Example:**
Forward local port `8080` to port `80` on `remote-server` via `ssh-server`:
```bash
ssh -L 8080:remote-server:80 user@ssh-server
```

- Access the service by visiting `http://localhost:8080`.

---

#### **2. Dynamic Port Forwarding (`-D`):**
Set up a SOCKS proxy for dynamic traffic routing through the SSH server.

**Command:**
```bash
ssh -D [local_port] [user]@[ssh_server]
```

**Example:**
Create a SOCKS proxy on local port `9090` via `ssh-server`:
```bash
ssh -D 9090 user@ssh-server
```

- Configure your browser or application to use `localhost:9090` as a SOCKS proxy for traffic routing.

---

Both methods require an SSH client and access to the SSH server. Use `-L` for specific services and `-D` for broader, dynamic traffic needs.