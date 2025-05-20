---
title: netbox-csv-imports
created: 2025-01-20
type: projectNote
status: 
tags:
  - project
  - uiowa
  - inventory
  - config
  - netbox
related_docs:
---
### **1. CSV Format for Device Types**

You can use this format to bulk import device types into NetBox:

```csv
manufacturer,model,slug,u_height
Supermicro,AS-1114S-WTRT,as-1114s-wtrt,1
Supermicro,SYS-1029U-TRTP2,sys-1029u-trtp2,1
Gigabyte,R152-Z31-00,r152-z31-00,1
Gigabyte,G221-Z30-00,g221-z30-00,2
Gigabyte,R272-Z30-00,r272-z30-00,2
Gigabyte,G292-Z44-00,g292-z44-00,4
Asus,ESC4000 G4,esc4000-g4,2
Asus,K14PP-D24 Series,k14pp-d24-series,1
Asus,RS720A-E12-RS12,rs720a-e12-rs12,2
Asus,ESC8000A-E12,esc8000a-e12,4
Asus,RS500A-E11-RS12U,rs500a-e11-rs12u,1
```

**Field Explanations**:

- `manufacturer`: Name of the manufacturer (e.g., Supermicro, Gigabyte, Asus).
- `model`: The exact model of the device.
- `slug`: A URL-safe, lowercase, hyphen-separated version of the model name.
- `u_height`: Height of the device in rack units (U). Default to 1U if uncertain or adjust based on actual specs.

### **CSV Format for Bulk Import**

Here’s the updated CSV for importing these roles in bulk:

```csv
name,slug,color,vm_role
Head Node,head-node,3498db,False
Application/Utility Node,app-node,2ecc71,False
Compute Node,compute-node,e74c3c,False
Ceph Node,ceph-node,9b59b6,False
GPU Node,gpu-node,#f1c40f,False
```



```csv
manufacturer,model,slug,u_height
Supermicro,unknown,unknown,2
Supermicro,unknown,unknown,4
Gigabyte,unknown,unknown,2
Gigabyte,unknown,unknown,4
asus,unknown,unknown,2
asus,unknown,unknown,4
```






### Rack csv

```csv
site,name,status,width,u_height
LC,F-7,active,19,42
LC,J-11,active,19,42
LC,N-29,active,19,42
LC,O-29,active,19,42
LC,P-29,active,19,42
LC,M-32,active,19,42
LC,N-32,active,19,42
LC,0-32,active,19,42
```




### ip ranges

```csv
start_address,end_address,status,description,tags
172.29.4.0/24,172.29.4.255/24,active,1104 vlan range,vlan-1104
172.29.5.0/24,172.29.5.255/24,active,1104 vlan range,vlan-1104
172.29.6.0/24,172.29.6.255/24,active,1104 vlan range,vlan-1104
172.29.7.0/24,172.29.7.255/24,active,1104 vlan range,vlan-1104
```


### Add these
compute13: 172.29.4.103
compute14: 172.29.4.104
