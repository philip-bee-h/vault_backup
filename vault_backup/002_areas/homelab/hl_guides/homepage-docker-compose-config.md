---
title: homepage-docker-compose-config
created: 2024-11-14
type: process
system:
  - docker
  - homelab
tags:
  - process
  - procedure
  - docker
  - docker-compose
  - homelab
related_docs: "[[01-pi-mon-yavin]]"
---
**LINK:** https://gethomepage.dev/installation/
## **Homepage Setup with Docker Compose**

### **Directory Structure**
Set up the necessary directories in your Raspberry Pi's filesystem:

```bash
mkdir -p ~/appdata/homepage/config
```

This structure will store:
- `docker-compose.yml`: The Docker Compose configuration file.
- `config`: Persistent storage for Homepage's configuration files.

### **Docker Compose File**
Create a `docker-compose.yml` file in `~/appdata/homepage`:

```yaml
services:
  homepage:
    image: ghcr.io/gethomepage/homepage:latest
    container_name: homepage
    ports:
      - "3000:3000"  # Host:Container port mapping
    volumes:
      - ./config:/app/config  # Persistent configuration
      # - /var/run/docker.sock:/var/run/docker.sock  # Optional for Docker integration
    environment:
      PUID: $PUID  # User ID for non-root execution
      PGID: $PGID  # Group ID for non-root execution
    restart: unless-stopped
```

> **Note:** The `version` field is no longer required in Docker Compose v2+. Remove it to avoid warnings.

---

### **Environment Variables**
Create a `.env` file in the same directory to specify the `PUID` and `PGID`:

```bash
touch ~/appdata/homepage/.env
```

Add the following content (replace `1000` with your user and group IDs):

```dotenv
PUID=1000
PGID=1000
```

You can find your user and group IDs with:

```bash
id
```

---

### **Setting Permissions**
Ensure the `config` directory has the correct ownership and permissions for the specified `PUID` and `PGID`:

```bash
sudo chown -R 1000:1000 ~/appdata/homepage/config
sudo chmod -R 755 ~/appdata/homepage/config
```

---

### **Starting the Container**
Navigate to the project directory and start the service:

```bash
cd ~/appdata/homepage
docker compose up -d
```

This will:
- Start the Homepage container.
- Automatically restart it if the system reboots or the container stops.

---

### **Stopping and Removing the Container**
To stop and remove the container, use:

```bash
docker compose down
```

#### Optional Flags:
- Remove volumes (all data will be lost):
  ```bash
  docker compose down --volumes
  ```
- Remove associated images:
  ```bash
  docker compose down --rmi all
  ```

---

### **Common Warnings**
- **`version` Field Obsolete**:
  The `version` field in `docker-compose.yml` is no longer needed for Docker Compose v2+ and will be ignored. Simply omit it from your configuration.

- **Optional Docker Integration**:
  The `docker.sock` volume mapping is optional. It allows Homepage to integrate with Docker but introduces potential security risks. Use it only if you need Docker-related features.

---

### **Verifying the Setup**
After starting the service:
1. Access Homepage at `http://<your-server-ip>:3000`.
2. Confirm the container is running:
   ```bash
   docker compose ps
   ```

---

### **Example Workflow Commands**

1. **Set Up and Start**:
   ```bash
   mkdir -p ~/appdata/homepage/config
   touch ~/appdata/homepage/.env
   nano ~/appdata/homepage/docker-compose.yml
   docker compose up -d
   ```

2. **Stop and Clean Up**:
   ```bash
   docker compose down
   ```

---

This guide ensures a clean and reusable setup for running Homepage on your Raspberry Pi with Docker Compose. 