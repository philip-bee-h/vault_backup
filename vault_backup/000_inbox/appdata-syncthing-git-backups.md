---
title: appdata-syncthing-git-backups
created: 2024-11-15
type: process
system: 
tags:
  - process
  - procedure
  - homelab
  - self-hosting
related_docs:
---
Here’s a simplified note to document the plan for your notes:

  

Syncthing and Git Backup Plan for Docker Appdata

  

Overview

  

Set up Syncthing to sync appdata folders from three Docker servers to a centralized fourth server. Use Git on the fourth server to version control and manage backups of the configurations.

  

Folder Structure

  

Standardize the appdata folder structure across all servers:

  

/srv/appdata/

├── server1/

├── server2/

└── server3/

  

Setup Steps

  

1. Install Syncthing

  

• Install Syncthing on:

• Docker Servers: To sync appdata folders.

• Central Server: To receive and manage all backups.

  

2. Configure Syncthing

  

• Docker Servers:

• Sync the appdata folder to the fourth server.

• Use “Send Only” mode to prevent overwrites.

• Central Server:

• Receive each server’s appdata folder:

• Server 1 -> /srv/appdata/server1/

• Server 2 -> /srv/appdata/server2/

• Server 3 -> /srv/appdata/server3/

• Enable “Receive Only” mode to avoid propagating changes back.

  

3. Enable Versioning

  

• On the central server, enable Simple File Versioning for safety:

• Retain 5-10 versions of changed/deleted files.

  

4. Set Up Git for Version Control

  

• Initialize a Git repository on the central server:

  

cd /srv/appdata

git init

git add .

git commit -m "Initial commit"

  

  

• Automate commits with a cron job or systemd timer:

  

git add .

git commit -m "Automated commit on $(date)"

  

5. Update Process

  

• Make changes directly on the server hosting the service.

• Allow Syncthing to sync changes to the central server.

• Use Git to track changes, manage updates, or roll back configurations as needed.

  

Advantages

  

• Centralized Backup: All appdata is stored in one location.

• Version Control: Git tracks all changes with rollback capability.

• Failover: Easy recovery in case of server failures.

  

Considerations

  

• Network Traffic: Exclude logs or temporary files to reduce bandwidth usage.

• Conflict Management: Avoid simultaneous changes across multiple servers.

• Scaling: Ensure the central server has enough resources for syncing and Git operations.

• Security: Use encrypted connections (TLS) and secure the Syncthing web interface with strong passwords.

  

This plan ensures redundancy, version control, and easy management of your Docker configurations.

  

Let me know if you’d like any additional details added to this note!