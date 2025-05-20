---
title: standing-up-k8s-in-homelab
created: 2025-04-30
type: project
status: 
tags:
  - project
  - kubernetes
  - homelab
  - process
  - working-note
related_docs:
---
# info

- k8s-ctrl-01 
	- 192.168.20.10
	- Control Plane
	- Beelink Mini S12 Pro Mini Pc,Intel 12th Gen Alder Lake-N100 (4C/4T, Up to 3.4GHz),16GB DDR4 RAM 500GB PCle SSD
- k8s-worker-01
	- 192.168.20.11
	- Worker node
	- HP EliteDesk 800 G3 35W Mini PC Computer - Intel i5-6500T 2.5Ghz, 32GB memory, 500gb ssd
- k8s-worker-02
	- 192.168.20.12
	- worker node
	- HP EliteDesk 800 G3 35W Mini PC Computer - Intel i5-6500T 2.5Ghz, 32GB memory, 500gb ssd

---

# work log

- [x] hardware upgrades
	- [x] ram upgrades in the 2 hp's
	- [x] wiped and installed ssd's
- [x] imaged all three machines with Ubuntu 22/04
	- [x] update and reboot
- [x] gen ssh keys
- [x] add to github
- [ ] set up dots
- [ ] install core apps
- [ ] test network ports



---

# Troubleshooting

## gpg key and repo change
## 3 other issues

### **1. Misconfigured or Missing CRI Endpoint in `crictl`**

- **Problem**: The `crictl` command was trying to use default endpoints like `/var/run/dockershim.sock`, which don't exist in your environment because you're using `containerd` instead of Docker.
- **Solution**: Setting the correct CRI socket in `/etc/crictl.yaml` as `runtime-endpoint: unix:///run/containerd/containerd.sock` resolved this.

---

### **2. Kubelet Connection to the API Server**

- **Problem**: The kubelet was unable to establish a connection with the Kubernetes API server at `https://192.168.20.10:6443`. This is evident from the errors like:
    
    ```
    dial tcp 192.168.20.10:6443: connect: connection refused
    ```
    
    - This was likely caused by an issue in the `/etc/default/kubelet` configuration. For example, the `--resolv-conf` flag might have been missing or incorrectly set.
    - Ubuntu's use of `systemd-resolved` creates multiple potential locations for the resolv.conf file (`/run/systemd/resolve/resolv.conf` vs `/etc/resolv.conf`), and the wrong one can lead to DNS or API connectivity issues.
- **Solution**:
    
    - You modified `/etc/default/kubelet` and added the correct `--resolv-conf` flag pointing to `/run/systemd/resolve/resolv.conf`. This ensured proper DNS resolution for kubelet communication.
    - Restarting the kubelet (`sudo systemctl restart kubelet`) then allowed it to reconnect to the API server successfully.

---

### **3. Correct Authentication with API Server**

- **Problem**: The `curl` command showed a `403 Forbidden` error because it was unauthenticated and treated as `system:anonymous`. This is expected behavior when no credentials are provided.
- **Solution**: The `kubectl` configuration (`kubectl config view`) showed that the `kubernetes-admin` context was correctly set. Once the API server was reachable and kubelet was configured properly, `kubectl get nodes` worked without issue.

---

### **Summary**

The issue was resolved by:

1. **Fixing the CRI endpoint**: Configuring `crictl` to explicitly use the `containerd` socket.
2. **Correcting kubelet's `--resolv-conf` setting**: Ensuring kubelet was using `/run/systemd/resolve/resolv.conf` for proper DNS resolution.
3. **Restarting kubelet**: Allowing it to re-establish communication with the API server.

Once these were addressed, your cluster came back online, and all nodes showed as `Ready`.

---

Let me know if you want help reviewing or refining any part of your setup further! 🚀

