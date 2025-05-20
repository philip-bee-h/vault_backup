---
title: apt-system-update-script
created: 2024-11-30
type: Script
system: 
tags:
  - script
  - bash
related_docs:
---
```sh
#!/bin/bash

sudo apt update
sudo apt upgrade -y
sudo apt autoremove -y
sudo apt clean
echo "System update completed"
```

