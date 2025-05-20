---
title: new-cluster-hl-k8s
created: 2025-05-13
type: general
tags:
  - general
  - topic-tag
  - k8s
  - setup
---
**Modified:**

## Server/computer names

vim-diesel
ssh-ia-labeouf
Keanu reboot
YAML L. Jackson
proxy-balboa
leapanardo-dicaprio



# Home Lab Kubernetes Plan

---

## Phase 1: Hardware & Role Assignment

|Device|Specs|Assigned Role|
|---|---|---|
|OptiPlex 9010|i5, 32GB RAM, SSD|Control Plane Node|
|HP G3 Mini #1|i5, 32GB RAM|Worker Node + Longhorn Storage|
|HP G3 Mini #2|i5, 32GB RAM|Worker Node + Longhorn Storage|
|Beelink Mini PC|8–16GB RAM|Jump Host + Monitoring Tools|
|MacBook Pro 2012|i7, 16GB RAM|Optional Worker / Dev Node|

---

## Phase 2: Kubernetes the Hard Way

**Objective:** Learn Kubernetes fundamentals by building a cluster manually without kubeadm or K3s.

**On the Beelink (Jump Host):**

Install:

- `openssh-server`, `tmux`, `curl`, `git`
- `kubectl`, `k9s`, `helm`
- `ansible`
- `syncthing` (optional, for config sync)
    

Tasks:

- SSH into each node from Beelink
    
- Install and configure:
    
    - etcd
        
    - containerd
        
    - kube-apiserver, controller-manager, scheduler, kubelet, and kube-proxy
        
    - TLS certificates and encryption
        
    - kubeconfig files and RBAC
        
- Use Ansible to automate and repeat configurations where possible
    

Expected Result:

- Fully functioning Kubernetes cluster with deep understanding of its components
    

---

## Phase 3: Wipe and Rebuild with K3s

**Objective:** Replace the manual setup with a lightweight production-grade cluster using K3s.

**K3s Cluster Layout:**

|Node|K3s Role|
|---|---|
|OptiPlex|Control Plane + Longhorn Storage|
|HP G3 Mini #1|Worker Node + Longhorn Storage|
|HP G3 Mini #2|Worker Node + Longhorn Storage|
|MacBook Pro|Optional Worker Node (testing)|
|Beelink|Not a cluster node; admin tools only|

**Install using `k3sup` from the Beelink:**

```bash
k3sup install --ip <optiplex-ip> --user ubuntu
k3sup join --ip <hp1-ip> --user ubuntu --server-ip <optiplex-ip>
k3sup join --ip <hp2-ip> --user ubuntu --server-ip <optiplex-ip>
```

---

## Tools to Install on the Beelink (Jump Host)

- `kubectl` – Manage the K3s cluster
    
- `k9s` – Terminal UI for Kubernetes
    
- `helm` – Deploy and manage Helm charts
    
- `ansible` – Automate setup across nodes
    
- `tmux` – Persistent shell sessions
    
- `tailscale` or `wireguard` – VPN access to your cluster
    
- `syncthing` – Sync scripts or configs across devices
    
- `git` – Manage playbooks and manifests with version control
    

---

## Tools to Deploy Inside the Cluster

Install using Helm or Kubernetes manifests:

|Tool|Purpose|
|---|---|
|Longhorn|Distributed persistent storage|
|Prometheus & Grafana|Monitoring and alerting|
|Loki or EFK Stack|Centralized logging|
|NGINX or Traefik|Ingress controller|
|cert-manager|Automatic TLS certificates|

---

## Best Practices

- Assign static IPs or set DHCP reservations for all nodes
    
- Use SSH keys for secure, passwordless access between nodes
    
- Store kubeconfig files on the Beelink and your MacBook
    
- Backup Longhorn volumes regularly
    
- Use `kubectl port-forward` to access internal services through the Beelink
    
- Use Ansible to manage updates, package installs, and reboots
    

---

## Summary

- Phase 1: Define roles and install Ubuntu/Debian as needed
    
- Phase 2: Learn Kubernetes internals by building it manually
    
- Phase 3: Deploy K3s for long-term use and easier management
    
- Beelink serves as the centralized admin hub, not a cluster node
    
- MacBook Pro can be optionally used as a worker node or for testing
    

---

Would you like setup scripts for:

- Installing and configuring tools on the Beelink?
    
- An Ansible playbook to install K3s or system packages on all nodes?
    

Just let me know.