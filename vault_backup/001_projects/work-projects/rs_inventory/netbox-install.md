---
title: netbox-install
created: 2024-11-08
type: process
system: 
tags:
  - process
  - procedure
  - uiowa
  - network
  - docker
related_docs:
---
**Modified:**

# Netbox Access:
admin 
test


```shell
ssh -L 8001:localhost:8001 endor.hpc.uiowa.edu
```


```web
http://localhost:8001
```

## Link: 
https://gitlab.com/network.tocoder/api-programing-netbox/-/blob/main/NetBox%20Installation%20Guide.md?ref_type=heads
  
# What is Netbox?

NetBox is like a super handy tool for managing your network stuff. It helps keep track of things like IP addresses and racks. Its RESTful API enables seamless integration with other systems, facilitating dynamic inventory management for tools like Ansible. Furthermore, NetBox's programmability extends beyond the API, allowing users to automate tasks, customize workflows, and enhance functionality through custom scripts and plugins. This flexibility empowers organizations to adapt NetBox to their specific network management needs, ensuring efficient and reliable operation. Additionally, NetBox plays a crucial role in CI/CD pipelines by providing accurate infrastructure information, facilitating smooth deployment and testing processes.

# Deployment

To setup Netbox on Linux distrubtion -  run following steps

## 1. install Docker (Ubuntu Linux)

### Setup Docker's apt repository

#### Add Docker's official GPG key

```bash
  sudo apt-get update
  sudo apt-get install ca-certificates curl
  sudo install -m 0755 -d /etc/apt/keyrings
  sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/ docker. asc
  sudo chmod a+r /etc/apt/keyrings/docker.asc
```

#### Add the repository to apt sources

```bash
   echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  sudo apt-get update
```

#### Install Docker CE

```bash
  sudo apt install docker-ce
```

#### Verify Docker Engine status

```bash
  sudo docker ps
  sudo docker info
```

## 2. Install Docker Compose

#### Download latest docker compose release ( Replace VERSION with the version you want to install (e.g., 1.29.2):)


```bash
   sudo curl -L "https://github.com/docker/compose/releases/download/VERSION/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
#### Example to install 1.29.2 version
```bash
   sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```


#### Set the correct permissions so that the docker-compose command is executable

```bash
   sudo chmod +x /usr/local/bin/docker-compose
```

#### Test and execute compose commands using docker-compose

```bash
  docker-compose --version
```

## 3. Install Netbox

#### Navigate to Netbox Community GIthub page / Select Netbox-docker


```bash
   https://github.com/netbox-community/netbox-docker
```

#### Clone repo to local system

```bash
   git clone https://github.com/netbox-community/netbox-docker.git 

```
Switch to netbox directory
```bash
   cd netbox-docker && ll
   
```

#### Adjust port mapping from 8000 to 8080 (under netbox community image , line#5 )

##### on VS code editior
```bash
  code docker-compose.yml

    ports:
     - "8000:8080" # adjust the port mapping
```

#### Download required docker images

```bash
   sudo docker-compose pull 
```

```bash
$ sudo docker images    
    REPOSITORY               TAG          IMAGE ID       CREATED        SIZE
    netboxcommunity/netbox   v3.7-2.8.0   8223cac91c72   9 days ago     663MB
    postgres                 16-alpine    9a510ccf1de4   6 weeks ago    243MB
    redis                    7-alpine     435993df2c8d   2 months ago   41MB
```

#### Bring up dockers using compose command

```bash
   sudo docker-compose up
```


#### Create first admin user 

```bash
   sudo docker-compose exec netbox /opt/netbox/netbox/manage.py createsuperuser
```

## Troubleshooting

### Incase any issue with containter - remove container 

```bash
docker container ls -a
docker container stop container_id
docker container rm container_id
```
### Incase any with port numbering

```bash
sudo lsof -i :8000

```


# Test Netbox GUI Access - http://hostip:8000