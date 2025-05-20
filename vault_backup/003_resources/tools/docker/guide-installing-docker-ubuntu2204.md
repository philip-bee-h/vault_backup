---
title: guide-installing-docker-ubuntu22.04
created: 2024-11-01
modified: 2024-11-01
type: process
system:
  - linux
  - docker
tags:
  - process
  - procedure
  - homelab
  - docker
related_docs:
---


## Installing Docker on Ubuntu Server 22.04

Docker is a fantastic tool for deploying containerized applications like Nextcloud, WordPress, or an in-house cloud-based office suite. It's user-friendly and supports a wide range of applications with GUI options, making it less daunting for those new to containers. Below is a step-by-step guide to installing Docker on Ubuntu Server 22.04.

### What You'll Need
- An Ubuntu Server 22.04 system
- A user account with sudo privileges

### Installation Steps

#### 1. Add the Docker Repository
First, you need to add Docker's official GPG key and repository to your system to ensure you get the latest version. Open your terminal and enter the following commands:

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

#### 2. Install Required Dependencies
Before installing Docker, you must install some prerequisites. Run the following command:

```bash
sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release -y
```

#### 3. Install Docker
Now, update your package index and install Docker by executing:

```bash
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io -y
```

#### 4. Add Your User to the Docker Group
To use Docker as a non-root user, you should add your user to the Docker group with:

```bash
sudo usermod -aG docker $USER
```

Remember to log out and log back in for the group changes to take effect.

### Testing the Installation

Verify your Docker installation and check the Docker version by running:

```bash
docker version
```

To ensure everything is working correctly, try running the hello-world Docker image:

```bash
docker pull hello-world
```

If the image pulls successfully, Docker is installed and ready for use on your system.

### Conclusion

Congratulations! You've successfully installed Docker on Ubuntu Server 22.04. You're now ready to explore the vast world of containerized applications and services. Docker simplifies deployment and is an invaluable tool for managing and running your applications efficiently. Happy Dockering!

Remember, this guide is specifically for Ubuntu Server 22.04. If you're using a different version or distribution, some steps may vary.