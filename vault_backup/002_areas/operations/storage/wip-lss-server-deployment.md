---
title: wip-lss-server-deployment
created: 2024-10-30
modified: YYYY-MM-DD
type: process
system: HPC|Storage|IDAS|Infrastructure
tags:
  - process
  - procedure
  - uiowa
related_docs: 
status: in-progress
---


# WIP - setting-up-lss-server


set two ssds as bootable drives
set up partition left unformated

very bottom
create software raid 

name mdo = md raid
raid level mirriored 
- only two disk

selcect two partiotions we made

go down to create

partition mdo free space

format ext4

mount/

/boot - 1G
/var - 5G
/temp - 75G
/     - that is everything else 

zfs dont use swap

grep -v 

-v ignore, everything but


# LSS Server Setup
- Open RemoteAdmin
- Navigate to Raratan in firefox browser
- add as bookmark
- hawkID to log in 
- navigate to storage on the left handside
- locate storage your working on
- Open KVM
- you might need to select options and "allow pop up"
- select input
- add macro F2, F11 and F12
- Boots in UEFI shell
- `exit`
- Bios
- Nav to BMC (ipmi)
	- Network config
		- Update lan config - yes
		- config address source - static
		- enter info from hostMaster
		- Disable IPV6 support
- esc
- nav to Boot
- try or install Ubuntu
- ubuntu server

formatting disks
- smaller to disk for os x 512
- larger ssd will be the log x 800
- first of the small disk enter - add as boot device
- hit enter - nav back to other small disk - add as another boot device
- back to first small disk again go to free space (do this to both)
- add gpt partition
- format - leave unformated
- create
- create software raid
- raid lvl 1 mirrored
- small disk partition 2 select both
- create
- free space under md0
- add gpt partition
- size: 1g
- format: ext4
- mount: /boot 
- create
- free space under mod 0
- add gpt
- 5g
- ext4
- other /tmp 
- create
- free space under mod0
- add gpt
- 75g
- ext4
- /var 
- free space under mod0
- add gpt
- all avail amout
- mount `/` 
- create

profile set up
- ubuadmin
- server name
- user - ubuadmin
- password in PWstate

- skip ubuntu pro set up
- install openssh

Install

## Once in 
- `sudo -i`
- `passwd`
- make the password for root the same ad ubuadmin
- test by running `su -`
- exit - dont do things as root
# Draft 1

Here's a streamlined and formatted guide for installing Ubuntu Server using the specific steps and tools mentioned in your transcript:

### **Ubuntu Server Installation Guide**

#### **Preparation and Access**
1. **Open RemoteAdmin Desktop Client**:
   - Launch Firefox and navigate to the Raritan management interface.
	   - *Bookmark the page for easy access.*
   - Log in using your hawkID.
	   - Select `input` from the top menu bar
		   - Configure the KVM input settings by adding macros for F2, F11, and F12 keys.

2. **Access Storage and KVM**:
   - On the left-hand side, select the specific storage you are working on.
   - Open the KVM console. You may need to adjust browser settings to allow pop-ups.


#### **BIOS Configuration**
1. **Access UEFI Shell**:
   - The server boots into the UEFI shell. Type `exit` to proceed to the BIOS.

2. **Network Configuration in BIOS**:
   - Navigate to the BMC (IPMI) settings.
   - Under Network Configuration:
     - Set "Update LAN Configuration" to Yes.
     - Set "Configuration Address Source" to Static.
     - Input network details as provided by hostMaster.
     - Disable IPV6 support.
   - Exit the Network settings.

3. **Boot Settings**:
   - Navigate to the Boot menu option at the top of BIOS
   - ???

#### **Installing Ubuntu Server**
1. Select **custom storage layout**
1. **Partitioning and Formatting Disks**:
   - **OS Disk**: Assign the smaller disk (512 GB suggested) for the operating system. Set it as the boot device.
   - **Log Disk**: Use the larger SSD (800 GB recommended) for logging purposes.
   - Navigate back to the first small disk:
     - In the free space, add a GPT partition but leave it unformatted for now.
     - Create a software RAID:
       - Level: RAID 1 (Mirrored).
       - Select both small disk partitions.
   - Configure partitions under `md0`:
     - `/boot`: 1 GB, formatted as ext4.
     - `/tmp`: 5 GB, formatted as ext4.
     - `/var`: 75 GB, formatted as ext4.
     - `/`: Use all remaining space, formatted as ext4.

#### **Final Configuration**
1. **Profile and User Setup**:
   - Set the server name and create a user named `ubuadmin`.
   - Set up the password (retrieved from PWstate).

2. **Software Installation**:
   - Opt to skip the Ubuntu Pro setup if prompted.
   - Install OpenSSH server to enable remote management.

#### **Post-Installation**
1. **Reboot and Initial Login**:
   - After installation, reboot the server.
   - Log in with the username `ubuadmin` and the password set during installation.
	   - `heQuakEStr1ke`

2. **Verify Installation**:
   - Check all systems and services to ensure they are running as expected.
   - Perform any necessary updates or configurations specific to your environment.

This guide incorporates detailed steps specific to your hardware and software setup, ensuring a thorough and customized installation process for Ubuntu Server.




# LSS Server Setup
- Open RemoteAdmin
- Navigate to Raratan in firefox browser
	- add as bookmark
	- hawkID to log in 
- navigate to storage on the left handside
	- locate storage your working on
- Open KVM
	- you might need to select options and "allow pop up"
- select input
	- add macro F2, F11 and F12
- Boots in UEFI shell
	- `exit`
- Bios
	- Nav to BMC (ipmi)
		- Network config
			- Update lan config - yes
			- config address source - static
			- enter info from hostMaster
			- Disable IPV6 support
- esc
- nav to Boot
- try or install Ubuntu
- ubuntu server

formatting disks
- smaller to disk for os x 512
- larger ssd will be the log x 800
- first of the small disk enter - add as boot device
- hit enter - nav back to other small disk - add as another boot device
- back to first small disk again go to free space (do this to both)
	- add gpt partition
	- format - leave unformated
	- create
- create software raid
	- raid lvl 1 mirrored
	- small disk partition 2 select both
	- create
- free space under md0
	- add gpt partition
	- size: 1g
	- format: ext4
	- mount: /boot 
	- create
- free space under mod 0
	- add gpt
	- 5g
	- ext4
	- other /tmp 
	- create
- free space under mod0
	- add gpt
	- 75g
	- ext4
	- /var 
- free space under mod0
	- add gpt
	- all avail amout
	- mount `/` 
	- create

profile set up
- ubuadmin
- server name
- user - ubuadmin
- password in PWstate

- skip ubuntu pro set up
- install openssh

Install

## Once in 
- `sudo -i`
- `passwd`
	- make the password for root the same ad ubuadmin
- test by running `su -`
- exit - dont do things as root
- 
# Setting Up an LSS Server on Ubuntu 22.04

This guide will walk you through the steps to set up an LSS server using Ubuntu 22.04, focusing on SSD configuration and RAID setup.

### Prerequisites
- Two SSDs for installation
- Ubuntu 22.04 installer

### Step 1: Prepare SSDs
1. **Initialize SSDs as Bootable Drives:** When booting from the Ubuntu installer, select each SSD and mark it as a bootable drive.

2. **Partition SSDs:**
   - Leave the partitions unformatted initially.

### Step 2: Configure Software RAID
1. **Create the RAID Array:**
   - In the RAID configuration menu, create a new RAID array named `md0`.
   - Set RAID level to "mirrored" (RAID 1).
   - Select the two partitions you prepared on the SSDs.

2. **Proceed to Create the Array:**
   - Confirm the creation to initialize the RAID array.

### Step 3: Partition the RAID Array
1. **Navigate to the free space on `md0`:**
   - Format this space with `ext4`.

2. **Assign Mount Points:**
   - Allocate mount points and sizes as follows:
     - `/boot` - 1GB: for boot files.
     - `/var` - 5GB: for variable data.
     - `/temp` - 75GB: for temporary files.
     - `/` (root) - assign all remaining space for system and application files.

### Step 4: Final Configuration
- **Avoid using a swap partition on ZFS:** It’s advisable not to configure a swap space when using ZFS to ensure system stability and performance.

### Additional Commands
- To filter out specific outputs in command line operations, use the `grep -v` command. This option excludes lines containing the specified pattern.

   ```bash
   grep -v 'pattern' filename
   ```

# Draft 2
----

It looks like your notes cover a detailed server installation process, including tasks related to ZFS pool creation, time synchronization, SSH setup, and Puppet configuration. Let’s break this down into clearer sections to improve organization and readability. I will also include context where appropriate:

---

## 1. Initial Setup

- **Run system updates**
  ```bash
  sudo apt update && sudo apt upgrade
  ```

- **Set the hostname**
  ```bash
  sudo hostnamectl set-hostname <your-hostname>
  ```

- **Install ZFS utilities**
  ```bash
  sudo apt install zfsutils-linux
  ```

---

## 2. ZFS Pool Creation

### Identify Disks

1. **List all block devices**:
   ```bash
   lsblk
   ```

2. **Count the disks available for the pool**:
   ```bash
   ls -alh /dev/disk/by-id | egrep 'wwn' | grep -Ev 'part|0x55cd' | grep -v 'sd[a,b]$' | wc -l
   ```
   - This command helps ensure the disk count matches the expected number (multiples of 18).

3. **List available disks** (excluding boot drives like `sda`, `sdb`):
   ```bash
   ls -alh /dev/disk/by-id | grep 'wwn' | grep -Ev 'part|0x55' | grep -v sd[a,b]
   ```

4. **Verify the output is WWN names only**:
   ```bash
   ls -alh /dev/disk/by-id | grep 'wwn' | grep -Ev 'part|0x55' | grep -v sd[a,b] | awk '{print $9}'
   ```

### Split Disk Names into Groups of 18

1. **Generate files with disk names in groups of 18**:
   ```bash
   ls -alh /dev/disk/by-id | grep 'wwn' | grep -Ev 'part|0x55' | grep -v sd[a,b] | awk '{print $9}' | split -l 18
   ```

2. **Verify the count in the generated files**:
   ```bash
   cat xaa
   wc -l xaa
   ```

### Create ZFS Pool

1. **Create the ZFS pool**:
   ```bash
   zpool create dpool01 raidz3 $(cat xaa) raidz3 $(cat xab) raidz3 $(cat xac) raidz3 $(cat xad)
   ```

2. **Verify the correct disk size is used**:
   ```bash
   lsblk | grep -v 10.9 | grep ^sd
   ```

### ZFS Pool for ZIL (SSD or NVMe)

- **Identify SSD or NVMe drives**:
  - For older systems (SATA SSDs):
    ```bash
    ls -alh /dev/disk/by-id | egrep 'sdak|sdal'
    ```

  - For newer systems (NVMe drives):
    ```bash
    ls -alh /dev/disk/by-id | egrep 'nvme'
    ```

- **Add SSD/NVMe for ZIL**:
  - With WWN:
    ```bash
    zpool add dpool01 log mirror wwn-0x55cd2e415248be70 wwn-0x55cd2e415248be76
    ```
  - With NVMe:
    ```bash
    zpool add dpool01 log mirror nvme-Micron_7450_MTFDKBA800TFS_234946CA9614 nvme-Micron_7450_MTFDKBA800TFS_234946CA962E
    ```

### Export and Import the ZFS Pool

- **Export the pool**:
  ```bash
  zpool export dpool01
  ```

- **Import the pool using disk paths**:
  ```bash
  zpool import -d /dev/disk/by-id dpool01
  ```

---

## 3. Time Synchronization

1. **Configure NTP servers**:
   ```bash
   sudo vi /etc/systemd/timesyncd.conf
   ```
   Add the following under the `[Time]` section:
   ```
   NTP=128.255.3.84 128.255.3.100 128.255.3.116
   ```

2. **Restart the time sync service**:
   ```bash
   sudo systemctl restart systemd-timesyncd
   ```

3. **Verify time sync status**:
   ```bash
   timedatectl
   ```

---

## 4. SSH Key Setup

1. **Switch to root**:
   ```bash
   sudo -i
   ```

2. **Generate an RSA keypair as root**:
   ```bash
   ssh-keygen
   ```
   - Use default options with a blank password.

---

## 5. Puppet Setup (For LSS Servers)

1. **Create Puppet facts directory**:
   ```bash
   mkdir -p /etc/facter/facts.d
   ```

2. **Test Puppet agent**:
   - Run Puppet in noop mode:
     ```bash
     sudo /opt/puppetlabs/bin/puppet agent -t --noop
     ```

   - Run with verbose output:
     ```bash
     sudo /opt/puppetlabs/bin/puppet agent -tv
     ```

---

## 6. Security and Monitoring (Wazuh Installation)

1. **Transfer GPG key**:
   ```bash
   scp ~/Downloads/GPG-KEY-WAZUH itf-rs-store38.hpc.uiowa.edu:~/
   ```

2. **Import the GPG key**:
   ```bash
   cat GPG-KEY-WAZUH | sudo gpg --no-default-keyring --keyring gnupg-ring:/etc/apt/trusted.gpg.d/wazuh.gpg --import
   sudo chmod 644 /etc/apt/trusted.gpg.d/wazuh.gpg
   ```

3. **Install Wazuh agent** (on LSA server):
   - Download the required version:
     ```bash
     apt download wazuh-agent=3.13.2-1
     ```
   - Transfer it to the LSA server:
     ```bash
     scp wazuh-agent_3.13.2-1_amd64.deb <lsa-server>:/tmp
     ```
   - Install:
     ```bash
     sudo apt install ./wazuh-agent_3.13.2-1_amd64.deb
     ```

---

These are the primary tasks extracted from your notes, organized in a more structured manner. Let me know if you need further clarification or any additional details!