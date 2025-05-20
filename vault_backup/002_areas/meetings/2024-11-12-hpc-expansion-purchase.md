---
title: 2024-11-12-hpc-expansion-purchase
created: 2024-11-12
type: meeting
attendees: 
tags:
  - meeting
  - uiowa
related_projects:
---
## Date: 2024-11-12
# Agenda


# Action Items
- [ ] gather info on switches
	- [ ] name
	- [ ] model/make
	- [ ] serial
	- [ ] cabinets 

# Follow-up

## **1. SNMP for Netgear Switches**

### Steps:

1. **Enable SNMP on the Switch**:
    
    - Log into the switch's web interface.
    - Navigate to **System > SNMP** (or similar).
    - Enable SNMP and set a community string (e.g., `public`).
2. **Query the Switch**:
    
    - Install `snmpwalk` on your system (e.g., via `apt`, `yum`, or `brew`):
        
        bash
        
        Copy code
        
        `sudo apt install snmp snmp-mibs-downloader`
        
    - Run this command:
        
        bash
        
        Copy code
        
        `snmpwalk -v2c -c <community-string> <switch-IP> sysDescr`
        
        This will return basic details about the switch.
3. **Check for Serial Number OIDs**:
    
    - Netgear OIDs include:
        
        - System description: `1.3.6.1.2.1.1.1.0`
        - Serial number: `1.3.6.1.4.1.4526.22.4.3.1.12`
        - Hardware model: `1.3.6.1.4.1.4526.11.1.1.0`
    - Query specific OIDs:
        
        bash
        
        Copy code
        
        `snmpget -v2c -c <community-string> <switch-IP> <OID>`
        
4. **Automate Collection**:
    
    - Use a script (e.g., Python with `pysnmp`) to loop through a list of switch IPs and pull the data automatically.

---

## **2. SSH/CLI Commands**

### Steps:

1. **Access the Switch via SSH**:
    
    - Use a terminal client to log in:
        
        bash
        
        Copy code
        
        `ssh admin@<switch-IP>`
        
2. **Run Commands to Gather Info**:
    
    - On Netgear switches, try:
        
        bash
        
        Copy code
        
        `show system-information show version`
        
    - These commands will display the model, serial number, and firmware version.
3. **Automate with Python**:
    
    - Use a library like `paramiko` to script SSH login and command execution.