---
title: 00-cody
created: 2024-12-03
type: general
tags:
  - general
  - uiowa
  - devops
  - commands
  - development
---
**Modified:**

# Software & Systems discussed

- Automation for bare metal
	- https://tinkerbell.org/
	- https://theforeman.org/
	- https://technotim.live/posts/metal-as-a-service-packer/
- DevPod - GH Codespace Alternative
	- https://devpod.sh/
- FlatCar Linux - for idas?
	- https://www.flatcar.org/
	- https://github.com/google/omaha/blob/main/doc/ServerProtocol.md
	- https://github.com/flatcar/nebraska
- Gentoo Prefix Project Overview: Installing Gentoo on Non-Standard Systems
	- https://wiki.gentoo.org/wiki/Project:Prefix
- **Scripting Proxmox API**
	- https://pve.proxmox.com/wiki/Proxmox_VE_API
- **Rasdaemon**
	- https://github.com/mchehab/rasdaemon



# Mentoring

[[work-prioritization-guide]]


# NOTES:

## IDAS Inventory

- https://uiowa.atlassian.net/browse/IDAS-1539
- arrival date as start of warranty warranty expiration
- 5 year warranty 7 year replacement
- compute 01 and compute 02 app 01/02
- use install date for warranty
- compute node is 7 years
- everything else is 5 year
- head and app nodes HAVE to be replaced on time 

- keep three head-nodes for ceph 
- re-racked them if we can - ask Ruxton


## build node prep
argon build server most important
but if idas comes first move forward
going in same rack as argon infra
2 power 2 fiber drops 
- copperline for ipmi port
- bonded pair
- tag for 1104
- tag for gsa in itf 1182?
- cables hung
- email hostmaster for for ip and ipmi address for both host
- itf-argon-build01
- itf-idas-build01

- bug ruxton about fiber lines for the the three argon infrastruction nodes in that rack

## 2024-12-04
- LC port migration
- walk through nodes for vlans and ports
- port capture - done by 13th
	- nodes and vlans
	- capture ports to if it makes sense
- 7th 
	- move last of argon
	- idas
	- LSS
	- if time dedicated
- Deligate nodes
	- logged in in raitan
	- watch it go down
	- come back up
	- confirm can ping 
- Deligate so everyone is comfortable with port moves
- smartsheet clean up - DONE
	- some decommission list
		- tell ruxton to pull connections now
		- mark as completed
		- find dedicated nodes - genavieve spreadsheet
		- ``