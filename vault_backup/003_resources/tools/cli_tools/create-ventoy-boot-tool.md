---
title: Create a USB Stick with Multiple Linux Install Images using Ventoy
created: 2024-06-30
modified: 2024-10-30
type: process
system:
  - linux
tags:
  - process
  - procedure
  - linux
related_docs:
---

## Resource Link 
- https://www.dwarmstrong.org/ventoy/


# Create a USB Stick with Multiple Linux Install Images using Ventoy


When you want to try out a Linux distribution on a physical machine, the traditional method involves downloading the `.iso` installer file, flashing it to a USB drive using the `dd` command, and repeating the process for each new distro. This approach requires wiping and flashing the USB for every new installation, which can be time-consuming and inconvenient.

## Introducing Ventoy

Ventoy offers a better solution by allowing you to configure a USB stick to hold multiple Linux install images while still functioning as a typical USB storage device for files. Here's how Ventoy simplifies the process:

- **Multiple ISOs on One USB:** Simply copy multiple Linux install `.iso` images to the USB stick like any other file.
- **Boot Menu:** Upon rebooting, Ventoy automatically generates a menu listing all available images to boot.
- **Direct Browsing:** Press F2 to directly browse and boot files on the drive.

## Steps to Create a Ventoy USB Stick

### 1. Install Ventoy on the USB Stick

> **Note:** All contents currently on the USB drive will be completely wiped during the installation.

#### 1.1. Prepare the USB Stick

1. **Plug in the USB stick.**

#### 1.2. Download Ventoy

1. **Download the latest release:**
   - `ventoy-VERSION-linux.tar.gz`
   - `sha256.txt`

#### 1.3. Verify the Download

```bash
$ sha256sum -c --ignore-missing sha256.txt
ventoy-1.0.99-linux.tar.gz: OK
```

#### 1.4. Unpack the Archive

```bash
$ tar xvf ventoy-VERSION-linux.tar.gz
```

#### 1.5. Install Ventoy to the USB Stick

1. **Navigate to the Ventoy directory:**

    ```bash
    $ cd ventoy-VERSION/
    ```

2. **Run the installation script (replace `/dev/sdX` with your USB device):**

    ```bash
    $ sudo sh Ventoy2Disk.sh -i /dev/sdX
    ```

3. **After installation, the USB stick will have two partitions:**
   - **Partition #1:** Formatted with the exFAT filesystem. Use this partition to copy `.iso` files. Ventoy will search all directories and subdirectories recursively to find and list all `.iso` files alphabetically in the boot menu. This partition can also function as a typical storage device when not used for booting.
   - **Partition #2:** Reserved for Ventoy tools.

#### 1.6. (Optional) Reserve Disk Space

If your USB stick has ample space and you prefer not to use it all in a single partition, you can reserve disk space for later use.

**Example:** For a 128GB USB drive with 32GB reserved:

```bash
$ sudo sh Ventoy2Disk.sh -i -r 32000 /dev/sdX
```

> **Note:** Keeping the first partition size below 137GB helps avoid potential legacy BIOS issues on some machines.

### 2. Customize Ventoy with Plugins

Ventoy allows customization through plugins, which are snippets of code written in a `ventoy.json` file.

#### 2.1. Create the Plugins Directory

1. **On the first partition of the Ventoy USB stick, create a directory named `ventoy`:**

    ```bash
    mkdir /path/to/usb/ventoy
    ```

2. **Create a `ventoy.json` file inside the `ventoy` directory:**

    ```json
    {
        "control": [
            { "VTOY_DEFAULT_MENU_MODE": "0" },
            { "VTOY_DEFAULT_SEARCH_ROOT": "/iso" }
        ],
        
        "theme": {
            "display_mode": "CLI"
        },
        
        "menu_alias": [
            {
                "image": "/iso/debian-12.6.0-amd64-netinst.iso",
                "alias": "Debian 12.6.0 amd64-netinst"
            },
            {
                "image": "/iso/debian-live-12.6.0-amd64-standard.iso",
                "alias": "Debian Live 12.6.0 amd64-netinst"
            },
            {
                "image": "/iso/lmde-6-cinnamon-64bit.iso",
                "alias": "Linux Mint Debian Edition 6"
            },
            {
                "image": "/iso/mt86plus_7.00_64.iso",
                "alias": "Memtest86+ v7 (64bit)"
            }
        ]
    }
    ```

### 3. Update Ventoy

Updating Ventoy to a new version is straightforward and non-destructive, meaning all files in the first partition remain unchanged.

#### 3.1. Download the Latest Ventoy Version

1. **Download the latest `ventoy-VERSION-linux.tar.gz` and `sha256.txt` as before.**

#### 3.2. Install the Update

1. **Ensure the Ventoy USB stick is unmounted.**
2. **Run the update command (replace `/dev/sdX` with your USB device):**

    ```bash
    $ sudo sh Ventoy2Disk.sh -u /dev/sdX
    ```

By following these steps, you can efficiently manage multiple Linux distributions on a single USB stick using Ventoy, streamlining the process of testing and installing various Linux environments.