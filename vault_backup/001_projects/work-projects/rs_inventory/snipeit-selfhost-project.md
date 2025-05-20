---
title: snipeit-selfhost-project
created: 2023-10-27
modified: 2024-10-27
type: project
status: active
tags:
  - project
  - docker
  - infrastructure
  - uiowa
---


# Snipe-IT Deployment Project Notes

---

### 1. Snipe-IT Deployed on Development VM

**Directory Setup for Docker Images**
- Store Docker images in the `app_data` directory instead of `docker` for better organization.

```plaintext
~/
|- app_data
    |- app_title
       |- docker-compose.yaml
```

**Useful Resources**
- [Snipe-IT Docker Documentation](https://snipe-it.readme.io/docs/docker)

**Commands to Simplify Usage**
- Add your user to the Docker group:
  ```shell
  sudo usermod -aG docker <hawkID>
  ```

**Email Configuration**
- Recommended email host: `ns-mx.uiowa.edu`

**Access Docker Hub Images**
- Login to Docker Hub:
  ```shell
  docker login https://rsctr.hpc.uiowa.edu
  ```
- Pull Snipe-IT Docker image:
  ```shell
  docker pull rsctr.hpc.uiowa.edu/dockerhub/snipe/snipe-it:v7.0.12
  ```

**Port Forwarding/Proxy Commands**
- Set up local port forwarding:
  ```shell
  ssh -L 8000:localhost:8000 <serverName>
  ```
  ```shell
  ssh -L 8000:localhost:8000 endor.hpc.uiowa.edu
  ```
  - Access via: [http://localhost:8000](http://localhost:8000)

**To-Do Checklist**
- Ensure the instance is up and running with at least 25 units
- Set up SAML authentication: [SAML Authentication Documentation](https://snipe-it.readme.io/docs/saml)

---

### 2. Self-Hosted Instance on Lenovo T450S

**Server Information for Lenovo T450S**
- **OS**: Ubuntu 22.04
- **Configuration Details**:
  - Hostname: `hoth`
  - Username: `hawrth`
  - Password: `Illphil11!`
  - **SSH**: `ssh hawrth@192.168.0.241`

```plaintext
IP: 192.168.0.241
```

**SSH Access**
```shell
ssh hawrth@192.168.0.241
```

**Work Log**

1. **Initial Setup**:
   - OS installed, static IP configured, and updates applied.
   - `.bashrc` and `.vimrc` configured.

2. **Docker Installation**:
   ```shell
   curl -sSL https://get.docker.com | sh
   ```

3. **Snipe-IT Installation Process**:
   - Pull Snipe-IT Docker image:
     ```shell
     docker pull snipe/snipe-it
     ```
   - Create necessary directories and environment files:
     ```shell
     mkdir docker/snipe-it
     vim my_env_file
     ```
   - Sample environment file (`my_env_file`):
     ```yaml
     MYSQL_ROOT_PASSWORD=rootpassword
     MYSQL_DATABASE=snipeit
     MYSQL_USER=user1
     MYSQL_PASSWORD=password
     MAIL_PORT_587_TCP_ADDR=ns-mx.uiowa.edu
     MAIL_PORT_587_TCP_PORT=587
     MAIL_ENV_FROM_ADDR=youremail@yourdomain.com
     MAIL_ENV_FROM_NAME=Your Full Email Name
     MAIL_ENV_ENCRYPTION=tcp
     APP_ENV=production
     APP_DEBUG=false
     APP_KEY=<<Fill in Later!>>
     APP_URL=http://192.168.0.241:5118
     APP_TIMEZONE=US/Chicago
     PHP_UPLOAD_LIMIT=100
     ```

   - Run MySQL container:
     ```shell
     docker run --name snipe-mysql --env-file=my_env_file --mount source=snipesql-vol,target=/var/lib/mysql -d -P mysql:5.6
     ```

   - Launch Snipe-IT with Docker:
     ```shell
     docker run -d --name snipe-it --network snipe-network --env-file ./my_env_file snipe/snipe-it
     ```

**Helpful Resources**
- [Running Snipe-IT in Docker on Ubuntu Server](https://i12bretro.wordpress.com/2023/12/14/running-snipe-it-in-docker-on-ubuntu-server/)
- [Snipe-IT Docker Deployment Guide](https://blog.networkprofile.org/quick-and-easy-snipe-it-docker-deployment/)

**Production Docker-Compose File Example**
- Ensure volumes and services are correctly set in the `docker-compose.yaml` file.

---

### Additional Notes

**Authentication Methods**:
- LDAP, OIDC options available for integrating authentication.

---

This setup guide should help keep the dev and self-hosted instance configurations organized. Let me know if you need further customization or additional deployment assistance.