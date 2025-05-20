---
title: k8s-setup-guide
created: 2025-01-11
type: process
system: 
tags:
  - process
  - procedure
  - kubernetes
  - install-guide
  - homelab
related_docs:
---
 
# **Step-by-Step Guide to Setting Up Kubernetes Cluster**

## **Cluster Details**

- **Control Plane**: `k8s-cp-01` (192.168.20.10)
- **Worker Nodes**:
    - `k8s-w-01` (192.168.20.11)
    - `k8s-w-02` (192.168.20.12)
- **VLAN**: All nodes are on a private VLAN with unrestricted communication.
- **Container Runtime**: `containerd`
- **Networking Add-ons**:
    - Flannel for pod networking (`10.244.0.0/16`)
    - Optional: Calico for network policies.
- **Jump-box**
	- `rogue01` (192.168.10.9) 

---

## **Step 1: Prepare Each Node**

### **1.1: Update and Reboot**

On **all nodes**, update the system and reboot:

```bash
sudo apt update && sudo apt upgrade -y
sudo reboot
```

---

### **1.2: Disable Swap**

On **all nodes**, Kubernetes requires swap to be off:

```bash
sudo swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab
```

---

### **1.3: Load Kernel Modules**

On **all nodes**, enable required kernel modules for Kubernetes networking:

```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter
```

---

### **1.4: Set Kernel Parameters**

On **all nodes**, configure kernel parameters for Kubernetes networking:

```bash
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward = 1
EOF
```

```Shell
sudo sysctl --system
```

---

## **Step 2: Install and Configure containerd**

### **2.1: Install containerd**

On **all nodes**, install the container runtime:

```bash
sudo apt-get install -y containerd
```

---

### **2.2: Configure containerd**

On **all nodes**, create and configure `containerd`’s default configuration file:

```bash
sudo mkdir -p /etc/containerd
sudo containerd config default | sudo tee /etc/containerd/config.toml
```

Restart and enable `containerd`:

```bash
sudo systemctl restart containerd
sudo systemctl enable containerd
```

---

Here’s the updated **Step 3: Install Kubernetes Components** section with the correct information based on your successful steps:

---

## **Step 3: Install Kubernetes Components**

### **3.1: Add Kubernetes Repository**

On **all nodes**, add the Kubernetes APT repository:

1. **Install Required Tools:**
    
```bash
sudo apt-get install -y apt-transport-https curl
```
    
2. **Create the Keyrings Directory (if needed):**
    
```bash
sudo mkdir -p /etc/apt/keyrings
```
    
3. **Download and Add the Public Signing Key:**
    
```bash
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
```
    
4. **Add the Kubernetes Repository:**
    
```bash
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /" | sudo tee /etc/apt/sources.list.d/kubernetes.list
    ```
    

---

### **3.2: Install `kubeadm`, `kubelet`, and `kubectl`**

On **all nodes**, install the Kubernetes tools and mark them on hold:

1. **Update the Package Index:**
    
```bash
sudo apt-get update
```
    
2. **Install Kubernetes Components:**
    
```bash
sudo apt-get install -y kubelet kubeadm kubectl
```
    
3. **Mark Components to Prevent Automatic Updates:**
    
```bash
sudo apt-mark hold kubelet kubeadm kubectl
```
    

---

### **To Update Kubernetes Components Later:**

If you need to update the components at a later date, remove the hold:

```bash
sudo apt-mark unhold kubelet kubeadm kubectl
```

Then, update as needed:

```bash
sudo apt-get update
sudo apt-get upgrade -y kubelet kubeadm kubectl
```

---

## **Step 4: Initialize the Control Plane**

On **k8s-control-01**, initialize the Kubernetes cluster:

```bash
sudo kubeadm init \
  --apiserver-advertise-address=192.168.20.10 \
  --pod-network-cidr=10.244.0.0/16
```

---

### **4.1: Configure kubectl**

On **k8s-control-01**, set up `kubectl` for the current user:

```bash
mkdir -p $HOME/.kube
sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

---

### **4.2: Save the Join Command**

Copy the `kubeadm join` command outputted by the initialization. It will look similar to:

```bash
sudo kubeadm join 192.168.20.10:6443 \
  --token <token> \
  --discovery-token-ca-cert-hash sha256:<hash>
```

This will be used to join Worker Nodes.

---

## **Step 5: Apply Pod Networking**

On **k8s-control-01**, install Flannel for pod networking:

```bash
kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml
```

---

## **Step 6: Join Worker Nodes**

On **k8s-worker-01** and **k8s-worker-02**, run the `kubeadm join` command saved in Step 4.2:

```bash
sudo kubeadm join 192.168.20.10:6443 \
  --token <token> \
  --discovery-token-ca-cert-hash sha256:<hash>
```

---

## **Step 7: Verify the Cluster**

On **k8s-control-01**, verify that all nodes are connected and ready:

```bash
kubectl get nodes
```

Check that pods are running:

```bash
kubectl get pods -A
```

---

## **Step 8: (Optional) Add Calico for Network Policies**

If you want to enforce pod-level network policies, layer Calico on top of Flannel. Run this on **k8s-control-01**:

```bash
kubectl apply -f https://docs.projectcalico.org/manifests/calico-policy-only.yaml
```

> **Note**: Calico for network policies works alongside Flannel, but be cautious if expanding beyond policies to avoid conflicts.

---

## **Step 9: Test the Cluster**

On **k8s-control-01**, deploy a sample application to verify functionality:

```bash
kubectl run nginx --image=nginx --port=80
kubectl expose pod nginx --type=ClusterIP --port=80
```

Retrieve the pod details and test connectivity:

```bash
kubectl get pods -o wide
curl <POD_IP>:80
```

---

## **What’s Next?**

- **Monitoring**: Install Prometheus and Grafana for resource monitoring.
- **Ingress Controller**: Set up an ingress for external access to your services.
- **Persistent Storage**: Add a storage provider for persistent volumes if needed.

---

### **Troubleshooting Tips**

1. **Check Logs**:
    - Control Plane issues: `journalctl -xeu kubelet`.
    - Worker Node join failures: `journalctl -xeu kubelet`.
2. **Network Connectivity**:
    - Test ping between nodes: `ping <other_node_IP>`.
    - Verify pod networking: `kubectl get pods -A -o wide`.

This adjusted guide is designed for clarity and flexibility. Let me know if you need further refinements!