---
title: ipmi-config-one-liner
created: 2025-01-22
type: command-reference
category: 
tags:
  - resource
  - bash
  - script
  - ipmi
  - command-ref
related_docs:
---
```shell
CHANNEL=1; USER_ID=3; USERNAME="idas"; PASSWORD="moped-bY2PW"; PRIVILEGE=4; \
sudo ipmitool user set name $USER_ID $USERNAME && \
sudo ipmitool user set password $USER_ID $PASSWORD && \
sudo ipmitool channel setaccess $CHANNEL $USER_ID callin=on link=off ipmi=on privilege=$PRIVILEGE && \
sudo ipmitool user enable $USER_ID && \
sudo ipmitool user list $CHANNEL && \
echo "IPMI configuration completed successfully!"
```


### Reference Note: IPMI User Configuration Script

#### **What This Script Does**

This script configures a new user in an IPMI (Intelligent Platform Management Interface) system. It performs the following steps:

1. **Set User Details**:
    - Assigns a username (`USERNAME`) and password (`PASSWORD`) to a specific user ID (`USER_ID`).
2. **Configure User Access**:
    - Updates the access settings for the specified channel (`CHANNEL`).
    - Enables the user and assigns a privilege level (`PRIVILEGE`).
3. **Verify Configuration**:
    - Lists all users on the specified channel to confirm the configuration.
4. **Completion Message**:
    - Displays a success message when the configuration is complete.

#### **Command Breakdown**

- `sudo ipmitool user set name $USER_ID $USERNAME`: Sets the username for the specified user ID.
- `sudo ipmitool user set password $USER_ID $PASSWORD`: Sets the password for the specified user ID.
- `sudo ipmitool channel setaccess $CHANNEL $USER_ID callin=on link=off ipmi=on privilege=$PRIVILEGE`: Configures access permissions for the user on the given channel.
- `sudo ipmitool user enable $USER_ID`: Enables the specified user ID.
- `sudo ipmitool user list $CHANNEL`: Lists all users on the channel to confirm the settings.
- `echo "IPMI configuration completed successfully!"`: Outputs a success message.

#### **Usage**

- Update the `CHANNEL`, `USER_ID`, `USERNAME`, `PASSWORD`, and `PRIVILEGE` variables as needed before executing the script.
- Ensure the script is run with `sudo` privileges to access the IPMI interface.
