---
title: uiowa-docker-hub-access
created: 2024-11-13
type: resource
category: 
tags:
  - resource
  - uiowa
  - docker
  - docker-compose
related_docs:
---
**Modified:**

# **Resource Overview**

- Cody has set up proxy access for us to docker hub for grabbing docker images

# **Key Points**

- when working with docker compose files this means we need to just add the below line to the image name

```shell
rsctr.hpc.uiowa.edu/dockerhub/
```

# **Example**

```yaml
# postgres
  postgres:
    image: rsctr.hpc.uiowa.edu/dockerhub/postgres:16-alpine
    healthcheck:
      test: pg_isready -q -t 2 -d $$POSTGRES_DB -U $$POSTGRES_USER
      start_period: 20s
      timeout: 30s
      interval: 10s
      retries: 5
    env_file: env/postgres.env
    volumes:
      - netbox-postgres-data:/var/lib/postgresql/data
```

