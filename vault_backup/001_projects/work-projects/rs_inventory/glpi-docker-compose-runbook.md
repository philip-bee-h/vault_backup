---
title: glpi-docker-compose-runbook
created: ""
modified: YYYY-MM-DD
type: runbook
system: 
status: 
tags:
  - operations
  - runbook
  - infrastructure
  - docker
related_docs:
---
## Usage

### Git clone

You can deploy it easily with the following command:

```bash
git clone https://github.com/elestio-examples/glpi.git
```

Copy the .env file from tests folder to the project directory

```bash
cp ./tests/.env ./.env
```

Edit the .env file with your own values.

Run the project with the following command

```bash
docker-compose up -d
```

You can access the Web UI at: `http://your-domain:22571`

### Docker-compose

Here are some example snippets to help you get started creating a container.

```bash
version: "3.3"

services:
    mysql:
        image: elestio/mysql:8.0
        restart: always
        hostname: mysql
        volumes:
            - ./storage/mysql:/var/lib/mysql
        env_file:
            - ./.env
        ports:
            - "172.17.0.1:50778:3306"

    glpi:
        image: elestio/glpi:latest
        restart: always
        hostname: glpi
        ports:
            - "172.17.0.1:22571:80"
        volumes:
            - /etc/timezone:/etc/timezone:ro
            - /etc/localtime:/etc/localtime:ro
            - ./storage/var/www/html/glpi/:/var/www/html/glpi
        environment:
            - TIMEZONE=Europe/Brussels

    pma:
        image: phpmyadmin
        restart: always
        links:
            - mysql:mysql
        ports:
            - "172.17.0.1:22811:80"
        environment:
          PMA_HOST: mysql
          PMA_PORT: 3306
          PMA_USER: root
          PMA_PASSWORD: ${ADMIN_PASSWORD}
          UPLOAD_LIMIT: 500M
          MYSQL_USERNAME: glpi
          MYSQL_ROOT_PASSWORD: ${ADMIN_PASSWORD}
        depends_on:
            - mysql
```

## Environment variables

|Variable|Value (example)|
|---|---|
|ADMIN_EMAIL|[admin@email.com⁠](mailto:admin@email.com)|
|ADMIN_PASSWORD|your-password|
|DOMAIN|your.domain.com|
|MYSQL_ROOT_PASSWORD|your-password|
|MYSQL_DATABASE|glpidb|
|MYSQL_USER|glpi|
|MYSQL_PASSWORD|your-password|

## Maintenance

### Logging

The Elestio GLPI Docker image sends the container logs to stdout. To view the logs, you can use the following command:

```bash
docker-compose logs -f
```

To stop the stack you can use the following command:

```bash
docker-compose down
```

### Backup and Restore with Docker Compose

To make backup and restore operations easier, we are using folder volume mounts. You can simply stop your stack with docker-compose down, then backup all the files and subfolders in the folder near the docker-compose.yml file.

Creating a ZIP Archive For example, if you want to create a ZIP archive, navigate to the folder where you have your docker-compose.yml file and use this command:

```bash
zip -r myarchive.zip .
```

Restoring from ZIP Archive To restore from a ZIP archive, unzip the archive into the original folder using the following command:

```bash
unzip myarchive.zip -d /path/to/original/folder
```

Starting Your Stack Once your backup is complete, you can start your stack again with the following command:

```bash
docker-compose up -d
```

That's it! With these simple steps, you can easily backup and restore your data volumes using Docker Compose.

### Links

- [GLPI Github repository⁠](https://github.com/glpi-project/glpi)
    
- [GLPI documentation⁠](https://glpi-project.org/documentation/)
    
- [Elestio/glpi Github repository⁠](https://github.com/elestio-examples/glpi)