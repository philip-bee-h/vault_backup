---
title: "lss-provisioning"
created: "2025-05-02"
type: runbook
system: 
status: 
tags:
  - operations
  - runbook
related_docs:
---
#### Bottlenecks
	b
create groups
- add your self to RW
- req add to am
	- they respond when finished
where share lives
- where back if needed
edit the yaml
- copy and past block update details
	- group ownership
	- contact
	- path
	- most just need nfs
- samba if needed - should be in workflow
- commit & push
- puppet deploy from local
- puppet agent on each server
	- primary then back up after completion
- acls added on primary after puppet agentrun complete

- ssh in to argon cd to dir as root 
	- created an able to auto mount
- smb - looking to see if advertise - dont
