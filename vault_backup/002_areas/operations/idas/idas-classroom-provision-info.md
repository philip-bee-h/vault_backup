---
title: "idas-classroom-provision-info"
created: 2024-10-28
modified: 2024-10-28
type: reference
system: idas
tags:
  - idas
  - jupyterhub
  - provisioning
  - classroom
  - uiowa
---

Information needed to fill out the steps from wiki article

```bash
idas-classroom-provision 


Ecosystem Ecology - GEOG:3315:0001


    location /fall2024-geog-3315-0001 {
        add_header Access-Control-Allow-Origin https://idp.uiowa.edu;
        proxy_pass http://jupyterhub-proxy-fall2024-geog-3315-0001.jhub-fall2024-geog-3315-0001.svc.cluster.local:8000/fall2024-geog-3315-0001;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        #proxy_set_header hawkID $shib_hawkID;
        proxy_set_header Access-Control-Allow-Origin https://idp.uiowa.edu;


        # websocket headers
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $connection_upgrade;
        proxy_read_timeout 20d;
        proxy_connect_timeout 180s;
        proxy_buffers 16 1024m;
        proxy_buffer_size 1024m;
        fastcgi_buffers 16 1024m;
        fastcgi_buffer_size 1024m;
      }


<div class="col-md-4">
              <p><a class="btn btn-secondary btn-block" href="https://notebooks.hpc.uiowa.edu/fall2024-geog-3315-0001/hub/home"><span class="sr-only">Jupyter Notebook </span>Ecosystem Ecology - GEOG:3315:0001</a></p>
        </div>



,'fall2024-geog-3315-0001'


https://notebooks.hpc.uiowa.edu/fall2024-geog-3315-0001/hub/oauth_callback
```

