---
title: homelab-monitoring
created: 2024-11-16
type: project
status: 
tags:
  - project
  - homelab
  - monitoring
  - raspberry-pi
  - docker
  - docker-compose
related_docs:
---





```yaml
[Unit]
Description=Prometheus Node Exporter
After=network.target

[Service]
User=pbh
ExecStart=/home/pbh/node_exporter-1.8.2.linux-amd64/node_exporter
Restart=always

[Install]
WantedBy=multi-user.target

```


