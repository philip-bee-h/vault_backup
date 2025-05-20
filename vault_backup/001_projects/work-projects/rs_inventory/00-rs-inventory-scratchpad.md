

Based on your inventory, I’ve organized the data into a format that aligns with the requirements for importing it into **NetBox**. Below, I’ve created two key sections to help you input your data: **Manufacturers** and **Device Types**.

---

### **1. Manufacturers**

You will need to add the following manufacturers into NetBox under `Devices → Manufacturers`:

- Supermicro
- Gigabyte
- Asus

---

### **2. Device Types**

Each device type should be mapped to its corresponding manufacturer in NetBox. Here’s the list based on your inventory:

#### **Supermicro**

- AS-1114S-WTRT
- SYS-1029U-TRTP2

#### **Gigabyte**

- R152-Z31-00
- G221-Z30-00
- R272-Z30-00
- G292-Z44-00

#### **Asus**

- ESC4000 G4
- K14PP-D24 Series
- RS720A-E12-RS12
- ESC8000A-E12
- RS500A-E11-RS12U



---

Working on Device import can you create the csv for import

|   |   |   |   |
|---|---|---|---|
|role||name|Assigned role|
|manufacturer||name|Device type manufacturer|
|device_type||model|Device type model|
|status||—|Operational status [](http://localhost:8001/dcim/devices/import/#)|
|site||name|Assigned site|
|name|—|—|Name|
|tenant|—|name|Assigned tenant|

- all devices have idas tenant
- all device names are the information before ".idas"
- all device are at the LC site
- all device have active status

I only need the above info no ips yes
