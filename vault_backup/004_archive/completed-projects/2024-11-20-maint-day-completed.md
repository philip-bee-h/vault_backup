---
title: 2024-11-20-maint-day
created: 2024-11-19
type: project
status: 
tags:
  - project
  - uiowa
  - argon
  - storage
  - idas
related_docs:
---
**Modified:**

# Project Overview
Keeping track of all I have to do and monitor for the 2024-11-20 maintenance day

# Objectives

- Jumbo Frames/ Reboots - DONE
- Firmware Update - DONE
- IDAS deployment

---
# Firmware Update - DONE reboot happening

- Product Name = HBA 9500-8i
	- FW Version = 26.00.01.00
	- [x] Flash Version 31

- Product Name = HBA 9500-8e
	- FW Version = 26.00.01.00
	- [x] Flash Version 31


---
# IDAS Deployment








-----
# Jumbo Frame Setup
## Notes


```sh
   sudo ip link set enp34s0f0 mtu 9000   
   sudo ip link set enp34s0f1 mtu 9000   
   sudo ip link set bond0 mtu 9000   
   sudo ip link set vlan.1104 mtu 9000   
   sudo ip link set enp129s0f1 mtu 9000
```


To test config ping this one
```sh
ping -s 8992 -M do 172.29.4.104
```

If it is not configured right

```sh
hawrth@lc-rs-store32:~$ ping -s 8992 -M do 172.29.4.104
PING 172.29.4.104 (172.29.4.104) 8992(9020) bytes of data.
ping: local error: message too long, mtu=1500
ping: local error: message too long, mtu=1500
ping: local error: message too long, mtu=1500
ping: local error: message too long, mtu=1500
^C
--- 172.29.4.104 ping statistics ---
4 packets transmitted, 0 received, +4 errors, 100% packet loss, time 3054ms
```


```shell
ip link show | grep mtu
```

```sh
ip a | grep --color=auto -E '172.29.(4|5|6)\.'
```


```sh
ip a | grep --color=auto -E '172.29.(4|5|6)\.|bond|vlan'

```

## Tmux set up for node-maint

Certainly! Here are the individual `tmux` commands you need to run manually to create the session and windows for each node, then reattach to the session. You can run these commands directly in your terminal.

### Step-by-Step Commands:

1. **Start a new `tmux` session**:

   ```bash
   tmux new-session -d -s node-maint
   ```

2. **Create a new window for each node and rename the windows**:

```sh
tmux new-session -d -s node-maint \; \
  new-window -t node-maint:1 -n "itf-rs-store22" \; \
  new-window -t node-maint:2 -n "itf-rs-store23" \; \
  new-window -t node-maint:3 -n "itf-rs-store29" \; \
  new-window -t node-maint:4 -n "itf-rs-store34" \; \
  new-window -t node-maint:5 -n "lc-rs-storage12" \; \
  new-window -t node-maint:6 -n "lc-rs-store25" \; \
  new-window -t node-maint:7 -n "lc-rs-store30" \; \
  new-window -t node-maint:8 -n "lc-rs-store32" \; \
  new-window -t node-maint:9 -n "itf-rs-store24" \; \
  new-window -t node-maint:10 -n "lc-rs-storage20" \; \
  attach-session -t node-maint


```

3. **Attach to the session**:

   ```bash
   tmux attach-session -t node-maint
   ```

### What this does:
- **Creates a new `tmux` session** named `node-maint`.
- **Creates 10 windows**, each named after a node in your list.
- **Attaches to the session**, where you can SSH manually into each node.

After running these commands, each window will be named according to the node (e.g., `itf-rs-store22`, `itf-rs-store23`, etc.), and you can SSH into each one manually using:

```bash
ssh itf-rs-store22
```

You can switch between the windows using `Ctrl-a n` (next window) or `Ctrl-a p` (previous window).

---

## Runbook for Jumbo Frame Setup and Maintenance

#### **1. itf-rs-store22.hpc.uiowa.edu - ==DONE==**
```ssh
ssh itf-rs-store22.hpc.uiowa.edu
```
- **Owner:** Jake Michaelson Lab
- **[ ] Reboot** (Needed)
  
**1104 Information:**

```sh
[hawrth@itf-rs-store22 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eno1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether ac:1f:6b:70:62:94 brd ff:ff:ff:ff:ff:ff
    inet 172.30.13.123/24 brd 172.30.13.255 scope global noprefixroute eno1
       valid_lft forever preferred_lft forever
    inet6 fe80::ae1f:6bff:fe70:6294/64 scope link
       valid_lft forever preferred_lft forever
3: eno2: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc mq state DOWN group default qlen 1000
    link/ether ac:1f:6b:70:62:95 brd ff:ff:ff:ff:ff:ff
4: enp95s0f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 3c:fd:fe:c2:98:44 brd ff:ff:ff:ff:ff:ff
    inet 128.255.83.117/24 brd 128.255.83.255 scope global noprefixroute enp95s0f0
       valid_lft forever preferred_lft forever
    inet6 fe80::3efd:feff:fec2:9844/64 scope link
       valid_lft forever preferred_lft forever
5: enp95s0f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 3c:fd:fe:c2:98:45 brd ff:ff:ff:ff:ff:ff
    inet 172.29.4.6/22 brd 172.29.7.255 scope global noprefixroute enp95s0f1
       valid_lft forever preferred_lft forever
    inet6 fe80::3efd:feff:fec2:9845/64 scope link
       valid_lft forever preferred_lft forever
[hawrth@itf-rs-store22 ~]$
```

Commands to Run
```sh
sudo ip link set enp95s0f1 mtu 9000
```

**Notes**
```sh
[hawrth@itf-rs-store22 ~]$ ip link show | grep mtu
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
2: eno1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
3: eno2: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc mq state DOWN mode DEFAULT group default qlen 1000
4: enp95s0f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
5: enp95s0f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9000 qdisc mq state UP mode DEFAULT group default qlen 1000
[hawrth@itf-rs-store22 ~]$ ping -s 8972 -M do 172.29.4.104
PING 172.29.4.104 (172.29.4.104) 8972(9000) bytes of data.
8980 bytes from 172.29.4.104: icmp_seq=1 ttl=64 time=1.10 ms
8980 bytes from 172.29.4.104: icmp_seq=2 ttl=64 time=0.583 ms
8980 bytes from 172.29.4.104: icmp_seq=3 ttl=64 time=0.584 ms
8980 bytes from 172.29.4.104: icmp_seq=4 ttl=64 time=0.581 ms
^C
--- 172.29.4.104 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3001ms
rtt min/avg/max/mdev = 0.581/0.712/1.101/0.225 ms
[hawrth@itf-rs-store22 ~]$
```

---

#### **2. itf-rs-store23.hpc.uiowa.edu - ==DONE==**
```ssh
ssh itf-rs-store23.hpc.uiowa.edu
```
- **Owner:** Terry Braun
- **[ ] Reboot** (Needed)
  
**1104 Information:**
```sh
[hawrth@itf-rs-store23 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp134s0f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 90:e2:ba:89:73:28 brd ff:ff:ff:ff:ff:ff
    inet 172.29.4.118/22 brd 172.29.7.255 scope global noprefixroute enp134s0f0
       valid_lft forever preferred_lft forever
    inet6 fe80::92e2:baff:fe89:7328/64 scope link
       valid_lft forever preferred_lft forever
3: enp134s0f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 90:e2:ba:89:73:29 brd ff:ff:ff:ff:ff:ff
    inet 128.255.83.118/24 brd 128.255.83.255 scope global noprefixroute enp134s0f1
       valid_lft forever preferred_lft forever
    inet6 fe80::92e2:baff:fe89:7329/64 scope link
       valid_lft forever preferred_lft forever
```

Commands to Run
```sh
sudo ip link set enp134s0f0 mtu 9000
```

---

#### **3. itf-rs-store29.hpc.uiowa.edu - ==DONE==**
```ssh
ssh itf-rs-store29.hpc.uiowa.edu
```
- **Owner:** Jun Wang Lab
- **[ ] Reboot** (Needed)
  
**1104 Information:**
```sh
[hawrth@itf-rs-store29 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp94s0f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 40:a6:b7:2e:c1:20 brd ff:ff:ff:ff:ff:ff
    inet 128.255.83.123/24 brd 128.255.83.255 scope global noprefixroute enp94s0f0
       valid_lft forever preferred_lft forever
    inet6 fe80::42a6:b7ff:fe2e:c120/64 scope link
       valid_lft forever preferred_lft forever
3: enp94s0f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 40:a6:b7:2e:c1:21 brd ff:ff:ff:ff:ff:ff
    inet 172.29.4.128/22 brd 172.29.7.255 scope global noprefixroute enp94s0f1
       valid_lft forever preferred_lft forever
    inet6 fe80::42a6:b7ff:fe2e:c121/64 scope link
       valid_lft forever preferred_lft forever
```

Commands to Run
```sh
sudo ip link set enp94s0f1 mtu 9000
```

**Notes:**
```sh
[hawrth@itf-rs-store29 ~]$ ip link show | grep mtu
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
2: enp94s0f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
3: enp94s0f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9000 qdisc mq state UP mode DEFAULT group default qlen 1000
[hawrth@itf-rs-store29 ~]$ ping -s 8972 -M do 172.29.4.104
PING 172.29.4.104 (172.29.4.104) 8972(9000) bytes of data.
8980 bytes from 172.29.4.104: icmp_seq=1 ttl=64 time=1.16 ms
8980 bytes from 172.29.4.104: icmp_seq=2 ttl=64 time=0.595 ms
8980 bytes from 172.29.4.104: icmp_seq=3 ttl=64 time=0.345 ms
c8980 bytes from 172.29.4.104: icmp_seq=4 ttl=64 time=0.555 ms
8980 bytes from 172.29.4.104: icmp_seq=5 ttl=64 time=0.571 ms
8980 bytes from 172.29.4.104: icmp_seq=6 ttl=64 time=0.550 ms
8980 bytes from 172.29.4.104: icmp_seq=7 ttl=64 time=0.347 ms
8980 bytes from 172.29.4.104: icmp_seq=8 ttl=64 time=0.564 ms
8980 bytes from 172.29.4.104: icmp_seq=9 ttl=64 time=0.562 ms
8980 bytes from 172.29.4.104: icmp_seq=10 ttl=64 time=0.551 ms
^C
--- 172.29.4.104 ping statistics ---
10 packets transmitted, 10 received, 0% packet loss, time 9001ms
rtt min/avg/max/mdev = 0.345/0.580/1.167/0.215 ms
[hawrth@itf-rs-store29 ~]$
```
---

#### **4. itf-rs-store34.hpc.uiowa.edu - ==DONE==**
```ssh
ssh itf-rs-store34.hpc.uiowa.edu
```
- **Owner:** IIHR
- **[ ] Reboot** (Needed)
  
**1104 Information:**
```sh
hawrth@itf-rs-store34:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eno1: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether d8:5e:d3:e3:a2:30 brd ff:ff:ff:ff:ff:ff
    altname enp1s0f0
3: ens3f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 50:7c:6f:13:a7:88 brd ff:ff:ff:ff:ff:ff
    altname enp23s0f0
    inet 172.29.4.66/24 brd 172.29.4.255 scope global ens3f0
       valid_lft forever preferred_lft forever
    inet6 fe80::527c:6fff:fe13:a788/64 scope link
       valid_lft forever preferred_lft forever
4: eth2: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether d8:5e:d3:e3:a2:32 brd ff:ff:ff:ff:ff:ff
5: enp1s0f1: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether d8:5e:d3:e3:a2:31 brd ff:ff:ff:ff:ff:ff
6: ens20: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether d8:5e:d3:e3:a2:33 brd ff:ff:ff:ff:ff:ff
    altname enp104s0
7: ens3f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 50:7c:6f:13:a7:89 brd ff:ff:ff:ff:ff:ff
    altname enp23s0f1
    inet 128.255.83.82/24 brd 128.255.83.255 scope global ens3f1
       valid_lft forever preferred_lft forever
    inet6 2620:0:e50:6a01:527c:6fff:fe13:a789/64 scope global dynamic mngtmpaddr noprefixroute
       valid_lft 2591885sec preferred_lft 604685sec
    inet6 fe80::527c:6fff:fe13:a789/64 scope link
       valid_lft forever preferred_lft forever
```

Commands to Run
```sh
sudo ip link set ens3f0 mtu 9000
```

**Notes:**
```sh
hawrth@itf-rs-store34:~$ ip link show | grep mtu
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
2: ens3f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9000 qdisc mq state UP mode DEFAULT group default qlen 1000
3: eno1: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
4: eth2: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
5: ens3f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
6: ens20: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
7: enp1s0f1: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
hawrth@itf-rs-store34:~$ ping -s 8972 -M do 172.29.4.104
PING 172.29.4.104 (172.29.4.104) 8972(9000) bytes of data.
8980 bytes from 172.29.4.104: icmp_seq=1 ttl=64 time=0.994 ms
8980 bytes from 172.29.4.104: icmp_seq=2 ttl=64 time=0.672 ms
8980 bytes from 172.29.4.104: icmp_seq=3 ttl=64 time=0.534 ms
8980 bytes from 172.29.4.104: icmp_seq=4 ttl=64 time=0.676 ms
8980 bytes from 172.29.4.104: icmp_seq=5 ttl=64 time=0.687 ms
^C
--- 172.29.4.104 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4061ms
rtt min/avg/max/mdev = 0.534/0.712/0.994/0.151 ms
hawrth@itf-rs-store34:~$
```

---

#### **5. lc-rs-storage12.hpc.uiowa.edu - ==DONE==**
```ssh
ssh lc-rs-storage12.hpc.uiowa.edu
```
- **Owner:** IIHR
  
**1104 Information:**
```sh
[hawrth@lc-rs-storage12 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: enp5s0f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq master LSA state UP group default qlen 1000
    link/ether 0c:c4:7a:a9:f9:3e brd ff:ff:ff:ff:ff:ff
3: enp5s0f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq master LSA state UP group default qlen 1000
    link/ether 0c:c4:7a:a9:f9:3e brd ff:ff:ff:ff:ff:ff
4: ens5f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq master team-1104 state UP group default qlen 1000
    link/ether a0:36:9f:9b:f9:de brd ff:ff:ff:ff:ff:ff
5: ens5f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq master team-1104 state UP group default qlen 1000
    link/ether a0:36:9f:9b:f9:de brd ff:ff:ff:ff:ff:ff
6: ens5f2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq master team-1104 state UP group default qlen 1000
    link/ether a0:36:9f:9b:f9:de brd ff:ff:ff:ff:ff:ff
7: ens5f3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq master team-1104 state UP group default qlen 1000
    link/ether a0:36:9f:9b:f9:de brd ff:ff:ff:ff:ff:ff
8: LSA: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 0c:c4:7a:a9:f9:3e brd ff:ff:ff:ff:ff:ff
    inet 172.30.21.19/24 brd 172.30.21.255 scope global noprefixroute LSA
       valid_lft forever preferred_lft forever
9: team-1104: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether a0:36:9f:9b:f9:de brd ff:ff:ff:ff:ff:ff
    inet 172.29.4.29/22 brd 172.29.7.255 scope global noprefixroute team-1104
       valid_lft forever preferred_lft forever
```

**Commands to Run**
- Set MTU to 9000 for `team-1104`:
```sh
sudo ip link set team-1104 mtu 9000
```

- Set MTU to 9000 for each of the individual bonded interfaces:
```sh
sudo ip link set ens5f0 mtu 9000
sudo ip link set ens5f1 mtu 9000
sudo ip link set ens5f2 mtu 9000
sudo ip link set ens5f3 mtu 9000
```

**Notes:**
```sh
[hawrth@lc-rs-storage12 ~]$ ip link show | grep mtu
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
2: enp5s0f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq master LSA state UP mode DEFAULT group default qlen 1000
3: enp5s0f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq master LSA state UP mode DEFAULT group default qlen 1000
4: ens5f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9000 qdisc mq master team-1104 state UP mode DEFAULT group default qlen 1000
5: ens5f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9000 qdisc mq master team-1104 state UP mode DEFAULT group default qlen 1000
6: ens5f2: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9000 qdisc mq master team-1104 state UP mode DEFAULT group default qlen 1000
7: ens5f3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9000 qdisc mq master team-1104 state UP mode DEFAULT group default qlen 1000
8: LSA: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DEFAULT group default qlen 1000
9: team-1104: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9000 qdisc noqueue state UP mode DEFAULT group default qlen 1000
[hawrth@lc-rs-storage12 ~]$ ping -s 8972 -M do 172.29.4.104
PING 172.29.4.104 (172.29.4.104) 8972(9000) bytes of data.
8980 bytes from 172.29.4.104: icmp_seq=1 ttl=64 time=1.18 ms
8980 bytes from 172.29.4.104: icmp_seq=2 ttl=64 time=1.13 ms
8980 bytes from 172.29.4.104: icmp_seq=3 ttl=64 time=0.894 ms
8980 bytes from 172.29.4.104: icmp_seq=4 ttl=64 time=1.11 ms
^C
--- 172.29.4.104 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3003ms
rtt min/avg/max/mdev = 0.894/1.080/1.189/0.118 ms
[hawrth@lc-rs-storage12 ~]$
```


---

#### **6. lc-rs-store25.hpc.uiowa.edu - ==DONE==**
```ssh
ssh lc-rs-store25.hpc.uiowa.edu
```
- **Owner:** Reinhardt & Hoffman
  
**1104 Information:**
```sh
[hawrth@lc-rs-store25 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp24s0f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 40:a6:b7:3c:75:bc brd ff:ff:ff:ff:ff:ff
    inet 128.255.1.94/26 brd 128.255.1.127 scope global noprefixroute enp24s0f0
       valid_lft forever preferred_lft forever
    inet6 fe80::42a6:b7ff:fe3c:75bc/64 scope link
       valid_lft forever preferred_lft forever
3: enp24s0f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 40:a6:b7:3c:75:bd brd ff:ff:ff:ff:ff:ff
    inet 172.29.4.15/22 brd 172.29.7.255 scope global noprefixroute enp24s0f1
       valid_lft forever preferred_lft forever
    inet6 fe80::42a6:b7ff:fe3c:75bd/64 scope link
       valid_lft forever preferred_lft forever
```

Commands to Run
```sh
sudo ip link set enp24s0f1 mtu 9000
```

**Notes:**
```sh
[hawrth@lc-rs-store25 ~]$ ip link show | grep mtu
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
2: enp24s0f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
3: enp24s0f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9000 qdisc mq state UP mode DEFAULT group default qlen 1000
[hawrth@lc-rs-store25 ~]$ ping -s 8972 -M do 172.29.4.104
PING 172.29.4.104 (172.29.4.104) 8972(9000) bytes of data.
8980 bytes from 172.29.4.104: icmp_seq=1 ttl=64 time=1.69 ms
8980 bytes from 172.29.4.104: icmp_seq=2 ttl=64 time=0.954 ms
8980 bytes from 172.29.4.104: icmp_seq=3 ttl=64 time=0.780 ms
8980 bytes from 172.29.4.104: icmp_seq=4 ttl=64 time=0.759 ms
8980 bytes from 172.29.4.104: icmp_seq=5 ttl=64 time=0.946 ms
^C
--- 172.29.4.104 ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4003ms
rtt min/avg/max/mdev = 0.759/1.026/1.694/0.345 ms
[hawrth@lc-rs-store25 ~]$
```
---

#### **7. lc-rs-store30.hpc.uiowa.edu - ==DONE==**
```ssh
ssh lc-rs-store30.hpc.uiowa.edu
```
- **Owner:** IIHR
  
**1104 Information:**
```sh
hawrth@lc-rs-store30:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eno1: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether d8:5e:d3:e3:a2:6e brd ff:ff:ff:ff:ff:ff
    altname enp103s0
    altname ens19
3: eth1: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether d8:5e:d3:e3:a2:6c brd ff:ff:ff:ff:ff:ff
4: ens4f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 50:7c:6f:13:a7:4e brd ff:ff:ff:ff:ff:ff
    altname enp202s0f0
    inet 172.29.4.65/22 brd 172.29.7.255 scope global ens4f0
       valid_lft forever preferred_lft forever
    inet6 fe80::527c:6fff:fe13:a74e/64 scope link
       valid_lft forever preferred_lft forever
5: ens20: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether d8:5e:d3:e3:a2:6f brd ff:ff:ff:ff:ff:ff
    altname enp104s0
6: enp1s0f1: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether d8:5e:d3:e3:a2:6d brd ff:ff:ff:ff:ff:ff
7: ens4f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 50:7c:6f:13:a7:4f brd ff:ff:ff:ff:ff:ff
    altname enp202s0f1
    inet 128.255.1.68/26 brd 128.255.1.127 scope global ens4f1
       valid_lft forever preferred_lft forever
    inet6 2620:0:e50:6602:527c:6fff:fe13:a74f/64 scope global dynamic mngtmpaddr noprefixroute
       valid_lft 2591683sec preferred_lft 604483sec
    inet6 fe80::527c:6fff:fe13:a74f/64 scope link
       valid_lft forever preferred_lft forever
```

Commands to Run
```sh
sudo ip link set ens4f0 mtu 9000
```

---

#### **8. lc-rs-store32.hpc.uiowa.edu - ==DONE==**
```ssh
ssh lc-rs-store32.hpc.uiowa.edu
```
- **Owner:** Jake Michaelson Lab
  
**1104 Information:**
```sh
hawrth@lc-rs-store32:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eno1: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether 74:56:3c:53:8a:e4 brd ff:ff:ff:ff:ff:ff
    altname enp103s0
    altname ens19
3: eth1: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether 74:56:3c:53:8a:e2 brd ff:ff:ff:ff:ff:ff
4: ens20: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether 74:56:3c:53:8a:e5 brd ff:ff:ff:ff:ff:ff
    altname enp104s0
5: enp1s0f1: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN group default qlen 1000
    link/ether 74:56:3c:53:8a:e3 brd ff:ff:ff:ff:ff:ff
6: ens5f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 40:a6:b7:be:94:48 brd ff:ff:ff:ff:ff:ff
    altname enp177s0f0
    inet 172.29.4.146/22 brd 172.29.7.255 scope global ens5f0
       valid_lft forever preferred_lft forever
    inet6 fe80::42a6:b7ff:febe:9448/64 scope link
       valid_lft forever preferred_lft forever
7: ens5f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 40:a6:b7:be:94:49 brd ff:ff:ff:ff:ff:ff
    altname enp177s0f1
    inet 128.255.1.98/26 brd 128.255.1.127 scope global ens5f1
       valid_lft forever preferred_lft forever
    inet6 2620:0:e50:6602:42a6:b7ff:febe:9449/64 scope global dynamic mngtmpaddr noprefixroute
       valid_lft 2591579sec preferred_lft 604379sec
    inet6 fe80::42a6:b7ff:febe:9449/64 scope link
       valid_lft forever preferred_lft forever
```

Commands to Run
```sh
sudo ip link set ens5f0 mtu 9000
```

---

#### **9. itf-rs-store24.hpc.uiowa.edu - ==DONE==**
```ssh
ssh itf-rs-store24.hpc.uiowa.edu
```
- **Owner:** Shared
  
**1104 Information:**
```sh
[hawrth@itf-rs-store24 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp94s0f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether ac:1f:6b:4e:21:e4 brd ff:ff:ff:ff:ff:ff
    inet 172.29.4.121/22 brd 172.29.7.255 scope global noprefixroute enp94s0f0
       valid_lft forever preferred_lft forever
    inet6 fe80::ae1f:6bff:fe4e:21e4/64 scope link
       valid_lft forever preferred_lft forever
3: enp94s0f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether ac:1f:6b:4e:21:e5 brd ff:ff:ff:ff:ff:ff
    inet 128.255.83.119/24 brd 128.255.83.255 scope global noprefixroute enp94s0f1
       valid_lft forever preferred_lft forever
    inet6 fe80::ae1f:6bff:fe4e:21e5/64 scope link
       valid_lft forever preferred_lft forever
```

Commands to Run
```sh
sudo ip link set enp94s0f0 mtu 9000
```

notes:
```sh
[hawrth@itf-rs-store24 ~]$ ip link show | grep mtu
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
2: enp94s0f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
3: enp94s0f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
[hawrth@itf-rs-store24 ~]$ sudo ip link set enp94s0f0 mtu 9000
[sudo] password for hawrth:
[hawrth@itf-rs-store24 ~]$ ip link show | grep mtu
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
2: enp94s0f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9000 qdisc mq state UP mode DEFAULT group default qlen 1000
3: enp94s0f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
[hawrth@itf-rs-store24 ~]$ ping -s 8972 -M do 172.29.4.104
PING 172.29.4.104 (172.29.4.104) 8972(9000) bytes of data.
8980 bytes from 172.29.4.104: icmp_seq=1 ttl=64 time=1.11 ms
8980 bytes from 172.29.4.104: icmp_seq=2 ttl=64 time=0.580 ms
8980 bytes from 172.29.4.104: icmp_seq=3 ttl=64 time=0.650 ms
8980 bytes from 172.29.4.104: icmp_seq=4 ttl=64 time=0.630 ms
^C
--- 172.29.4.104 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3001ms
rtt min/avg/max/mdev = 0.580/0.744/1.118/0.218 ms
[hawrth@itf-rs-store24 ~]$
```

---

#### **10. lc-rs-storage20.hpc.uiowa.edu - ==Done==**
```ssh
ssh lc-rs-storage20.hpc.uiowa.edu
```
- **Owner:** Shared
  
**1104 Information:**
```sh
[hawrth@lc-rs-storage20 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: eno1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether ac:1f:6b:61:72:c4 brd ff:ff:ff:ff:ff:ff
3: eno2: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc mq state DOWN group default qlen 1000
    link/ether ac:1f:6b:61:72:c5 brd ff:ff:ff:ff:ff:ff
4: enp216s0f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 3c:fd:fe:be:fe:90 brd ff:ff:ff:ff:ff:ff
    inet 172.29.4.97/22 brd 172.29.7.255 scope global noprefixroute enp216s0f0
       valid_lft forever preferred_lft forever
    inet6 fe80::3efd:feff:febe:fe90/64 scope link
       valid_lft forever preferred_lft forever
5: enp216s0f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 3c:fd:fe:be:fe:91 brd ff:ff:ff:ff:ff:ff
    inet 128.255.1.89/26 brd 128.255.1.127 scope global noprefixroute enp216s0f1
       valid_lft forever preferred_lft forever
    inet6 fe80::3efd:feff:febe:fe91/64 scope link
       valid_lft forever preferred_lft forever
```

Commands to Run
```sh
sudo ip link set enp216s0f0 mtu 9000
```

**Notes:**
```sh
[hawrth@lc-rs-storage20 ~]$ ip link show | grep mtu
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
2: eno1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
3: eno2: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc mq state DOWN mode DEFAULT group default qlen 1000
4: enp216s0f0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9000 qdisc mq state UP mode DEFAULT group default qlen 1000
5: enp216s0f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP mode DEFAULT group default qlen 1000
[hawrth@lc-rs-storage20 ~]$ ping -s 8972 -M do 172.29.4.104
PING 172.29.4.104 (172.29.4.104) 8972(9000) bytes of data.
8980 bytes from 172.29.4.104: icmp_seq=1 ttl=64 time=1.64 ms
8980 bytes from 172.29.4.104: icmp_seq=2 ttl=64 time=0.953 ms
8980 bytes from 172.29.4.104: icmp_seq=3 ttl=64 time=0.958 ms
8980 bytes from 172.29.4.104: icmp_seq=4 ttl=64 time=0.926 ms
8980 bytes from 172.29.4.104: icmp_seq=5 ttl=64 time=0.964 ms
8980 bytes from 172.29.4.104: icmp_seq=6 ttl=64 time=0.812 ms
8980 bytes from 172.29.4.104: icmp_seq=7 ttl=64 time=0.977 ms
8980 bytes from 172.29.4.104: icmp_seq=8 ttl=64 time=0.771 ms
8980 bytes from 172.29.4.104: icmp_seq=9 ttl=64 time=0.978 ms
8980 bytes from 172.29.4.104: icmp_seq=10 ttl=64 time=0.989 ms
^C
--- 172.29.4.104 ping statistics ---
10 packets transmitted, 10 received, 0% packet loss, time 9011ms
rtt min/avg/max/mdev = 0.771/0.997/1.644/0.227 ms
[hawrth@lc-rs-storage20 ~]$
```



## LC nodes not accepting Jumbo Frames

- DCIM
	- Need names & ports for nodes not accepting JF


 **lc-rs-store30 - Blocked**
![[CleanShot 2024-11-20 at 11.51.57.png]]

**lc-rs-store32 - Blocked**
![[CleanShot 2024-11-20 at 11.53.05.png]]

**rs-idas-ceph01 - Blocked**
![[CleanShot 2024-11-20 at 11.49.52.png]]

**rs-idas-ceph02 - Blocked** 
![[CleanShot 2024-11-20 at 11.50.45.png]]

**rs-idas-ceph03 - Blocked** 
![[CleanShot 2024-11-20 at 11.51.18.png]]





