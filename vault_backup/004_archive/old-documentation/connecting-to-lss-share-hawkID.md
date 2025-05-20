---
title: resolving-access-issues-lss-shares
type: process
processName: resolving-access-issues-lss-shares
tags:
  - uiowa/process
---

### Issue
When users experience issues logging in to and accessing the SMB LSS share, it is usually caused by macOS trying to use outdated credentials stored in the system cache. This issue occurs despite deleting the share credentials cached in the keychain, as they might be stored elsewhere, possibly involving Kerberos. This problem often arises after a recent password change.

### Solution
To ensure macOS uses the updated credentials for the SMB share, follow these steps:

1. **Clear Cached Credentials**:
	- **Open Keychain Access**:
		- Press `Cmd + Space` to open Spotlight search.
		- Type `Keychain Access` and press `Enter`.
		- In the Keychain Access window, select the `login` keychain from the sidebar.
	- **Locate and Delete Server Credentials**:
		- In the search bar at the top right, type the server name (e.g., `smb` or the specific server address).
		- Right-click on the relevant entry and select `Delete`.
	- **Reboot the Mac**:
		- Click the Apple menu in the top left corner.
		- Select `Restart` and confirm.

2. **Refresh Credential Cache**:
	- Rebooting the Mac will refresh the credential cache, ensuring that macOS will not try to connect to the SMB share using old cached credentials.

3. **Remap the Server with Updated Credentials**:
	- **Open Finder**:
		- Click on the Finder icon in the Dock.
		- Navigate to the top menu and select `Go` > `Connect to Server`.
- **Enter the Server Address**:
	- In the `Server Address` field, type the following URL:
```
smb://someuser:*@<serverName>.hpc.uiowa.edu
```
- Replace `<serverName>` with the actual server name.
- **Authenticate**:
	- When prompted, select `Connect As: Registered User`.
	- Enter the following details:
		- **Username**: `hawkID@iowa.uiowa.edu`
		- **Password**: Enter your HawkID password.
- **Connect**:
	- Click `Connect` to establish a connection to the server.

### Troubleshooting
Please send any error screenshots you capture during this process to the support team for further troubleshooting.




[mac auto caching credentials for smb - Google Search](https://www.google.com/search?q=mac+auto+caching+credentials+for+smb&oq=mac+auto+caching+credentials+for+smb&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIHCAEQIRifBTIHCAIQIRifBTIHCAMQIRifBTIHCAQQIRifBTIHCAUQIRifBTIHCAYQIRifBTIHCAcQIRifBTIHCAgQIRifBdIBCTE4MDkwajBqNKgCALACAQ&sourceid=chrome&ie=UTF-8)

[KB4120: SMB credentials cached by macOS (veeam.com)](https://www.veeam.com/kb4120)

[network - Where are my server credentials stored? - Ask Different (stackexchange.com)](https://apple.stackexchange.com/questions/407690/where-are-my-server-credentials-stored)

[Deleting/changing SMB credentials in Sonoma(!?) | Ars OpenForum (arstechnica.com)](https://arstechnica.com/civis/threads/deleting-changing-smb-credentials-in-sonoma.1498679/)

[Somehow SMB network passwords are getting cached in MacOS - until a full reboot of OS?? : r/sysadmin (reddit.com)](https://www.reddit.com/r/sysadmin/comments/17fymes/somehow_smb_network_passwords_are_getting_cached/)

[Disable local SMB directory enumeration caching - Apple Support](https://support.apple.com/en-us/101918)



## Email Template

Sure, here's a more user-friendly email template for the instructions on how to access the SMB LSS share on macOS after a password change:

---

**Subject:** Steps to Resolve SMB LSS Share Access Issue on macOS

**Dear (User's Name),**

If you're encountering difficulties accessing the SMB LSS share after changing your password, it's likely due to macOS using outdated credentials. Please follow the steps below to resolve this issue:

### 1. **Clear Cached Credentials**
#### Open Keychain Access:
- Press `Cmd + Space` to open Spotlight search.
- Type `Keychain Access` and press `Enter`.
- In Keychain Access, select the `login` keychain from the sidebar.

#### Locate and Delete Server Credentials:
- Use the search bar at the top right to type the server’s name (e.g., `smb` or the specific server address).
- Right-click on the relevant entry and select `Delete`.

### 2. **Reboot the Mac**
- Click the Apple menu in the top left corner.
- Select `Restart` and confirm to reboot. This step helps refresh the credential cache.

### 3. **Remap the Server with Updated Credentials**
#### Open Finder:
- Click on the Finder icon in the Dock.
- Navigate to the top menu and select `Go > Connect to Server`.

#### Enter the Server Address:
- In the Server Address field, type the following URL:
  ```
  smb://someuser:*@<serverName>.hpc.uiowa.edu
  ```
  *Replace `<serverName>` with the actual server name.*

#### Authenticate:
- When prompted, select `Connect As: Registered User`.
- Enter your details:
	- **Username:** hawkID@iowa.uiowa.edu
	- **Password:** *Your HawkID password*

#### Connect:
- Click `Connect` to establish a connection to the server.

### Troubleshooting
If you encounter any issues during this process, please send screenshots of any errors to the support team for further assistance.

We hope this helps you swiftly regain access to your files. Should you need further help, feel free to reach out.

**Best Regards,**




