---
title: idas-new-node-port-confirmation
created: 2024-11-01
modified: YYYY-MM-DD
type: general
tags:
  - uiowa
  - idas
  - issue
  - networking
---

# Notes

## Follow up on networking issues

**Email to NES**
Subject: Request for Assistance with Node Deployment Networking Issues

Hello,

We are encountering networking issues while deploying our two new nodes in IDAS. Could you please assist us by checking the following for both nodes:

**Nodes:** 
- rs-k8-compute13
- rs-k8-compute14

**Port name** - dc-lc-leaf-3@2

- Verify the configuration of the port.
- Confirm that the port is active.
- Check whether the port is configured for 10G or 25G (it should be set to 25G).

Thank you for your help!

Best 

----

well the links are up but I still can't ping.. 

Need to verify which vlans are exposed now I guess... 

I’m gonna tell Patrice that we can see the links are up, but we’re still unable to ping and ask to verify that 1104,1108,1131 are tagged on these interfaces

can you verify they are in etherchannel too? 

I did try both but to eliminate the guessing  Just ask to verify that 1104,1108,1131 are tagged on these interfaces The links are up but we can't ping IPs on those vlans they are trunks so its close  back in a bit running off to find lunch