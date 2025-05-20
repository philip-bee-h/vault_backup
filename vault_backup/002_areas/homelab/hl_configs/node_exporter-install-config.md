---
title: node_exporter-install-config
created: 2024-11-17
type: process
system: 
tags:
  - process
  - procedure
  - uiowa
related_docs:
---
**Modified:**

### **1. Install Node Exporter**
1. **Download Node Exporter**
```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.8.2/node_exporter-1.8.2.linux-amd64.tar.gz
```

2. **Extract the Archive**
```bash
tar -xvzf node_exporter-1.8.2.linux-amd64.tar.gz
cd node_exporter-1.8.2.linux-amd64
```

3. **Move the Binary**
```bash
sudo mv node_exporter /usr/local/bin/
```

---

### **2. Run Node Exporter as a Service**
1. **Create a Systemd Service File**
```bash
sudo nano /etc/systemd/system/node_exporter.service
```

2. **Add the Following Content**

```yaml
   [Unit]
   Description=Prometheus Node Exporter
   After=network.target

   [Service]
   User=your-username  # Replace 'your-username' with your actual username
   ExecStart=/usr/local/bin/node_exporter
   Restart=always

   [Install]
   WantedBy=multi-user.target
```
   

3. **Enable and Start the Service**
```bash
sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
```

4. **Check Status**
```bash
sudo systemctl status node_exporter
```

---

### **3. Verify Node Exporter**
1. Open your browser and visit:
   
```web
http://<server-ip>:9100/metrics
```
   Replace `<server-ip>` with the IP address of the server where Node Exporter is running. You should see a long list of metrics.

---

### **4. Add Node Exporter to Prometheus**
1. **Edit `prometheus.yaml` on your Prometheus Server**
   Open your Prometheus configuration file:
```bash
nano /path/to/prometheus.yaml
```

2. **Add the Node Exporter Target**
   Under `scrape_configs`, add:
```yaml
scrape_configs:
 - job_name: 'node_exporter'
   static_configs:
	 - targets: ['<server-ip>:9100', '<server-ip>:9100']  # Replace <server-ip> with the actual IP
```

3. **Reload Prometheus**
   If you enabled the `--web.enable-lifecycle` flag, reload Prometheus:
```bash
curl -X POST http://localhost:9090/-/reload
```
   Otherwise, restart the Prometheus container:
```bash
docker restart prometheus
```

---

### **5. Verify in Prometheus**
1. Go to the Prometheus UI:

```
http://<prometheus-server-ip>:9090
```


2. Navigate to **Status > Targets**:

```web
http://<prometheus-server-ip>:9090/targets
```

   - Confirm that the `node_exporter` target is listed and its status is `UP`.

---

### **Summary**
- **Install Node Exporter**: Download, extract, move binary, and run as a service.
- **Verify Node Exporter**: Visit `http://<server-ip>:9100/metrics`.
- **Configure Prometheus**: Add the server IP to `prometheus.yaml` and reload Prometheus.
- **Check Targets**: Verify in the Prometheus UI that Node Exporter is being scraped.

Let me know if you need help with any step!