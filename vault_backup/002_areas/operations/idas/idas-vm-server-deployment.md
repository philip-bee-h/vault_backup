---
title: idas-vm-server-deployment
created: 2024-10-30
modified: 2024-10-30
type: process
system: idas
tags:
  - process
  - procedure
  - uiowa
  - idas
  - vm
related_docs:
---
This is being modified from Cody's run book for Kubernetes Node Setup
[Node Setup - HPC Admin - UIowa Wiki (atlassian.net)](https://uiowa.atlassian.net/wiki/spaces/hpcadmin/pages/76514368/Node+Setup)



Commands to run once OS is installed. sudo can be excluded if ran as root. Some of these steps can probably be moved into puppet but for right now they are standalone the computer object should be pre-created in the ITS → Services → Research → IDAS → VMs OU

## Create computer object in AD

ITS > Services > Research > VMs


## Current directions for Ubuntu 22.04 LTS

```shell
sudo apt-get update
sudo apt-get upgrade
sudo reboot
sudo apt-get install realmd sssd sssd-tools samba krb5-user packagekit adcli ntp
sudo realm --verbose join --user <admin_id> iowa.uiowa.edu
curl -k https://pe-rs.its.uiowa.edu:8140/packages/current/install.bash -O
chmod +x install.bash
sudo ./install.bash
sudo puppet agent --disable
sudo vi /etc/puppetlabs/puppet/puppet.conf
```

**edit puppet.conf to look like this**

```yaml
[main]
server = pe-rs.its.uiowa.edu
certname = <node name>.hpc.uiowa.edu
environment = puppet_rs_vms_hawkid
```

environment = branch
example: puppet_rs_vms_hawkid

## Run Puppet
```shell
sudo puppet agent --enable
sudo puppet agent -tv
```


## Notes
- we had issues with puppet at this point due to outdated versions of yaml config for docker package - FIXED in puppet_vmrepo
- duo_unix failed - do to an issue in `/etc/apt/sources.list.d`
```shell
cd /etc/apt/sources.list.d
rm -r duosecurity.list
```
- Ran update&upgrade
```shell
sudo apt update
sudo apt upgrade
```

---

Certainly! Here's a cleaner and more structured version of the text you provided, formatted for a Confluence article:

---

**Title: Kubernetes Node Setup (Modified from Cody's Run Book)**

**Source:** [Node Setup - HPC Admin - UIowa Wiki](https://uiowa.atlassian.net/wiki/spaces/hpcadmin/pages/76514368/Node+Setup)

---

### **Initial Setup Commands**
After installing the OS, execute the following commands. You may omit `sudo` if running as root. Although these steps can be integrated into Puppet in the future, they are currently standalone. Ensure the computer object is pre-created under the following path in AD:

**Path in AD:** ITS → Services → Research → IDAS → VMs

---

### **Commands for Ubuntu 22.04 LTS**

**System Updates and Package Installation:**
```shell
sudo apt-get update
sudo apt-get upgrade
sudo reboot
sudo apt-get install realmd sssd sssd-tools samba krb5-user packagekit adcli ntp
```

**Join the Realm:**
```shell
sudo realm --verbose join --user <admin_id> iowa.uiowa.edu
```

**Puppet Installation:**
```shell
curl -k https://pe-rs.its.uiowa.edu:8140/packages/current/install.bash -O
chmod +x install.bash
sudo ./install.bash
sudo puppet agent --disable
```

**Configure Puppet:**
```shell
sudo vi /etc/puppetlabs/puppet/puppet.conf
```
**Edit `puppet.conf` to reflect:**
```yaml
[main]
server = pe-rs.its.uiowa.edu
certname = <node name>.hpc.uiowa.edu
environment = puppet_rs_vms_hawkid
```

**Enable and Run Puppet:**
```shell
sudo puppet agent --enable
sudo puppet agent -tv
```

---

### **Troubleshooting Notes:**

- **Puppet Configuration Issues:** Initially, there were issues with Puppet configurations due to outdated YAML configurations for the Docker package, which have been resolved in `puppet_vmrepo`.
- **Duo Unix Installation Failure:** There was a problem with the Duo Unix installation due to an issue in `/etc/apt/sources.list.d`.
  ```shell
  cd /etc/apt/sources.list.d
  rm -r duosecurity.list
  ```
- **System Update and Upgrade Post-Fix:**
  ```shell
  sudo apt update
  sudo apt upgrade
  ```

---

This format structures your content in a way that enhances readability and clarity for the reader, making it easier to follow each step and understand the context of the commands used.