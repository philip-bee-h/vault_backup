---
title: "argon-node-deployment"
created: 2024-10-28
modified: 2024-10-28
type: process
system: hpc
tags:
  - argon
  - provisioning
  - burn-in
  - gpu
  - puppet
  - bios
  - warewulf
---

**Racking and cabling the hardware is now the responsibility of the RS team.**

## Provisioning Process

- Important:

    - Do not add the node to any hostgroups until it is ready for production.
    - The node must be added to WareWulf from `argon-itf-head.hpc.uiowa.edu`.


Steps:

1. MAC Address:
    1. Obtain the MAC address for the interface connected to the provisioning VLAN. Retrieve through BIOS, reach out to sys analyst (operations) to locate
2. IP Address:
    1. Determine the IP address using the 1104 spreadsheet (link to the spreadsheet).
3. Add the Node to WareWulf:
    1. You can add the node manually using `wwsh` 
	    1.  *`wwnodescan` has not been working*

Example using `wwsh`:

```Shell
   wwsh node new argon-itf-bx48-xx --netdev=eth0 --hwaddr=ac:1f:6b:41:66:3c --ipaddr=172.29.4.xxx --network=172.29.4.0 -G 172.29.6.109 -M 255.255.252.0 --groups=itf,infiniband
```

If you encounter an error where the command is unavailable, manually typing the command can resolve the issue in some cases.

1. Verify Node Addition:

After adding the node, confirm it was added correctly by running the following command:

```Shell
   wwsh node print argon-itf-ca39-06
```

Example Output:

```Shell
   argon-itf-ca39-06.hpc: ID               = 637
   argon-itf-ca39-06.hpc: NAME             = argon-itf-ca39-06
   argon-itf-ca39-06.hpc: eth0.HWADDR      = ac:1f:6b:41:66:3c
   argon-itf-ca39-06.hpc: eth0.IPADDR      = 172.29.7.223
   argon-itf-ca39-06.hpc: eth0.NETMASK     = 255.255.252.0
```

1. Provisioning:

Once BIOS boot mode is set to "Legacy" and boot order includes PXE, the node will be provisioned over the provisioning network.

1. Run Puppet:

Execute Puppet on `argon-head` and the new compute node. Ensure the node's hostname resolves.

---

## BIOS Configuration

Use the scripts `computeNodeBiosConf` and `gpuNodeBiosConf` located in `/opt/ohpc/admin/biostools` to configure BIOS via the Redfish API.

### Steps to Run the BIOS Script:

1. Navigate to the Directory:

First, change to the directory where the script is located:

```Shell
   cd /opt/ohpc/admin/biostools/
```

1. Run the Script:

Since the script `gpuNodeBiosConf` already has execute permissions (`-rwxr-x---`), you do not need to use `bash`. Instead, run the script directly:

```Shell
   sudo ./gpuNodeBiosConf hostname.ipmi
```

Example:

```Shell
   sudo ./gpuNodeBiosConf argon-itf-ca44-04.ipmi
```

This command runs the script, assuming `argon-itf-ca44-04.ipmi` is the node configuration or node name.

1. For Multiple Nodes:

If the script accepts multiple node names, you can specify them all, separated by spaces. For example:

```Shell
   sudo ./gpuNodeBiosConf argon-itf-ca44-04.ipmi argon-itf-ca44-05.ipmi argon-itf-ca44-06.ipmi
```

Important Notes:

- Ensure that you are in the correct directory before running the script.
- Make sure the node names or `.ipmi` files are properly specified.

---

## Burn-in and Benchmarking

 **Running CPU Burn-In and GPU Benchmarking Simultaneously**
### Step 1: SSH into the Node from `argon-itf-head`

```bash
ssh hostName
```

### Step 2: Start a New `tmux` Session
Create a single `tmux` session to run both tests simultaneously.

```bash
tmux new -s burn_in_test
```

### Step 3: Create a Temporary Directory and Navigate to It
Create a folder for storing logs from both the CPU and GPU tests.

```bash
mkdir -p ~/burn_in_tests
cd ~/burn_in_tests
```

### Step 4: Run CPU Burn-In Test
Start the CPU burn-in process. It will run in the background.

```bash
sys_basher -t 256 -ho 48 > cpu_log.txt &
```

- For a heavier load:
  ```bash
  sys_basher -t 512 -ho 48 > cpu_log_heavy.txt &
  ```

### Step 5: Run GPU Burn-In Test (For GPU Nodes)
If the node has a GPU, load the necessary modules and start the GPU benchmark:

```bash
module load stack gpu-burn
gpu_burn 3600 > gpu_burn.txt &
```

Now both the CPU and GPU tests are running concurrently, with their logs being written to `cpu_log.txt` and `gpu_burn.txt` respectively.

### Step 6: Detach from the `tmux` Session
You can detach from the session and let both tests continue running in the background:

```bash
Ctrl-b d
```

### Step 7: Monitor the Progress
To check on the status of both processes, reattach to the `tmux` session:

```bash
tmux attach -t burn_in_test
```

```shell
ps aux | grep -E '[s]ys_basher|[g]pu_burn'
```

You can also check the progress individually:

#### Check CPU burn-in:
```bash
ps aux | grep sys_basher
```

#### Check GPU burn-in:
```bash
ps aux | grep gpu_burn
```

### Step 8: Review the Logs
Once both tests are complete, review their logs to ensure there are no errors:

- CPU log:
  ```bash
  less cpu_log.txt
  ```

- GPU log:
  ```bash
  less gpu_burn.txt
  ```

### Step 9: Clean Up
Once the tests are done, and you’ve confirmed everything is working correctly, clean up the temporary files:

```bash
rm -rf ~/burn_in_tests
```

### Important Notes:
- **System Resources**: Running both tests at the same time will push the node’s resources to their limits. Be sure to monitor the system's temperature and performance metrics, especially in longer burn-in tests.
- **Temperature Monitoring**: You can monitor CPU and GPU temperatures using utilities like `nvidia-smi` for GPUs and `sensors` for CPUs.
- **Error Handling**: Check both the CPU and GPU logs for any signs of instability, such as overheating or component failure.

---

## Adding node to Hostgroup in Puppet

1. Add Node to Hostgroup:

Add the node to the appropriate hostgroup in the `sge_argon::hostgroup` Puppet class:

Path: `/reshpc/site-modules/sge_argon/manifests/hostgroup.pp`

- Add ticket number to git commit message
- `git commit -m "blah blah Ticket-number"`

This will trigger Puppet events that bring the node into production. Allow ~2 hours for the node to be ready.



---

## Need?

## Resource Configuration and Verification

1. Adjust Resource Specs for XDMod:

See Open XDMoD for the required adjustments.

1. Verification:

Run the following command on `argon-head` to verify Puppet ran successfully:

```Shell
   qstat -f -q QUEUE_NAME
```

You should see `load_avg` and `arch` populated, not `"-NA-"`.

Example Output:

```Shell
   queuename                      qtype resv/used/tot. load_avg arch          states
   *QUEUE_NAME*@argon-itf-bx48 BIPC  0/0/80         8.63     lx-amd64
```

## 

---

Post-Deployment Tasks

1. Update Tracking Spreadsheets:

Add the node and relevant details to the Argon Compute Node Tracking spreadsheet (found in Teams).

1. Update Queues and Policies:

If the node is new, add its resources to the 'Investor Queues' table and `all.q` in the Queues and Policies document.

1. Resource Configuration in Puppet (if needed):

If the node has resources that were previously unavailable (CPU, memory, GPU), ensure they are added in Puppe