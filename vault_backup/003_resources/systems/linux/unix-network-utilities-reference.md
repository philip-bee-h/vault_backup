---
title: "unix-network-utilities-reference"
created: 2024-10-28
modified: 2024-10-28
type: command-reference
system: unix
tags:
  - commands
  - networking
  - unix
  - cli-tools
  - reference
---


## **1. Netcat (nc)**
- **Description**: Known as the "Swiss Army knife" of networking, Netcat is invaluable for networking diagnostics, chatting, file transfers, port scanning, and listening to connections.
- **Common Uses**:
	- **Port Listening**: `nc -l 1234`
	- **Remote Host Connection**: `nc example.com 1234`
	- **File Transfer**:
	    - **Sender**: `nc -l 1234 < file.txt`
	    - **Receiver**: `nc example.com 1234 > file.txt`
- **Installation**:
	- Ubuntu/Debian: `sudo apt-get install netcat`
	- Red Hat/CentOS: `sudo yum install nc`

## **2. Ping**
- **Description**: Checks connectivity between devices and measures round-trip time (RTT) for packets.
- **Basic Command**: `ping example.com`
- **Options**:
  - **Count**: `-c` (e.g., `ping -c 5 example.com` for 5 pings)
  - **Flooding**: `-f` (faster packet sending for diagnostics)

## **3. Traceroute**
- **Description**: Maps the route packets take to reach a remote host, identifying potential network delays.
- **Basic Command**: `traceroute example.com`
- **Options**:
  - **Max TTL**: `-m` to limit hops (e.g., `traceroute -m 15 example.com`)

## **4. Nmap (Network Mapper)**
- **Description**: A comprehensive tool for network discovery and security auditing, used for host discovery, service scanning, and OS detection.
- **Basic Syntax**: `nmap [options] target`
- **Common Uses**:
  - **Port Scanning**: `nmap -p 1-100 example.com`
  - **OS Detection**: `nmap -O example.com`
- **Installation**:
  - Ubuntu/Debian: `sudo apt-get install nmap`
  - Red Hat/CentOS: `sudo yum install nmap`

## **5. IP (iproute2)**
- **Description**: Provides comprehensive networking information and configurations.
- **Common Commands**:
  - **Show IPs**: `ip addr show`
  - **Show Routes**: `ip route show`
  - **Modify Routes**: `ip route add [destination]`

## **6. Curl**
- **Description**: Transfers data from or to a server, supporting various protocols (HTTP, FTP, etc.) and often used for web API testing.
- **Basic Command**: `curl http://example.com`
- **Options**:
  - **Save to File**: `curl -o output.html http://example.com`
  - **POST Request**: `curl -X POST -d "param=value" http://example.com/api`

## **7. Telnet**
- **Description**: A diagnostic tool for testing connectivity to remote hosts over TCP, often used for testing open ports.
- **Basic Command**: `telnet example.com 80`
