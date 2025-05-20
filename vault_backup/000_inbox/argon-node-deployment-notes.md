## **Core Information**

- **Investor:**  
    Claudio Margulis
    
- **What is it:**  
    265GB GPU system with 1 x RTX 4500 Aad
	
- **HostName**
	argon-itf-ca44-05
	
- **Serial Number:**  
    RASOMD0000P5
    
- **Purchase Order (PO):**  
    1003133037
    
- **MAC Address:**  
    B4:96:91:34:0F:D0
	
- **1104:ip** - 
	172.29.5.240


## **Core Information**

- **Investor:**  
	Claudio Margulis
    
- **What is it:**  
    265GB GPU system with 1 x RTX 4500 Aad
    
- **HostName**
	argon-itf-ca44-06
	
- **Serial Number:**  
    R8SOMD00033L
    
- **Purchase Order (PO):**  
	1003133037
    
- **MAC Address:**  
	B4:96:91:31:CE:B8
	
- **1104:ip** - 
	172.29.5.241



```sh
     wwsh node new argon-itf-ca44-05\
       --netdev=eth0 \
       --mtu=9000 \
       --hwaddr=B4:96:91:34:0F:D0 \
       --ipaddr=172.29.5.240 \
       --network=172.29.4.0 \
       -G 172.29.6.109 \
       -M 255.255.252.0 \
       --groups=itf,infiniband
------------------------------
08:28:36 hawrth @ argon-itf-head-ssh_session in ~
❯ wwsh node print argon-itf-ca44-05
#### argon-itf-ca44-05.hpc ####################################################
argon-itf-ca44-05.hpc: ID               = 1296
argon-itf-ca44-05.hpc: NAME             = argon-itf-ca44-05,argon-itf-ca44-05.hpc
argon-itf-ca44-05.hpc: NODENAME         = argon-itf-ca44-05
argon-itf-ca44-05.hpc: ARCH             = x86_64
argon-itf-ca44-05.hpc: CLUSTER          = hpc
argon-itf-ca44-05.hpc: DOMAIN           = UNDEF
argon-itf-ca44-05.hpc: GROUPS           = itf,infiniband,newnode
argon-itf-ca44-05.hpc: ENABLED          = TRUE
argon-itf-ca44-05.hpc: eth0.HWADDR      = b4:96:91:34:0f:d0
argon-itf-ca44-05.hpc: eth0.HWPREFIX    = UNDEF
argon-itf-ca44-05.hpc: eth0.IPADDR      = 172.29.5.240
argon-itf-ca44-05.hpc: eth0.NETMASK     = 255.255.252.0
argon-itf-ca44-05.hpc: eth0.NETWORK     = 172.29.4.0
argon-itf-ca44-05.hpc: eth0.GATEWAY     = 172.29.6.109
argon-itf-ca44-05.hpc: eth0.MTU         = 9000
argon-itf-ca44-05.hpc: eth0.FQDN        = UNDEF

```

```sh
     wwsh node new argon-itf-ca44-06\
       --netdev=eth0 \
       --mtu=9000 \
       --hwaddr=B4:96:91:31:CE:B8 \
       --ipaddr=172.29.5.241 \
       --network=172.29.4.0 \
       -G 172.29.6.109 \
       -M 255.255.252.0 \
       --groups=itf,infiniband

```

```sh
node new argon-itf-bx48-xx --netdev=eth0 --mtu=9000 --hwaddr=ac:1f:6b:41:66:3c --ipaddr=172.29.4.xxx --network=172.29.4.0 -G 172.29.6.109 -M 255.255.252.0 --groups=itf,infiniband
```





set up wwsh new node 
- run as root 
- use sudo su

confirm node name and info on head
- print node command

reboot node
- look for it tio come up with hostname/nodename

run puppet on head

bios config