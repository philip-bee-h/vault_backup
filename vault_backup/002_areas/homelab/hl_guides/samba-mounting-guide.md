---
title: samba-mounting-guide
created: 2024-12-17
type: process
system: 
tags:
  - process
  - procedure
  - homelab
related_docs:
---
### Samba Mounting Guide for Homelab

#### 1. Verify Samba Server is Running

Ensure that the Samba server is installed and running on your storage server. You can check its status with:

```bash
sudo systemctl status smbd
```

Server IP: **192.168.10.23**

#### 2. Create a Samba Share

Make sure you have created a Samba share (e.g., `MediaShare`) on your server. Verify the share is accessible:

```bash
smbclient -L //192.168.10.23 -U guest
```

#### 3. Install CIFS Utilities on Client Machines

Install the necessary tools for mounting Samba shares on your ripping station and Plex server:

```bash
sudo apt update
sudo apt install cifs-utils -y
```

- **`smbclient`** _(Optional)_: For testing and troubleshooting Samba connections. It's not required for mounting but useful for diagnostics.
```sh
sudo apt install smbclient -y
```

For Fedora-based systems:

```bash
sudo dnf install cifs-utils -y
```

#### 4. Test Connection to Samba Share

Before mounting, test the connection:

```bash
smbclient //192.168.10.23/MediaShare -U guest
```

#### 6. Create the Mount Point (If Needed)

If the local directory `/srv/media` does not exist, create it:

```bash
sudo mkdir -p /srv/media
```

#### 5. Mount the Share

Mount the Samba share on both the ripping station and Plex server:

```bash
sudo mount -t cifs //192.168.10.23/MediaShare /srv/media -o guest,uid=1000,gid=1000
```

#### 7. Make the Mount Persistent (Optional)

To ensure the Samba share mounts automatically at boot, add the following line to `/etc/fstab` on each client machine:

```plaintext
//192.168.10.23/MediaShare /srv/media cifs guest,uid=1000,gid=1000 0 0
```

#### 8. Test the `/etc/fstab` Configuration

Test the configuration to ensure there are no errors:

```bash
sudo mount -a
```

If there are issues, review the `/etc/fstab` entry and check system logs for errors:

```bash
journalctl -xe
```

#### 9. Verify the Mount

Confirm the share is successfully mounted:

```bash
df -h | grep /srv/media
```

Your Samba share is now mounted and ready for use on both your media ripping station and Plex server.