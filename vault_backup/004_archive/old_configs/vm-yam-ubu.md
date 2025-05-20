---
title: vm-yam-ubu
created: 2024-12-06
type: project
status: 
tags:
  - project
  - homelab
  - arr
  - docker
  - docker-compose
  - proxmox
related_docs:
---
**Modified:**

## Server Info

Name:
OS
USER
PW: 

IP: 192.169.10.22

![[vm-yam-ubu-1.png]]

found date out of sync and had to set it

```sh
pbh@vm-samba-ubu:~$ date
Sat Dec  7 06:05:24 AM UTC 2024
pbh@vm-samba-ubu:~$ sudo timedatectl set-timezone America/Chicago
pbh@vm-samba-ubu:~$ date
Sat Dec  7 12:05:40 AM CST 2024
```


---

# Work Log

## **Date**: 2024-12-17 - samba set up

#### **Tasks Completed**:

1. **System Update**:
    
    - Ran `sudo apt upgrade` to update the system packages.
    - Rebooted the server to apply updates.
2. **Installed Required Packages**:
    
    - Installed **CIFS Utilities** for Samba share mounting:
        
        ```bash
        sudo apt install cifs-utils -y
        ```
        
    - Installed **Samba Client** for testing and troubleshooting:
        
        ```bash
        sudo apt install smbclient -y
        ```
        
3. **Tested Samba Share**:
    
    - Verified access to the `MediaShare` on the Samba server (`192.168.10.23`) using:
        
        ```bash
        smbclient //192.168.10.23/MediaShare -U guest
        ```
        
4. **Prepared Mount Point**:
    
    - Created the directory `/srv/media` for mounting:
        
        ```bash
        sudo mkdir -p /srv/media
        ```
        
5. **Mounted Samba Share**:
    
    - Mounted the Samba share temporarily:
        
        ```bash
        sudo mount -t cifs //192.168.10.23/MediaShare /srv/media -o guest,uid=1000,gid=1000
        ```
        
6. **Configured Persistent Mount**:
    
    - Edited `/etc/fstab` to ensure the Samba share mounts at boot:
        
        ```plaintext
        //192.168.10.23/MediaShare /srv/media cifs guest,uid=1000,gid=1000 0 0
        ```
        
    - Tested the configuration by running:
        
        ```bash
        sudo mount -a
        ```
        
7. **Verified Mount**:
    
    - Checked that the share was mounted successfully:
        
        ```bash
        df -h | grep /srv/media
        ```
        
    - Verified access to files within `/srv/media`.

#### **Next Steps**:

- Continue with YAMS installation and configuration.
- Test the media folder structure created by YAMS.


---

## Date: 2024-12-21 - YAMS install

I have done this....
```sh
21:58:32 pbh @ vm-yam-ubu-ssh_session in ~
❯ sudo mkdir -p /opt/yams
21:58:51 pbh @ vm-yam-ubu-ssh_session in ~
❯ sudo chown -R $USER:$USER /opt/yams
21:58:58 pbh @ vm-yam-ubu-ssh_session in ~
❯

```

- ran install followed all the defaults
- used account number for username/password setting up vpn

```sh
========================================================
     _____          ___           ___           ___
    /  /::\        /  /\         /__/\         /  /\
   /  /:/\:\      /  /::\        \  \:\       /  /:/_
  /  /:/  \:\    /  /:/\:\        \  \:\     /  /:/ /\
 /__/:/ \__\:|  /  /:/  \:\   _____\__\:\   /  /:/ /:/_
 \  \:\ /  /:/ /__/:/ \__\:\ /__/::::::::\ /__/:/ /:/ /\
  \  \:\  /:/  \  \:\ /  /:/ \  \:\~~\~~\/ \  \:\/:/ /:/
   \  \:\/:/    \  \:\  /:/   \  \:\  ~~~   \  \::/ /:/
    \  \::/      \  \:\/:/     \  \:\        \  \:\/:/
     \__\/        \  \::/       \  \:\        \  \::/
                   \__\/         \__\/         \__\/
========================================================
All done!✅ Enjoy YAMS!
You can check the installation on /opt/yams
========================================================
Everything should be running now! To check everything running, go to:

Service URLs:
qBittorrent: http://192.168.10.22:8080/
Radarr: http://192.168.10.22:7878/
Sonarr: http://192.168.10.22:8989/
Lidarr: http://192.168.10.22:8686/
Readarr: http://192.168.10.22:8787/
Prowlarr: http://192.168.10.22:9696/
Bazarr: http://192.168.10.22:6767/
plex: œ
Portainer: http://192.168.10.22:9000/
sabnzbd: http://192.168.10.22:8181/

You might need to wait for a couple of minutes while everything gets up and running

All the services location are also saved in ~/yams_services.txt
========================================================

To configure YAMS, check the documentation at
https://yams.media/config

========================================================
```

#### Setting up. SABnzbd

Here's how to add **SABnzbd** to your YAMS stack successfully:

### Steps to Add SABnzbd to YAMS

1. **Directory Setup**: Ensure your directories are configured correctly. The directories SABnzbd uses for incomplete and complete downloads must match your `docker-compose` configuration. For example:
    
    - **Incomplete Downloads**: `/srv/media/downloads/incomplete`
    - **Complete Downloads**: `/srv/media/downloads/complete`
2. **Update `docker-compose.custom.yaml`**: Add the SABnzbd container configuration using the YAMS variables for consistency. Here's the adjusted configuration:
    
    ```yaml
    version: "3"
    
    services:
      sabnzbd:
        image: lscr.io/linuxserver/sabnzbd
        container_name: sabnzbd
        environment:
          - PUID=${PUID}
          - PGID=${PGID}
        volumes:
          - ${MEDIA_DIRECTORY}/downloads:/downloads
          - ${INSTALL_DIRECTORY}/config/sabnzbd:/config
        ports:
          - 8181:8080
        restart: unless-stopped
    ```
    
3. **Restart YAMS**: After saving the file, restart the YAMS stack to apply the changes:
    
    ```bash
    yams restart
    ```
    
4. **Configure SABnzbd**: In the SABnzbd web interface:
    - Set the **Incomplete Folder** to `/downloads/incomplete`.
    - Set the **Complete Folder** to `/downloads/complete`.
5. **Adjust Sonarr and Radarr**: Since SABnzbd downloads files to `/downloads`, ensure Sonarr and Radarr are configured with the correct paths:
    - Add a **Remote Path Mapping** in Sonarr/Radarr:
        - **Host**: `sabnzbd` (or the IP/hostname of your container)
        - **Remote Path**: `/downloads/complete`
        - **Local Path**: `/srv/media/downloads/complete`
6. **Test the Setup**:
    
    - Queue a download in Sonarr or Radarr.
    - Verify that the file downloads via SABnzbd and is moved to the correct location for import.

### Common Issues

- **Wrong Directory Paths**: Ensure the directories in SABnzbd match the `docker-compose` volumes.
- **Permissions**: If files aren't accessible, verify that the `PUID` and `PGID` values match the user running YAMS.

This setup aligns SABnzbd with YAMS and ensures seamless integration with Sonarr and Radarr. Let me know if you encounter any issues!






https://support.newshosting.com/kb/article/277-sabnzbd-setup/





