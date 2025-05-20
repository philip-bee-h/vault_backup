---
title: 00-working-dead-nodes
created: 2025-02-26
type: general
tags:
  - uiowa
  - working-note
  - hpc
  - argon
---
### **Issue Summary: Hostname Mismatch in SGE Execution Hosts**

#### **Observation:**

While logging into `argon-itf-ca45-07` via SSH, the prompt indicates that the system is actually `argon-itf-cf46-04`. This discrepancy is confirmed by running `hostnamectl`, which shows:

plaintext

CopyEdit

`Static hostname: argon-itf-cf46-04.hpc`

Additionally, the SGE spool directory structure and logs further confirm that the system identifies itself as `argon-itf-cf46-04`.

```sh
11:28:14 hawrth @ argon-itf-head-ssh_session in ~
❯ sudo ssh argon-itf-ca45-07
[sudo] password for hawrth:
[root@argon-itf-cf46-04 ~]# cd /opt/sge/default/spool/
[root@argon-itf-cf46-04 spool]# ll
total 0
drwxr-xr-x 5 sgeadmin sgeadmin 140 Feb 26 09:51 argon-itf-cf46-04
[root@argon-itf-cf46-04 spool]# cd argon-itf-cf46-04/
[root@argon-itf-cf46-04 argon-itf-cf46-04]# ll
total 8
drwxr-xr-x 2 sgeadmin sgeadmin 40 Feb 26 09:51 active_jobs
-rw-r--r-- 1 sgeadmin sgeadmin  6 Feb 26 09:51 execd.pid
drwxr-xr-x 2 sgeadmin sgeadmin 40 Feb 26 09:51 job_scripts
drwxr-xr-x 2 sgeadmin sgeadmin 40 Feb 26 09:51 jobs
-rw-r--r-- 1 sgeadmin sgeadmin 88 Feb 26 09:51 messages
[root@argon-itf-cf46-04 argon-itf-cf46-04]# cat messages
02/26/2025 09:51:08|  main|argon-itf-cf46-04|I|starting up SGE 8.1.9 (MUNGE) (lx-amd64)
[root@argon-itf-cf46-04 argon-itf-cf46-04]# hostnamectl
   Static hostname: argon-itf-cf46-04.hpc
         Icon name: computer-server
           Chassis: server
        Machine ID: e221ca6951344df68c096c27c4144cfc
           Boot ID: 146ae5a82e434b2f854e003a87e87814
  Operating System: CentOS Linux 7 (Core)
       CPE OS Name: cpe:/o:centos:centos:7
            Kernel: Linux 3.10.0-1160.119.1.el7.x86_64
      Architecture: x86-64
[root@argon-itf-cf46-04 argon-itf-cf46-04]#
```
