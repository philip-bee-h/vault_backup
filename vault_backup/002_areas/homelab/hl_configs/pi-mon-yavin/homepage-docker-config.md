---
title: homepage-docker-config
created: 2024-11-14
type: resource
category: 
tags:
  - resource
  - service-config
  - homelab
  - docker
  - docker-compose
related_docs:
---

## **Service Details**
- **Service Name**: Homepage
- **Host Device**: [[01-pi-mon-yavin]]
- **Purpose**: Provides a customizable homepage with shortcuts and monitoring.
- **Resource Links**: https://gethomepage.dev/installation/

## ssh access
```sh
ssh pbh@192.168.10.6
```
---

## **Deployment Configuration**

### **Docker Compose File**
- **Path**: `~/appdata/homepage/docker-compose.yml`

```yaml
services:
  homepage:
    image: ghcr.io/gethomepage/homepage:latest
    container_name: homepage
    ports:
      - "3000:3000"
    volumes:
      - ./config:/app/config
      # Docker socket monitoring is optional:
      # - /var/run/docker.sock:/var/run/docker.sock
    environment:
      PUID: $PUID
      PGID: $PGID
    restart: unless-stopped
```

---

### **Environment Variables**
- **File Location**: `~/appdata/homepage/.env`
- **Content**:
```ini
PUID=1000
PGID=1000
```

---

## **Host Configuration**

### **Config Directory**
- **Location**: `~/appdata/homepage/config`
- **Purpose**: Stores all configuration files for Homepage.
- **Permissions**:
  - Owner: `1000:1000` (matches PUID/PGID in `.env`)
  - Permissions: `755`
- Commands to set up:
```bash
mkdir -p ~/appdata/homepage/config
chown -R 1000:1000 ~/appdata/homepage/config
chmod -R 755 ~/appdata/homepage/config
```

---

## **Customizations**
- **Host Port**: 
  - Exposed on `3000` (default, no conflicts found).
- **Optional Features**:
  - Docker socket integration for container monitoring: Uncomment the corresponding volume in `docker-compose.yml`.

## Resource Links

- Icons
	- https://github.com/walkxcode/dashboard-icons


---

## **Notes**
1. **Reset Configuration**:
   - Run `docker compose down --volumes` if a full reset is needed (clears config data).
2. **Reapply Changes**:
   - After modifying the `.env` file or `config`, restart the container:
 ```bash
 docker compose up -d
 ```
1. **Logs**:
   - Check service logs with:
 ```bash
 docker logs homepage
 ```
