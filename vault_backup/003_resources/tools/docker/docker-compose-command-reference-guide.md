---
title: docker-reference-guide
created: YYYY-MM-DD
modified: YYYY-MM-DD
type: resource
category:
  - tool|system|procedure
tags:
  - resource
  - tool-name
  - uiowa
related_docs:
---



### **Introduction**

This reference guide consolidates essential commands for managing Docker and Docker Compose, with a primary focus on Docker Compose. It includes commands for image and container management, networking, volumes, and resource cleanup, ensuring you have a comprehensive toolkit for your containerized workflows.

## Docker Commands

#### **Image Management**

- **List All Images**
```bash
docker images
```

- **Pull an Image from a Registry**
```bash
docker pull <image_name>:<tag>
```

- **Remove an Image**
```bash
docker rmi <image_name>:<tag>
```

- **Build an Image from a Dockerfile**
```bash
docker build -t <image_name>:<tag> .
```

#### **Container Management**

- **List Running Containers**
```bash
docker ps
```

- **List All Containers (Including Stopped)**
```bash
docker ps -a
```

- **Run a New Container**
```bash
docker run -d --name <container_name> <image_name>:<tag>
```

- **Stop a Running Container**
```bash
docker stop <container_name>
```

- **Start a Stopped Container**
```bash
docker start <container_name>
```

- **Remove a Container**
```bash
docker rm <container_name>
```

- **View Container Logs**
```bash
docker logs <container_name>
```

#### **Networking**

- **List All Networks**
```bash
docker network ls
```

- **Create a New Network**
```bash
docker network create <network_name>
```

- **Connect a Container to a Network**
```bash
docker network connect <network_name> <container_name>
```

- **Disconnect a Container from a Network**
```bash
docker network disconnect <network_name> <container_name>
```

#### **Volumes**

- **List All Volumes**
```bash
docker volume ls
```

- **Create a New Volume**
```bash
docker volume create <volume_name>
```

- **Remove a Volume**
```bash
docker volume rm <volume_name>
```

#### **Cleaning Up Resources**

- **Prune Unused Docker Objects (Containers, Networks, Images, and optionally Volumes)**
```bash
docker system prune
```

- **Include Volumes**
```bash
docker system prune --volumes
```

- **Force Prune Without Confirmation**
```bash
docker system prune -f
```

- **Force Prune All Unused Images (Not Just Dangling)**
```shell
docker system prune -a -f
```

- **Prune Images**
```bash
docker image prune
```

- **Remove All Unused Images (Not Just Dangling)**
```bash
docker image prune -a
```

- **Prune Containers**
```bash
docker container prune
```

- **Prune Volumes**
```bash
docker volume prune
```

- **Prune Networks**
```bash
docker network prune
```

---

### Docker Compose Commands

#### **Basic Commands**

1. **Start and Run All Containers in Detached Mode**
```bash
docker-compose up -d
```

2. **Stop and Remove Containers (Keep Volumes and Networks)**
```bash
docker-compose down
```

3. **View the Status of All Running Containers**
```bash
docker-compose ps
```

4. **Follow the Logs for All Containers (Real-Time Output)**
```bash
docker-compose logs -f
```

5. **Follow the Logs for a Specific Container**
```bash
docker-compose logs -f <service-name>
```

6. **Build or Rebuild Services**
```bash
docker-compose build
```

7. **Stop All Running Containers Without Removing Them**
```bash
docker-compose stop
```

8. **Start Stopped Containers Again**
```bash
docker-compose start
```

9. **Restart Containers (Useful After Configuration Changes)**
```bash
docker-compose restart
```

10. **Bring Up Containers Without Using Cache (Useful During Development)**
```bash
docker-compose up --build --no-cache
```

#### **Advanced Commands**

11. **Remove Stopped Containers, Networks, Volumes, and Images Not Used by Any Containers**
```bash
docker-compose down --volumes --rmi all
```

12. **Run a One-Time Command Inside a Running Container**
```bash
docker-compose exec <service-name> <command>
```
*Example: Running a shell in a container*
```bash
docker-compose exec <service-name> /bin/bash
```

13. **View Container Resource Usage (CPU, Memory, etc.)**
```bash
docker stats
```

14. **Scale a Specific Service to a Certain Number of Containers**
```bash
docker-compose up -d --scale <service-name>=<number-of-containers>
```

#### **Container Cleanup Commands**

15. **Remove All Stopped Containers and Unused Networks**
```bash
docker system prune
```

---

### Example Use Cases

1. **Start and Run Containers in the Background**
```bash
docker-compose up -d
```

2. **Check the Status of Running Containers**
```bash
docker-compose ps
```

3. **Restart Containers After a Configuration Change**
```bash
docker-compose restart
```

---

### Common Commands Cheat Sheet

| **Action**                        | **Docker Command**                           | **Docker Compose Command**                      |
|-----------------------------------|----------------------------------------------|-------------------------------------------------|
| List all containers               | `docker ps -a`                                | `docker-compose ps`                             |
| Start containers                  | `docker start <container>`                   | `docker-compose up`                             |
| Stop containers                   | `docker stop <container>`                    | `docker-compose down`                           |
| View logs                         | `docker logs <container>`                    | `docker-compose logs`                           |
| Execute command in container       | `docker exec -it <container> <command>`      | `docker-compose exec <service> <command>`        |
| Build images                      | `docker build -t <image> .`                  | `docker-compose build`                          |
| Remove containers                  | `docker rm <container>`                      | `docker-compose down --rmi all`                  |
| Remove images                      | `docker rmi <image>`                          | N/A                                             |
| Prune unused resources             | `docker system prune`                         | N/A                                             |

---

### Quick Tips

- **Always run `docker-compose down` before making major changes** to your containers or networks to ensure they’re cleanly shut down.

- **Use `.dockerignore` Files**: Exclude unnecessary files from the build context to speed up image builds.

- **Tag Images Appropriately**: Use meaningful tags for easier identification and versioning.

- **Leverage Multi-Stage Builds**: Optimize image sizes by using multi-stage Dockerfiles.

- **Manage Secrets Securely**: Use Docker secrets or environment variables to handle sensitive information.

- **Regularly Update Images**: Keep your images up-to-date to incorporate security patches and improvements.

- **Monitor Resource Usage**: Use tools like `docker stats` to monitor container performance.

- **Automate Cleanup Tasks**: Schedule regular pruning to maintain a clean environment.

- **Use Docker Compose for Development**: Simplify multi-container setups during development with Docker Compose.

---
