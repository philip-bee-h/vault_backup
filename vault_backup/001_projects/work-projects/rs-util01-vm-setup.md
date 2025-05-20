---
title: rs-util01-vm-setup
created: 2024-12-02
type: project
status: 
tags:
  - project
  - uiowa
  - vm
related_docs:
---
# Task List for VM Server Setup

LINK: https://uiowa.atlassian.net/wiki/x/IIDUFQ

## Notes
- I would do Ubuntu 24.04 and you can use the puppet_rs_vms puppet environment



## ToDo

- [ ] **Request VM Server** - ==Requested waiting on confirmation ==
  - **Action:** Navigate to the VM server request page and complete the form as specified in the how-to article.


- [x] **Acquire an IP Address** -
  - **Action:** Send an email to hostmaster to request an IP address.- 
  - **Email Template:**
    ```
    Hostmaster,
    Please issue an IP in the 172.30.102.0/24 range with a name of <hostName>.hpc.uiowa.edu.
    ```
  - **Response:**
```sh
IP: rs-util01.hpc.uiowa.edu, 172.30.102.100

IPv4 netmask: 255.255.255.0/24

IPv4 gateway: 172.30.102.1

IPv4 DNS: 128.255.1.3, 128.255.64.11, 128.255.1.8
```

- [x] **Request Firewall Rules for VM Server** - ==Request sent - waiting fopr rewsponse==
  - **Steps:**
    - Upon receiving the IP address, request firewall rules as outlined in the linked guide.
    - Monitor your inbox for a "Routing complete" email, confirming the creation of the firewall rules.
```markdown
### Package Summary

|   |   |
|---|---|
|Request|**Protocol - TCP Source - uiowa-vpn Destination - 172.30.102.100 DestPort - 22 Protocol - TCP Source - uiowa-nets Destination - 172.30.102.100 DestPort - 22**|
|Justification|**Allow ssh for management and access to scripting/dev host**|
|Form|**Firewall Rule Request Form**|
|Initiator|**Haworth, Philip**|
|Workflow ID|**15021184**|
```

- [ ] **Set Up Your Ubuntu VM Development Server**
  - **Action:** Follow the detailed steps in the setup guide to image your VM.
  - **Guide:** [Add link here]

- [ ] **Create a Computer Object in AD**
  - **Location:** Navigate to AD and create a computer object under `ITS > Research > RS-VMs`.

- [ ] **Deploy Ubuntu Development VM Server**
  - **Action:** Execute the deployment following the steps outlined in the linked article.
  - **Article:** [Add link here]
