---
title: docker-install-debian-system
created: 2024-11-01
modified: 2024-11-01
type: process
system:
  - docker
  - linux
  - infrastructure
tags:
  - process
  - procedure
  - uiowa
  - docker
  - ubuntu
---

## **1. Purpose**

This document outlines the step-by-step procedure to install Docker and its associated components on a fresh Debian-based Linux system. Following this guide ensures a proper and secure Docker setup, enabling containerization and management of applications.

## **2. Prerequisites**

- **Operating System:** Debian-based Linux distribution (e.g., Ubuntu 20.04 LTS or later).
- **User Privileges:** Access to a user account with `sudo` (administrative) privileges.
- **Internet Connection:** Required to download packages and Docker components.

## **3. Step-by-Step Installation Guide**

### **Step 1: Update Existing Packages**

Begin by updating your system's package index to ensure all existing packages are up to date.

```bash
sudo apt-get update
```

### **Step 2: Install Prerequisite Packages**

Install packages that allow `apt` to use repositories over HTTPS.

```bash
sudo apt-get install \
  ca-certificates \
  curl \
  gnupg \
  lsb-release
```

- **`ca-certificates`**: Verifies SSL certificates.
- **`curl`**: Transfers data from or to a server.
- **`gnupg`**: Provides encryption and signing tools.
- **`lsb-release`**: Displays Linux Standard Base information.

### **Step 3: Add Docker’s Official GPG Key**

Import Docker’s GPG key to ensure the authenticity of the Docker packages.

```bash
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```

- **Explanation:**
  - **`curl -fsSL`**: Downloads Docker's GPG key.
  - **`gpg --dearmor`**: Converts the GPG key to a format usable by `apt`.
  - **`/etc/apt/keyrings/docker.gpg`**: Destination for the converted GPG key.

### **Step 4: Set Up the Docker Repository**

Configure the Docker repository to enable `apt` to fetch Docker packages.

```bash
echo "deb [arch=arm64 signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian bookworm stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

- **Components:**
  - **`deb [arch=...]`**: Specifies the architecture and repository URL.
  - **`signed-by=/etc/apt/keyrings/docker.gpg`**: Links the repository to Docker's GPG key.
  - **`$(lsb_release -cs)`**: Dynamically inserts the codename of your Ubuntu release (e.g., `focal` for Ubuntu 20.04).

### **Step 5: Update the Package Index Again**

Refresh the package index to include packages from the newly added Docker repository.

```bash
sudo apt-get update
```

### **Step 6: Install Docker and Its Components**

Proceed to install Docker along with its essential components and plugins.

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

- **Components Installed:**
  - **`docker-ce`**: Docker Community Edition (Docker Engine).
  - **`docker-ce-cli`**: Command-Line Interface for Docker.
  - **`containerd.io`**: High-level container runtime.
  - **`docker-buildx-plugin`**: Enhanced build capabilities (supports multi-platform builds).
  - **`docker-compose-plugin`**: Enables Docker Compose functionality.

### **Step 7: Verify the Installation**

Ensure Docker is installed correctly by checking its version and running a test container.

#### **a. Check Docker Version**

```bash
docker --version
```

**Expected Output:**
```
Docker version 24.0.0, build <commit-hash>
```
*(Version number may vary based on the latest release.)*

#### **b. Run a Test Container**

```bash
sudo docker run hello-world
```

**What It Does:**
- Downloads the `hello-world` image from Docker Hub.
- Runs the container, which prints a "Hello from Docker!" message.

**Successful Output Example:**
```
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

### **Step 8: (Optional) Manage Docker as a Non-Root User**

By default, Docker commands require `sudo` privileges. To run Docker commands without `sudo`, add your user to the `docker` group.

#### **a. Create the Docker Group (If It Doesn't Exist)**

```bash
sudo groupadd docker
```

#### **b. Add Your User to the Docker Group**

```bash
sudo usermod -aG docker $USER
```
- **`$USER`**: Automatically uses your current username.

#### **c. Apply the New Group Membership**

- **Option 1: Log Out and Log Back In**
  - This ensures that your group membership is re-evaluated.

- **Option 2: Use `newgrp` Command**

  ```bash
  newgrp docker
  ```

#### **d. Verify Without `sudo`**

```bash
docker run hello-world
```

**Successful Output:** Same as in Step 7b, but without using `sudo`.

## **4. Summary**

To install Docker on a **fresh Debian-based system**, follow these essential steps:

1. **Update existing packages.**
2. **Install prerequisite packages** for repository setup.
3. **Add Docker’s official GPG key.**
4. **Set up the Docker repository.**
5. **Update the package index** to include Docker packages.
6. **Install Docker and its components** using `apt-get install`.
7. **Verify the installation** by checking Docker’s version and running a test container.
8. **(Optional) Manage Docker as a non-root user** by adding your user to the `docker` group.

## **5. Additional Notes**

- **Security Consideration:** Adding your user to the `docker` group grants privileges equivalent to the `root` user for Docker operations. Ensure you understand the security implications before proceeding.
- **Repository Updates:** Always refer to the [official Docker installation guide](https://docs.docker.com/engine/install/) for the most up-to-date instructions, as procedures may evolve over time.
- **Troubleshooting:** If you encounter issues during installation, consult Docker’s [official troubleshooting documentation](https://docs.docker.com/engine/install/troubleshoot/).

## **6. References**

- **Official Docker Installation Guide:** [Docker Docs - Install Docker Engine](https://docs.docker.com/engine/install/)
- **Docker Compose Documentation:** [Docker Docs - Docker Compose](https://docs.docker.com/compose/)
- **Docker Buildx Documentation:** [Docker Docs - Buildx](https://docs.docker.com/buildx/working-with-buildx/)
- **Post-Installation Steps for Linux:** [Docker Docs - Post-installation steps for Linux](https://docs.docker.com/engine/install/linux-postinstall/)

---

**Caution:** Installing Docker requires administrative privileges and can make significant changes to your system. Ensure you understand each step and its implications, especially when adding your user to the `docker` group.