---
title: idas-node-deployment
created: 2024-10-31
modified: 2024-10-31
type: process
system:
  - idas
tags:
  - process
  - procedure
  - uiowa
  - idas
  - deployment
related_docs:
---
# Deployment Steps

Puppet node.yaml
NIC Firmware
Install Puppet
Add tio k8
Add node label
ipmi config
Argon DNS

## IP Info needed
### compute14 info
ip: 172.30.21.96
ipmi: 172.30.21.98
1104: 172.29.21.96
1108: 192.168.64.38
ipmi config
argon dns


## Thoughts on racking

- pull drives and reseat for good measure
---
## BIOS Setup

### BIOS and IPMI Configuration for IDAS Node

### Accessing the BIOS

1. **Login to Remote System**
   - Log in to the node via **RDP/Windows**.
   
2. **Access Raritan Interface**
   - Open **Raritan** in your web browser.
   - Locate the node under the **Nodes** section.

---

### BIOS Settings

Verify and configure the following BIOS settings:

1. **CPU Configuration**
   - Go to **Advanced > CPU Configuration**.
     - **Enable SVM**: Ensure this is enabled.

2. **Power Management**
   - Go to **Advanced > APM Configuration**.
     - **Restore AC Power Loss**: Set to **Power On**.

3. **AMD CPU Settings**
   - Go to **Advanced > AMD CBS > CPU Common Options**.
     - **Performance**: Confirm **SMT Control** is set to **Enable**.

4. **AMD Northbridge I/O (NBIO) Settings**
   - Go to **Advanced > AMD CBS > NBIO Common Options**.
     - **IOMMU**: Set to **Disabled** (confirm if this setting is required in your environment).
   - Go to **SMU Common Options** within NBIO:
     - **TDP Control**: Set to **Manual**.
     - **TDP**: Set to **400**.
     - **PPT**: Set to **400**.
     - **Determinism Enable**: Set to **Performance**.
     - **Power Profile Selection**: Set to **High Performance**.

5. **Secure Boot**
   - Ensure **Secure Boot** is **Disabled** to avoid potential driver conflicts.

---

### IPMI Configuration

1. **Enable IPv4 Support**
   - In IPMI settings, ensure **IPv4 Support** is enabled.

2. **DM_LAN Configuration**
   - **Address Source**: Select **Static**.
     - **Station IP Address**: Enter the designated IPMI address.
     - **Subnet Mask**: Set to **255.255.255.0**.
     - **Router IP Address**: Set to the network gateway address.

3. **Exit and Save**
   - Go to the **Exit** tab.
   - Select **Save Changes & Reset** to apply all settings.

---

### Additional Notes

- **Boot Menu**: For ASUS servers, press **F8** during boot to access the boot menu.

---
## OS Install

1. **Boot from Ubuntu Server Installation Media**
   - Insert or connect the Ubuntu Server installation media (USB or network).
   - Start the node and boot into the Ubuntu Server installer.

2. **Select Installation Type**
   - Choose **Ubuntu Server** (normal installation image).

3. **Network Configuration**
   - Configure network interfaces and VLAN settings as follows:

	- **Create a Bonded Network Interface**
		- Select **Create Bond** for high-availability network interface bonding.
		- Choose the NICs (Network Interface Cards) to include in the bond.
		- Configure the bond settings as per your IDAS specifications.

	- **Add a VLAN to the Bonded Interface**
		- After creating the bond, select it to **Add VLAN**.
		- **VLAN Tag**: Enter the specified VLAN tag value (e.g., `<vlan>`).
		- **VLAN Name**: Set an identifiable name if needed (optional).

	- **Configure VLAN IP Settings**
		- Once the VLAN is added, navigate to its settings to configure IPv4 manually:
			 - **IPv4 Method**: Select **Manual**.
			 - **IP Address**: Set to `<172.29.4.104>`
			 - **Subnet**: Specify `<172.29.4.0/22>`
			 - **Gateway**: Set to `<172.29.6.109>`
			 - **Name Server**: Use the gateway as the DNS server, `<172.29.6.109>`.
			 - **Search Domain**: Leave this field blank.

4. **Set University Mirrors**
   - When prompted to configure mirror settings, choose the university’s specific Ubuntu mirror
   - Change the default mirror to `http://mirrors.uiowa.edu/pub/ubuntu`

5. **Storage Configuration**
   - For disk configuration:
	 - **Partitioning Method**: Select **Use entire disk**.
	 - **Disk Selection**: Choose the smaller of the available disks for the system installation.
	 - **Volume Settings**:
		   - In **device ubuntu-lv** (logical volume), ensure the value is zeroed out to use all remaining free space by default.

6. **User Profile Setup**
   - Configure basic user and system details:
	 - **Your Name**: Enter `**idasadmin**`.
	 - **Server Name**: Set the **hostname** to something descriptive of the node’s purpose.
	 - **Username**: Use `idasadmin`.
	 - **Password**: For simplicity, use `123456` (or follow your security policy if this needs updating).

7. **Install OpenSSH**
   - Enable the **OpenSSH Server** option during setup to ensure remote management capabilities.
---
## Node Steup

### **Add node to ADUC**
1. **Log in to Remote Admin**  
    *Access the remote admin environment where you can manage Active Directory.*
2. **Open Active Directory Users and Computers (ADUC)**  
    *Launch the **ADUC** tool from your system.*
3. **Navigate to the Computer Objects Folder**
	- In the left pane, go to **DataCtr > RS > Server**.
4. **Create a New Computer Object**
    - Right-click on **Servers** and select **New > Computer**.
    - Enter the **hostname** in the designated field (use the format **`<hostname>`**).

### **System Updates and Package Installation:**
```shell
sudo apt-get update
sudo apt-get upgrade
sudo reboot
sudo apt-get install realmd sssd sssd-tools samba krb5-user packagekit adcli ntp
```

### **Configuring Kerberos Authentication - Leave blank**
![[idas-node-deployment-1.png]]

### **Join the Realm:**

```shell
sudo realm --verbose join --user <admin_id> iowa.uiowa.edu
```

### **Puppet Installation:**
*Run commands one at a time*
```shell
curl -k https://pe-rs.its.uiowa.edu:8140/packages/current/install.bash -O chmod +x install.bash 
sudo ./install.bash 
sudo puppet agent --disable
```

### **Configure Puppet:**

```shell
sudo vi /etc/puppetlabs/puppet/puppet.conf
```

**Edit** `puppet.conf` to reflect:

```yaml
[main]
server = pe-rs.its.uiowa.edu
certname = <node name>.hpc.uiowa.edu
environment = resk8_ubuntu
```

### **Create node.yaml**
**Clone Repo**

**Link to repo:**
```shell
https://git.uiowa.edu/its-rs/puppetcode/controlrepo/puppet-resk8
```

**navigate to repo:**
```shell
cd puppet-resk8
```

**Copy file to to same dir with new name**

```shell
cp /data/nodes/rs-k8-compute-template.hpc.uiowa.edu.yaml /data/nodes/<hostName>-template.hpc.uiowa.edu.yaml
```

**Editing the node.yaml**

![[idas-node-yaml.png]]

**Confirm `site.pp` accommodates your new node in the node classifier**

```shell
/manifests/site.pp
```

![[idas-site-pp.png]]

**Commit changes**
```shell
git add .
git commit -m "created node.yaml for <hostname>"
git push
```

### **Run puppet on your local machine**

**Puppet log in**
```shell
puppet-access login --lifetime=4h <hawkid>
```

**Deploy**
```shell
puppet-code deploy <repo> --wait
```


### **Enable and Run Puppet on the node:**

```shell
sudo puppet agent --enable 
sudo puppet agent -tv
````

----
# 2024-11-15 continuation 

- Make sure to reboot after first puppet run
	- to refresh network config
	- to refresh nvidia drivers

Check GPU count

```sh 
nvidia-smi
```
- confirm gpu count matches expected count

**edit the kubelet extra config file**
```sh
sudo vi /etc/default/kubelet
```
**The file should have the following contents**
```sh
KUBELET_EXTRA_ARGS=  --node-ip=<K8 Net IP>
```
### Turn off swap  
```sh
sudo swapoff -a
```

**comment swap out in fstab** (Screen shot)

![[CleanShot 2024-11-20 at 15.33.24.png]]

```sh
sudo /etc/fstab
```

### Setup NVMe Drive

1. **Identify the NVMe Disk**  
   Start by listing all disks and identifying the NVMe drive:
```sh
sudo fdisk -l
```

   This will show you all available disks. Look for something like `/dev/nvme0n1` with the correct size, in this case, `3.49 TiB`.

2. **Partition the Disk**  
   Next, use `fdisk` to create partitions on the NVMe drive:
```sh
sudo fdisk /dev/nvme0n1
```

   Follow the interactive steps to create the partition. Here are some common steps:
   - g - for gpt partition
   - n - new partition
   - defaults - enter on every thing else
   - w - write and exit



1. **Format the Partition**  
   After creating the partition (e.g., `/dev/nvme0n1p1`), format it with `ext4`:
```sh
sudo mkfs.ext4 /dev/nvme0n1p1
```

4. **Identify the Partition UUID**  
   Use `blkid` to get the UUID of the new partition:
```sh
sudo blkid
```

   Look for the line corresponding to `/dev/nvme0n1p1` and note the UUID.

5. **Edit `/etc/fstab` to Mount the Partition Automatically**  
   Open the `/etc/fstab` file with a text editor to add the partition for automatic mounting:
```sh
sudo vim /etc/fstab
   ```

   Add the following line to the file, replacing `<your-uuid>` with the actual UUID from the previous step:
```sh
/dev/disk/by-uuid/<your-uuid> /var/Lib/containerd ext4 defaults 0 1
```

   Save the file 

6. **Mount the Partition**  

```sh 
cd /var/lib/containerd/
```

   Mount the partition using the following command:
```sh
sudo mount -a
```

7. **Verify the Mount**  
   Ensure the partition is mounted correctly by running:
```sh
cat /proc/mounts
```

This will show all currently mounted file systems. Look for your NVMe partition (e.g., `/mnt/my_nvme`).

**expected output**
```sh
/dev/nvme0n1p1 /var/lib/containerd ext4 rw,relatime,stripe=32 0 0
```


```sh
sudo systemctl restart containerd
```

### Add cluster  

create token run on 

**On Head01/02**

```sh
sudo kubeadm token create --print-join-command
```

Expected output
```sh
kubeadm join 192.168.64.15:8443 --token <token> --discovery-token-ca-cert-hash <cert hash>
```

```sh
sudo systemctl enable kubelet
```

input the token from Head01/02
```sh
sudo kubeadm join 192.168.64.15:8443 --token <token> --discovery-token-ca-cert-hash <cert hash>
```

If the above runs successfully you can verify the node has been added back on a **head node** it will take a few moments for the nodes to become ready

**On Head01/02** confirm node has joined the cluster
```sh
kubectl get nodes
```


https://uiowa.atlassian.net/wiki/spaces/hpcadmin/pages/76513974/Configure+Kubectl+to+connect+to+the+cluster
### Add Node Labels



# Rough notes



## Bios setup
**BIOS Config**

- Log in to RDP/Windows 
- Access Raitan through your browser
- locate node under `Nodes`

**BIOS Settings**

Some settings only require verification

- [x] Advanced -> CPU Configuration -> Enable SVM  
- [x] Advanced -> APM Configuration -> Restore AC Power Loss _> Power On  
- [x] Advanced -> AMD CBS -> CPU Common Options -> Performance -> SMT Control Enable  
- [x] Advanced -> AMD CBS -> NBIO Common Options -> IOMMU -> Disabled  ????
- [x] Advanced -> AMD CBS -> NBIO Common Options-> SMU Common Options ->  TDP Control Manual  
- [x] Advanced -> AMD CBS -> NBIO Common Options -> SMU Common Options -> TDP 400  
- [x] Advanced -> AMD CBS -> NBIO Common Options -> SMU Common Options -> PPT 400  
- [x] Advanced -> AMD CBS -> NBIO Common Options -> SMU Common Options -> Determinism Enable -> Performance  
- [x] Advanced -> AMD CBS -> NBIO Common Options -> SMU Common Options -> Power Profile Selection -> High Performance
- [ ] make sure secure boot is off - can cause issues with drivers

**IPMI Config**

- Configure ipv4 support
- dm_lan
	- Configuration address source
		-  select `static`
		- station -> ipmi address
		- subnet mask -> 255.255.255.0
		- Router IP address -> gateway

- Exit tab
	- save changes & reset

ASUS Servers F8 for boot menu


## Puppet node.yaml
## NIC Firmware
## Install Puppet
## Add tio k8
## Add node label

## ipmi config
## Argon DNS



# Node Setup

**Owned by:** Cody Johnson  
**Last updated:** Mar 20, 2024  
**Read time:** 3 min  
**View count:** 8  
**Editor:** Legacy editor  

Commands to run once OS is installed. `sudo` can be excluded if run as root. Some of these steps can probably be moved into Puppet, but for now they are standalone. The computer object should be pre-created in the ITS → Services → Research → IDAS → k8-nodes OU.

### Current directions for Ubuntu 20.04 LTS

```bash
sudo apt-get update
sudo apt-get upgrade
sudo reboot
sudo apt-get install realmd sssd sssd-tools samba krb5-user packagekit adcli ntp
```


```shell
sudo realm --verbose join --user <admin_id> iowa.uiowa.edu
curl -k https://pe-rs.its.uiowa.edu:8140/packages/current/install.bash -O
chmod +x install.bash
sudo ./install.bash
sudo puppet agent --disable
```
Edit the Puppet configuration file:

```bash
sudo vi /etc/puppetlabs/puppet/puppet.conf
```

Update `puppet.conf` to look like this:

```ini
[main]
server = pe-rs.its.uiowa.edu
certname = <node name>.hpc.uiowa.edu
environment = resk8_ubuntu
```

### Node.yaml
Clone Repo

Link to repo:
```shell
https://git.uiowa.edu/its-rs/puppetcode/controlrepo/puppet-resk8
```

navigate to repo:
```shell
cd puppet-resk8
```

Copy file to to same dir with new name
```shell
cp /data/nodes/rs-k8-compute-template.hpc.uiowa.edu.yaml /data/nodes/<hostName>-template.hpc.uiowa.edu.yaml
```

Editing the node.yaml

![[idas-node-yaml.png]]

```shell
git add .
git commit -m "created node.yaml for <hostname>"
git push
```

Confirm `site.pp` accommodates your new node in the node classifier
```shell
/manifests/site.pp
```

![[idas-site-pp.png]]










---

### Network Interface Renaming

Add the udev rule to rename the network interface that will connect to the K8 network to `enp129s0f0`. This step is required to make the K8 network work correctly. (Adding the udev rule should be done via Puppet now).

```bash
sudo vi /etc/udev/rules.d/70-persistent-net.rules
```

The `70-persistent-net.rules` file should contain the following:

```plaintext
SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="interface mac address", NAME="enp129s0f0"
```

### Kubelet Extra Config File

Edit the kubelet extra config file:

```bash
sudo vi /etc/default/kubelet
```

Update the file with the following contents:

```plaintext
KUBELET_EXTRA_ARGS= --node-ip=<K8 Net IP>
```

### Finalizing Setup with Puppet

Reboot, then enable and run Puppet to complete the configuration:

```bash
sudo reboot
sudo puppet agent --enable
sudo puppet agent -tv
```

Once Puppet has run (it usually takes a few runs for all components to fit together), add the node to the cluster.

### Joining the Cluster

**On Head01/02:**

Generate the join command:

```bash
kubeadm token create --print-join-command
```

The output will look like this:

```plaintext
kubeadm join 192.168.64.15:8443 --token <token> --discovery-token-ca-cert-hash <cert hash>
```

**On New Node:**

```bash
sudo swapoff -a
sudo systemctl enable kubelet
kubeadm join 192.168.64.15:8443 --token <token> --discovery-token-ca-cert-hash <cert hash>
```

After joining, you can verify the node has been added back on a head node (it may take a few moments for nodes to become ready):

```bash
kubectl get nodes
```

Example output:

```plaintext
NAME                            STATUS     ROLES                  AGE      VERSION
rs-k8-app01.hpc.uiowa.edu       Ready      frontend               2y236d   v1.22.5
rs-k8-app02.hpc.uiowa.edu       Ready      frontend               2y236d   v1.22.5
rs-k8-compute01.hpc.uiowa.edu   Ready      compute                2y235d   v1.22.5
rs-k8-compute02.hpc.uiowa.edu   Ready      compute                2y235d   v1.22.5
rs-k8-compute03.hpc.uiowa.edu   Ready      compute                728d     v1.22.5
rs-k8-compute04.hpc.uiowa.edu   Ready      compute                714d     v1.22.5
rs-k8-compute05                 Ready      compute                274d     v1.22.5
rs-k8-head01                    Ready      control-plane          98d      v1.22.5
rs-k8-head02                    Ready      control-plane          98d      v1.22.5
```

### Labeling the Node

Label the node with its role (e.g., compute, app, or util). The example below is for a compute node. The `jupyterRole` label will also match the node role (this will be updated to `idasRole` in the near future):

```bash
kubectl label node <nodename> node-role.kubernetes.io/compute=
kubectl label node rs-k8-util01 jupyterRole=util
```
```