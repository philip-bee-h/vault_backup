---
title: macos-smb-access-email
type: emailDraft
tags:
  - emailDraft
date: 2024-07-31
sent: false
---
Date: 2023-10-29

## General email template 

Subject: Assistance Required for Drive Access Issues

Hello (User),

Thank you for reaching out to us with the issue you're experiencing in accessing the drive. I apologize for the inconvenience and am here to help you resolve this matter swiftly.

To assist you more effectively, could you please provide us with the following information?

1. **Hostname of the Computer:** What is the name of the computer from which you are trying to access the drive?
2. **Recent Password Change:** I noticed that your password was changed 2 days ago. Could you confirm if this is around the time the access issues began?
3. **Location of Access Attempt:** Are you attempting to connect from home, on campus, or another location?
4. **Type of Network Connection:** Are you using a WiFi or a wired connection? If it's WiFi, could you please specify the network name?
5. **Usage of VPN:** Do you have a VPN enabled while trying to access the drive?
6. **Screenshots:** If possible, please include screenshots of any error messages you encounter. This will greatly help us in troubleshooting the issue.

We have noticed that similar issues often occur with Mac users, especially after recent password changes. Here are the steps we typically follow to resolve such issues:

### Resolution Steps for Mac Users

**Clear Cached Credentials:**
- **Open Keychain Access:**
  - Press `Cmd + Space` to open Spotlight search.
  - Type `Keychain Access` and press `Enter`.
  - In the Keychain Access window, select the `login` keychain from the sidebar.
- **Locate and Delete Server Credentials:**
  - In the search bar at the top right, type the server’s name (e.g., `smb` or `store`).
  - Right-click on the relevant entry and select `Delete`.
- **Reboot the Mac:**
  - Click the Apple menu in the top left corner.
  - Select `Restart` and confirm.

**Remap the Server with Updated Credentials:**
- **Open Finder:**
  - Click on the Finder icon in the Dock.
  - Navigate to the top menu and select `Go` > `Connect to Server`.
- **Enter the Server Address:**
  - Type `smb://someuser:*@<serverName>.hpc.uiowa.edu` (replace `<serverName>` with the actual server name).
- **Authenticate:**
  - When prompted, select `Connect As: Registered User`.
  - Enter `hawkID@iowa.uiowa.edu` for the username and your HawkID password for the password.
- **Connect:**
  - Click `Connect` to establish a connection to the server.

Please feel free to attempt the above steps and let us know if the issue persists. Also, any error screenshots you provide will help us further diagnose and troubleshoot the problem.

Thank you for your cooperation and patience. 

Best ,

---

## RDSS info

To ensure macOS uses the updated credentials for the SMB share, follow these steps:
1. **Clear Cached Credentials**:
    - **Open Keychain Access**:
        - Press `Cmd + Space` to open Spotlight search.
        - Type `Keychain Access` and press `Enter`.
        - In the Keychain Access window, select the `login` keychain from the sidebar.
    - **Locate and Delete Server Credentials**:
        - In the search bar at the top right, type the server’s name (e.g., `smb` or the specific server address `store`).
        - Right-click on the relevant entry and select `Delete`.

2. **Refresh Credential Cache**
    - *Rebooting the Mac will refresh the credential cache, ensuring that macOS will not try to connect to the SMB share using old cached credentials.*
	- **Reboot the Mac**:
        - Click the Apple menu in the top left corner.
        - Select `Restart` and confirm.

Try that and then map your RDSS Share.  Chances are there are old credentials stuck