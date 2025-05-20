---
type: reference
refrenceName: switch-config-via-serial-cli-macos
tags: 
source: "{{source}}"
date: 2024-10-01
---
**Guide for connecting to a console by serial on a Mac**
## Prerequisites
- macOS (iTerm2 or macOS Terminal)
- Console cable (RJ-45 RS232)
- USB-to-RS232 adapter (if needed)

## Step-by-Step Guide

### 1. Connect the Console Cable
- **Connect one end** of the console cable to your switch’s console port.
- **Connect the other end** to your Mac using a USB-to-RS232 adapter if necessary.

### 2. Identify the Serial Port on macOS
- **Open Terminal** or **iTerm2**.
- Run the following command to list available serial devices:
  ```bash
  ls /dev/tty.*
  ```
- Look for an entry like `/dev/tty.usbserial` or `/dev/tty.usbmodem` (this represents your connected adapter). For example, you might see:
  ```
  /dev/tty.usbserial-10
  ```

### 3. Install and Use `screen` for Terminal Emulation
- macOS has `screen` pre-installed, which can be used for terminal emulation.
- Start a terminal session using `screen`, replacing `tty.usbserial` with your actual serial device:
  ```bash
  screen /dev/tty.usbserial-10 115200
  ```
  This command sets the connection to a baud rate of 115200 bps, with 8 data bits, no parity, 1 stop bit, and no flow control (default behavior of `screen`).

### 4. Log in to the Switch
- Once connected, the switch should prompt for a username:
  - **Username**: `admin`
  - **Password**: Press **Enter** (no password is set for initial access).
- **Note**: If you do not see a prompt, try pressing **Enter** a few times to trigger the prompt.
- Follow the on-screen prompts to set a new password.

### 5. Configure the IP Address
After logging in, you can configure the IP address using one of the following options:
- **Option 1**: Manually assign a static IP address to the service port.
- **Option 2**: Configure the switch as a DHCP client for the service port.
- **Option 3**: Assign a static IP address to an Ethernet port.
- **Option 4**: Configure the switch as a DHCP client for an Ethernet port.

### 6. Exit the `screen` Session
- To exit the screen session, press `Ctrl-A`, then `Ctrl-\`, and confirm.

## Troubleshooting Common Issues

### Issue: "cannot open line '/dev/tty.usbserial-10' for r/w: resource busy"
- **Cause**: Another process is already using the `/dev/tty.usbserial-10` port.

#### Steps to Resolve:
1. **Check for Other Processes Using the Port**
   ```bash
   lsof | grep tty.usbserial-10
   ```
   This will list any processes using the port.

2. **Terminate the Process**
   If any processes are listed, note their Process ID (PID) and terminate them:
   ```bash
   sudo kill -9 <PID>
   ```
   Replace `<PID>` with the actual process ID.

3. **Retry Connecting with `screen`**
   After terminating the process, retry the screen command:
   ```bash
   screen /dev/tty.usbserial-10 115200
   ```

4. **Reboot as a Last Resort**
   If the issue persists, try rebooting your Mac. This can clear any lingering locks on the serial device.

### Example of Checking and Terminating a Process
- If you run `lsof | grep tty.usbserial-10` and see:
  ```
  screen    33226 hawrth    5u      CHR                9,8        0t6                 731 /dev/tty.usbserial-10
  ```
  You can terminate the screen process:
  ```bash
  kill -9 33226
  ```

## Commands

- To find the ip of the service port (OOB) 
```shell
show serviceport
```

- To find the IP address of the management interface
```shell
show ip interface
```

