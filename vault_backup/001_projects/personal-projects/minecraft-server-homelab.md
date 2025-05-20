---
title: minecraft-server-homelab
created: 2024-11-23
type: process
system: 
tags:
  - process
  - procedure
  - uiowa
related_docs:
---
For a **first Minecraft server** with **2-3 players**, your **OptiPlex 9010** is more than sufficient. Here’s a simple setup tailored to your needs:

---

# **Key Considerations for Your Setup**

- **Vanilla Server:** Ideal for a simple, no-mods experience.
- **Light Resources:** Allocate just enough RAM and CPU to keep the server running smoothly.
- **Local Access Only:** No need for internet-facing configurations.

---

# **Configuration for Your Server**

### **1. Allocate Minimal Resources**

For 2-3 players, you only need:

- **RAM:** 1GB–2GB is enough.
- **CPU:** 1–2 cores.

Startup command for a lightweight setup:

```bash
java -Xmx2G -Xms1G -jar server.jar nogui
```

- `-Xmx2G`: Maximum memory (2GB).
- `-Xms1G`: Minimum memory (1GB).

---

### **2. Keep the Setup Simple**

- Use **Vanilla** or **PaperMC**:
    - **Vanilla** for a default experience.
    - **PaperMC** ([Download here](https://papermc.io/)) if you want better performance or plugins later.

---

# **Setup Steps**

1. **Install Java**  
    Minecraft requires Java 17 or later:
    
    ```bash
    sudo apt update && sudo apt install openjdk-17-jre -y
    ```
    
2. **Download the Server File**
    
    - Vanilla: [Minecraft Server Download](https://www.minecraft.net/en-us/download/server).
    - PaperMC (for performance): [PaperMC Downloads](https://papermc.io/downloads).
3. **Create a Server Folder**
    
    - Store all server files in one place:
        
        ```bash
        mkdir ~/minecraft-server && cd ~/minecraft-server
        ```
        
4. **Start the Server**
    
    ```bash
    java -Xmx2G -Xms1G -jar server.jar nogui
    ```
    
    - The server will generate necessary files and stop.
    - Open `eula.txt` and accept the EULA:
        
        ```bash
        nano eula.txt
        ```
        
        Change:
        
        ```
        eula=false
        ```
        
        To:
        
        ```
        eula=true
        ```
        
5. **Configure `server.properties`**
    
    - Open the file:
        
        ```bash
        nano server.properties
        ```
        
    - Set these key values:
        
        ```
        max-players=3
        server-ip=192.168.x.x  # Replace with your server's local IP
        online-mode=true       # Enable local player authentication
        ```
        
    - Save and exit (`Ctrl+O`, `Enter`, `Ctrl+X`).
6. **Allow Local Connections Only**
    
    - Set up your firewall:
        
        ```bash
        sudo ufw allow from 192.168.0.0/24 to any port 25565
        sudo ufw enable
        ```
        
7. **Reconnect to the Server**
    
    - On each player’s device, go to **Multiplayer** > **Direct Connect** and enter the server's local IP.

---

# **Testing**

- Have the kids connect to the server and play!
- Monitor performance (CPU, RAM) with:
    
    ```bash
    top
    ```
    
    or
    
    ```bash
    htop
    ```
    

---

This setup is lightweight and local, perfect for 2-3 players. Let me know if you’d like help adding features like mods or backups in the future!