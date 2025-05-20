---
title: zfs-monitoring-malony-scripts-for-prom
created: 2025-05-16
type: general
tags:
  - general
  - uiowa
  - zfs
  - monitoring
---

### 1. ZFS Dataset Statistics (`zfs_dataset_exporter.sh`)

```bash
#!/usr/bin/env bash
# ZFS Dataset Statistics for Prometheus
set -eu

PREFIX="zfs"
DATASET="${PREFIX}_dataset"
HOSTNAME=$(hostname -s)
ENVIRONMENT="production"

# Check if dpool01 exists
if ! /usr/sbin/zpool list "dpool01" &>/dev/null; then
  echo "# ERROR: ZFS pool 'dpool01' not found"
  exit 0  # Exit cleanly for Prometheus
fi

# Get datasets from dpool01
mapfile -t datasets < <(/usr/sbin/zfs list -H | awk '/^dpool01/ {print $1}')

echo "# HELP ${DATASET}_info Information about ZFS datasets"
echo "# TYPE ${DATASET}_info gauge"
logger -t zfs_exporter "Starting collection of ZFS dataset metrics"

for d in "${datasets[@]}"; do
  properties=$(/usr/sbin/zfs get all "${d}" -p -H | awk '
    $2 == "compressratio" || 
    $2 == "referenced" || 
    $2 == "written" || 
    $2 == "logicalused" || 
    $2 == "logicalreferenced" || 
    $2 == "used" || 
    $2 == "refcompressratio" || 
    $2 == "usedbysnapshots" || 
    $2 == "available" || 
    $2 == "type" || 
    $2 == "mounted" || 
    $2 == "creation" || 
    $2 == "mountpoint" || 
    $2 == "checksum" || 
    $2 == "compression" || 
    $2 == "readonly" {print $2"|"$3}')
    
  # Parse and export dataset metadata
  type=""
  mountpoint=""
  while IFS= read -r p; do
    IFS="|" read -r name value <<< "${p}"
    
    # Store some values to use as labels
    if [ "$name" == "type" ]; then
      type=$value
    elif [ "$name" == "mountpoint" ]; then
      mountpoint=$value
    elif [ "$name" == "compressratio" ]; then
      # Convert 1.43x to 1.43
      value=${value/%x/}
      echo "${DATASET}_compressratio{dataset=\"$d\",type=\"$type\",mountpoint=\"$mountpoint\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} $value"
    elif [ "$name" == "refcompressratio" ]; then
      value=${value/%x/}
      echo "${DATASET}_refcompressratio{dataset=\"$d\",type=\"$type\",mountpoint=\"$mountpoint\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} $value"
    elif [[ $name == *"mounted"* || $name == *"readonly"* ]]; then
      # Convert yes/no to 1/0
      if [ "$value" == "yes" ]; then
        echo "${DATASET}_${name}{dataset=\"$d\",type=\"$type\",mountpoint=\"$mountpoint\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} 1"
      else
        echo "${DATASET}_${name}{dataset=\"$d\",type=\"$type\",mountpoint=\"$mountpoint\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} 0"
      fi
    elif [[ $name == *"used"* || $name == *"available"* || $name == *"referenced"* || $name == *"logical"* || $name == *"written"* ]]; then
      # These are all byte values
      echo "${DATASET}_${name}_bytes{dataset=\"$d\",type=\"$type\",mountpoint=\"$mountpoint\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} $value"
    fi
  done <<< "$properties"
done

logger -t zfs_exporter "Completed collection of ZFS dataset metrics"
```

### 2. ZFS Quota Exporter (`zfs_quota_exporter.sh`)

```bash
#!/usr/bin/env bash
# ZFS Quota Metrics for Prometheus
set -eu

PREFIX="zfs"
QUOTA="${PREFIX}_quota"
HOSTNAME=$(hostname -s)
ENVIRONMENT="production"

# Check if dpool01 exists
if ! /usr/sbin/zpool list "dpool01" &>/dev/null; then
  echo "# ERROR: ZFS pool 'dpool01' not found"
  exit 0  # Exit cleanly for Prometheus
fi

# Specify datasets to check quotas (optimize for performance)
quota_datasets=(
  "dpool01/home"
  "dpool01/data"
  # Add more as needed
)

echo "# HELP ${QUOTA}_user_used_bytes Space used by user"
echo "# TYPE ${QUOTA}_user_used_bytes gauge"
echo "# HELP ${QUOTA}_user_quota_bytes Space quota for user"
echo "# TYPE ${QUOTA}_user_quota_bytes gauge"

logger -t zfs_exporter "Starting collection of ZFS quota metrics"

for d in "${quota_datasets[@]}"; do
  # User quota metrics
  /usr/sbin/zfs userspace -Hp "${d}" 2>/dev/null | awk '{
    print "'"${QUOTA}"'_user_used_bytes{dataset=\"'"${d}"'\",user=\""$3"\",host=\"'"${HOSTNAME}"'\",env=\"'"${ENVIRONMENT}"'\"} "$4;
    if ($5 != "-") print "'"${QUOTA}"'_user_quota_bytes{dataset=\"'"${d}"'\",user=\""$3"\",host=\"'"${HOSTNAME}"'\",env=\"'"${ENVIRONMENT}"'\"} "$5;
    print "'"${QUOTA}"'_user_obj_used{dataset=\"'"${d}"'\",user=\""$3"\",host=\"'"${HOSTNAME}"'\",env=\"'"${ENVIRONMENT}"'\"} "$6;
    if ($7 != "-") print "'"${QUOTA}"'_user_obj_quota{dataset=\"'"${d}"'\",user=\""$3"\",host=\"'"${HOSTNAME}"'\",env=\"'"${ENVIRONMENT}"'\"} "$7;
  }' 2>/dev/null || true
  
  # Group quota metrics
  /usr/sbin/zfs groupspace -Hp "${d}" 2>/dev/null | awk '{
    print "'"${QUOTA}"'_group_used_bytes{dataset=\"'"${d}"'\",group=\""$3"\",host=\"'"${HOSTNAME}"'\",env=\"'"${ENVIRONMENT}"'\"} "$4;
    if ($5 != "-") print "'"${QUOTA}"'_group_quota_bytes{dataset=\"'"${d}"'\",group=\""$3"\",host=\"'"${HOSTNAME}"'\",env=\"'"${ENVIRONMENT}"'\"} "$5;
    print "'"${QUOTA}"'_group_obj_used{dataset=\"'"${d}"'\",group=\""$3"\",host=\"'"${HOSTNAME}"'\",env=\"'"${ENVIRONMENT}"'\"} "$6;
    if ($7 != "-") print "'"${QUOTA}"'_group_obj_quota{dataset=\"'"${d}"'\",group=\""$3"\",host=\"'"${HOSTNAME}"'\",env=\"'"${ENVIRONMENT}"'\"} "$7;
  }' 2>/dev/null || true
  
  # Project quota metrics
  /usr/sbin/zfs projectspace -Hp "${d}" 2>/dev/null | awk '{
    print "'"${QUOTA}"'_project_used_bytes{dataset=\"'"${d}"'\",projectid=\""$2"\",host=\"'"${HOSTNAME}"'\",env=\"'"${ENVIRONMENT}"'\"} "$3;
    if ($4 != "-") print "'"${QUOTA}"'_project_quota_bytes{dataset=\"'"${d}"'\",projectid=\""$2"\",host=\"'"${HOSTNAME}"'\",env=\"'"${ENVIRONMENT}"'\"} "$4;
    print "'"${QUOTA}"'_project_obj_used{dataset=\"'"${d}"'\",projectid=\""$2"\",host=\"'"${HOSTNAME}"'\",env=\"'"${ENVIRONMENT}"'\"} "$5;
    if ($6 != "-") print "'"${QUOTA}"'_project_obj_quota{dataset=\"'"${d}"'\",projectid=\""$2"\",host=\"'"${HOSTNAME}"'\",env=\"'"${ENVIRONMENT}"'\"} "$6;
  }' 2>/dev/null || true
done

logger -t zfs_exporter "Completed collection of ZFS quota metrics"
```

### 3. ZFS Mount Verification (`zfs_mount_check_exporter.sh`)

```bash
#!/usr/bin/env bash
# ZFS Mount Verification for Prometheus
set -eu

PREFIX="zfs"
MOUNT="${PREFIX}_mount"
HOSTNAME=$(hostname -s)
ENVIRONMENT="production"

# Check if dpool01 exists
if ! /usr/sbin/zpool list "dpool01" &>/dev/null; then
  echo "# ERROR: ZFS pool 'dpool01' not found"
  exit 0  # Exit cleanly for Prometheus
fi

# Specify critical datasets to check
critical_datasets=(
  "dpool01/home"
  "dpool01/data"
  # Add other critical datasets
)

# Define specific check files for datasets
declare -A check_files
check_files["dpool01/home"]=".zfs"  # Hidden ZFS directory
check_files["dpool01/data"]=".zfs"
# Add more dataset-specific check files as needed

echo "# HELP ${MOUNT}_check Verification that ZFS filesystems are properly mounted"
echo "# TYPE ${MOUNT}_check gauge"

logger -t zfs_exporter "Starting collection of ZFS mount check metrics"

for d in "${critical_datasets[@]}"; do
  # Get mountpoint
  mountpoint=$(/usr/sbin/zfs get -H -o value mountpoint "${d}" 2>/dev/null)
  
  # Use dataset-specific check file or default to "."
  check_file="${check_files[$d]:-"."}"
  
  # Check if it's in /proc/mounts
  if grep -q "${d} " /proc/mounts; then
    proc_check=1
  else
    proc_check=0
  fi
  
  # Check if we can stat a file in the mountpoint
  if [ -d "$mountpoint" ] && stat "${mountpoint}/${check_file}" >/dev/null 2>&1; then
    stat_check=1
  else
    stat_check=0
  fi
  
  # Both checks must pass
  if [ $proc_check -eq 1 ] && [ $stat_check -eq 1 ]; then
    result=1
  else
    result=0
    # Log mount failures
    logger -t zfs_exporter "WARNING: Mount check failed for ${d} (proc_check=${proc_check}, stat_check=${stat_check})"
  fi
  
  echo "${MOUNT}_check{dataset=\"${d}\",mountpoint=\"${mountpoint}\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} $result"
done

logger -t zfs_exporter "Completed collection of ZFS mount check metrics"
```

### 4. ZFS Pool Health Exporter (`zfs_pool_health_exporter.sh`)

```bash
#!/usr/bin/env bash
# ZFS Pool Health Status for Prometheus
set -eu

PREFIX="zfs"
HEALTH="${PREFIX}_health"
HOSTNAME=$(hostname -s)
ENVIRONMENT="production"

# Check if dpool01 exists
if ! /usr/sbin/zpool list "dpool01" &>/dev/null; then
  echo "# ERROR: ZFS pool 'dpool01' not found"
  exit 0  # Exit cleanly for Prometheus
fi

# Specifically monitor dpool01
pools=("dpool01")

echo "# HELP ${HEALTH}_status Health status of ZFS pools and devices"
echo "# TYPE ${HEALTH}_status gauge"
echo "# HELP ${HEALTH}_errors Error counts for ZFS pools and devices"
echo "# TYPE ${HEALTH}_errors counter"

logger -t zfs_exporter "Starting collection of ZFS pool health metrics"

for p in "${pools[@]}"; do
  # Temporary file for pool status
  tfile=$(mktemp /tmp/zpool.XXXXXX)
  
  # Get pool status with error counts
  /usr/sbin/zpool status "${p}" -p | sed '1,/NAME/d' | sed -n '/errors/q;p' | sed '/^[[:space:]]*$/d' | awk '{print $1" "$2" "$3" "$4" "$5}' > "${tfile}"
  
  vdev_type="pool"
  while read -r l; do
    IFS=" " read -r name state readerr writeerr chksmerr <<< "${l}"
    
    # Convert state to numeric value for Prometheus
    case ${state} in
      ONLINE) state_num=0 ;;  # 0 = good
      DEGRADED) state_num=1 ;; # 1 = warning
      FAULTED) state_num=2 ;; # 2 = critical
      UNAVAIL) state_num=2 ;; # 2 = critical
      OFFLINE) state_num=1 ;; # 1 = warning
      REMOVED) state_num=1 ;; # 1 = warning
      *) state_num=3 ;;       # 3 = unknown
    esac
    
    # Log non-healthy states
    if [ "$state_num" -ne 0 ]; then
      logger -t zfs_exporter "WARNING: ZFS device ${name} in pool ${p} has state ${state}"
    fi
    
    case "${name}" in
      "${p}")
        # Pool-level metrics
        echo "${HEALTH}_status{pool=\"${p}\",device=\"${name}\",type=\"pool\",vdev_type=\"none\",state=\"${state}\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} ${state_num}"
        echo "${HEALTH}_errors_read{pool=\"${p}\",device=\"${name}\",type=\"pool\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} ${readerr}"
        echo "${HEALTH}_errors_write{pool=\"${p}\",device=\"${name}\",type=\"pool\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} ${writeerr}"
        echo "${HEALTH}_errors_checksum{pool=\"${p}\",device=\"${name}\",type=\"pool\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} ${chksmerr}"
        ;;
      raidz*|mirror*|draid*)
        # VDEV-level metrics
        echo "${HEALTH}_status{pool=\"${p}\",device=\"${name}\",type=\"vdev\",vdev_type=\"${vdev_type}\",state=\"${state}\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} ${state_num}"
        echo "${HEALTH}_errors_read{pool=\"${p}\",device=\"${name}\",type=\"vdev\",vdev_type=\"${vdev_type}\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} ${readerr}"
        echo "${HEALTH}_errors_write{pool=\"${p}\",device=\"${name}\",type=\"vdev\",vdev_type=\"${vdev_type}\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} ${writeerr}"
        echo "${HEALTH}_errors_checksum{pool=\"${p}\",device=\"${name}\",type=\"vdev\",vdev_type=\"${vdev_type}\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} ${chksmerr}"
        ;;
      special|logs|cache|spares)
        vdev_type=${name}
        ;;
      *)
        # Device-level metrics
        echo "${HEALTH}_status{pool=\"${p}\",device=\"${name}\",type=\"drive\",vdev_type=\"${vdev_type}\",state=\"${state}\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} ${state_num}"
        if [ "${vdev_type}" != "spares" ]; then
          echo "${HEALTH}_errors_read{pool=\"${p}\",device=\"${name}\",type=\"drive\",vdev_type=\"${vdev_type}\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} ${readerr}"
          echo "${HEALTH}_errors_write{pool=\"${p}\",device=\"${name}\",type=\"drive\",vdev_type=\"${vdev_type}\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} ${writeerr}"
          echo "${HEALTH}_errors_checksum{pool=\"${p}\",device=\"${name}\",type=\"drive\",vdev_type=\"${vdev_type}\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} ${chksmerr}"
        else
          echo "${HEALTH}_errors_read{pool=\"${p}\",device=\"${name}\",type=\"drive\",vdev_type=\"${vdev_type}\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} 0"
          echo "${HEALTH}_errors_write{pool=\"${p}\",device=\"${name}\",type=\"drive\",vdev_type=\"${vdev_type}\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} 0"
          echo "${HEALTH}_errors_checksum{pool=\"${p}\",device=\"${name}\",type=\"drive\",vdev_type=\"${vdev_type}\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} 0"
        fi
        ;;
    esac
  done < "${tfile}"
  
  # Clean up temporary file
  rm -f "${tfile}"
done

logger -t zfs_exporter "Completed collection of ZFS pool health metrics"
```

### 5. ZFS Pool Capacity Exporter (`zfs_pool_capacity_exporter.sh`)

```bash
#!/usr/bin/env bash
# ZFS Pool Capacity Metrics for Prometheus
set -eu

PREFIX="zfs"
POOL="${PREFIX}_pool"
HOSTNAME=$(hostname -s)
ENVIRONMENT="production"

# Check if dpool01 exists
if ! /usr/sbin/zpool list "dpool01" &>/dev/null; then
  echo "# ERROR: ZFS pool 'dpool01' not found"
  exit 0  # Exit cleanly for Prometheus
fi

# Specifically monitor dpool01
pools=("dpool01")

# Define thresholds for alerting
WARNING_THRESHOLD=80
CRITICAL_THRESHOLD=90

echo "# HELP ${POOL}_size_bytes Total size of the pool"
echo "# TYPE ${POOL}_size_bytes gauge"
echo "# HELP ${POOL}_free_bytes Free space in the pool"
echo "# TYPE ${POOL}_free_bytes gauge"
echo "# HELP ${POOL}_capacity_percent Capacity usage percentage of the pool"
echo "# TYPE ${POOL}_capacity_percent gauge"

logger -t zfs_exporter "Starting collection of ZFS pool capacity metrics"

for p in "${pools[@]}"; do
  # Pool capacity metrics
  pool_info=$(/usr/sbin/zpool list -Hp -o name,size,free,allocated,capacity,dedupratio,fragmentation "${p}")
  IFS=$'\t' read -r name size free allocated capacity dedup frag <<< "${pool_info}"
  
  # Remove % sign from capacity and fragmentation
  capacity=${capacity//%/}
  frag=${frag//%/}
  
  # Remove 'x' from dedup ratio
  dedup=${dedup/%x/}
  
  # Log if capacity exceeds thresholds
  if [ "${capacity%%.*}" -ge "$CRITICAL_THRESHOLD" ]; then
    logger -t zfs_exporter "CRITICAL: ZFS pool ${name} capacity at ${capacity}% (>=${CRITICAL_THRESHOLD}%)"
  elif [ "${capacity%%.*}" -ge "$WARNING_THRESHOLD" ]; then
    logger -t zfs_exporter "WARNING: ZFS pool ${name} capacity at ${capacity}% (>=${WARNING_THRESHOLD}%)"
  fi
  
  echo "${POOL}_size_bytes{pool=\"${name}\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} ${size}"
  echo "${POOL}_free_bytes{pool=\"${name}\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} ${free}"
  echo "${POOL}_allocated_bytes{pool=\"${name}\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} ${allocated}"
  echo "${POOL}_capacity_percent{pool=\"${name}\",warning=\"${WARNING_THRESHOLD}\",critical=\"${CRITICAL_THRESHOLD}\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} ${capacity}"
  echo "${POOL}_dedupratio{pool=\"${name}\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} ${dedup}"
  echo "${POOL}_fragmentation_percent{pool=\"${name}\",host=\"${HOSTNAME}\",env=\"${ENVIRONMENT}\"} ${frag}"
done

logger -t zfs_exporter "Completed collection of ZFS pool capacity metrics"
```

## Implementation Plan

### Step 1: Prerequisites Installation

```bash
# Install needed packages
sudo apt-get update
sudo apt-get install -y moreutils  # For the 'sponge' command
```

### Step 2: Create Scripts Directory

```bash
# Create directory for the scripts
sudo mkdir -p /usr/share/prometheus-node-exporter-collectors
```

### Step 3: Deploy Scripts

```bash
# Copy scripts to collectors directory
sudo tee /usr/share/prometheus-node-exporter-collectors/zfs_dataset.sh > /dev/null << 'EOF'
#!/usr/bin/env bash
# [Insert zfs_dataset_exporter.sh content here]
EOF

sudo tee /usr/share/prometheus-node-exporter-collectors/zfs_quota.sh > /dev/null << 'EOF'
#!/usr/bin/env bash
# [Insert zfs_quota_exporter.sh content here]
EOF

sudo tee /usr/share/prometheus-node-exporter-collectors/zfs_mount.sh > /dev/null << 'EOF'
#!/usr/bin/env bash
# [Insert zfs_mount_check_exporter.sh content here]
EOF

sudo tee /usr/share/prometheus-node-exporter-collectors/zfs_pool_health.sh > /dev/null << 'EOF'
#!/usr/bin/env bash
# [Insert zfs_pool_health_exporter.sh content here]
EOF

sudo tee /usr/share/prometheus-node-exporter-collectors/zfs_pool_capacity.sh > /dev/null << 'EOF'
#!/usr/bin/env bash
# [Insert zfs_pool_capacity_exporter.sh content here]
EOF

# Make scripts executable
sudo chmod +x /usr/share/prometheus-node-exporter-collectors/zfs_*.sh
```

### Step 4: Create Metrics Directory

```bash
# Create directory for Prometheus Node Exporter
sudo mkdir -p /var/lib/prometheus/node-exporter
sudo chown -R prometheus:prometheus /var/lib/prometheus/node-exporter
```

### Step 5: Create SystemD Service Files

```bash
# Dataset service
sudo tee /lib/systemd/system/prometheus-node-exporter-zfs-dataset.service > /dev/null << 'EOF'
[Unit]
Description=Collect ZFS Dataset metrics for prometheus-node-exporter
After=zfs-mount.service
Requires=zfs.service

[Service]
Type=oneshot
Environment=TMPDIR=/var/lib/prometheus/node-exporter
ExecStart=/bin/bash -c "/usr/share/prometheus-node-exporter-collectors/zfs_dataset.sh | sponge /var/lib/prometheus/node-exporter/zfs_dataset.prom"
EOF

# Quota service
sudo tee /lib/systemd/system/prometheus-node-exporter-zfs-quota.service > /dev/null << 'EOF'
[Unit]
Description=Collect ZFS Quota metrics for prometheus-node-exporter
After=zfs-mount.service
Requires=zfs.service

[Service]
Type=oneshot
Environment=TMPDIR=/var/lib/prometheus/node-exporter
ExecStart=/bin/bash -c "/usr/share/prometheus-node-exporter-collectors/zfs_quota.sh | sponge /var/lib/prometheus/node-exporter/zfs_quota.prom"
EOF

# Mount check service
sudo tee /lib/systemd/system/prometheus-node-exporter-zfs-mount.service > /dev/null << 'EOF'
[Unit]
Description=Collect ZFS Mount check metrics for prometheus-node-exporter
After=zfs-mount.service
Requires=zfs.service

[Service]
Type=oneshot
Environment=TMPDIR=/var/lib/prometheus/node-exporter
ExecStart=/bin/bash -c "/usr/share/prometheus-node-exporter-collectors/zfs_mount.sh | sponge /var/lib/prometheus/node-exporter/zfs_mount.prom"
EOF

# Pool health service
sudo tee /lib/systemd/system/prometheus-node-exporter-zfs-pool-health.service > /dev/null << 'EOF'
[Unit]
Description=Collect ZFS Pool Health metrics for prometheus-node-exporter
After=zfs.service
Requires=zfs.service

[Service]
Type=oneshot
Environment=TMPDIR=/var/lib/prometheus/node-exporter
ExecStart=/bin/bash -c "/usr/share/prometheus-node-exporter-collectors/zfs_pool_health.sh | sponge /var/lib/prometheus/node-exporter/zfs_pool_health.prom"
EOF

# Pool capacity service
sudo tee /lib/systemd/system/prometheus-node-exporter-zfs-pool-capacity.service > /dev/null << 'EOF'
[Unit]
Description=Collect ZFS Pool Capacity metrics for prometheus-node-exporter
After=zfs.service
Requires=zfs.service

[Service]
Type=oneshot
Environment=TMPDIR=/var/lib/prometheus/node-exporter
ExecStart=/bin/bash -c "/usr/share/prometheus-node-exporter-collectors/zfs_pool_capacity.sh | sponge /var/lib/prometheus/node-exporter/zfs_pool_capacity.prom"
EOF
```

### Step 6: Create SystemD Timer Files

```bash
# Dataset timer
sudo tee /lib/systemd/system/prometheus-node-exporter-zfs-dataset.timer > /dev/null << 'EOF'
[Unit]
Description=Run ZFS Dataset metrics collection every 5 minutes
ConditionPathExists=/usr/sbin/zfs

[Timer]
OnBootSec=0
OnUnitActiveSec=5min

[Install]
WantedBy=timers.target
EOF

# Quota timer (less frequent due to potential performance impact)
sudo tee /lib/systemd/system/prometheus-node-exporter-zfs-quota.timer > /dev/null << 'EOF'
[Unit]
Description=Run ZFS Quota metrics collection every 30 minutes
ConditionPathExists=/usr/sbin/zfs

[Timer]
OnBootSec=0
OnUnitActiveSec=30min

[Install]
WantedBy=timers.target
EOF

# Mount check timer (more frequent for critical monitoring)
sudo tee /lib/systemd/system/prometheus-node-exporter-zfs-mount.timer > /dev/null << 'EOF'
[Unit]
Description=Run ZFS Mount check metrics collection every 1 minute
ConditionPathExists=/usr/sbin/zfs

[Timer]
OnBootSec=0
OnUnitActiveSec=1min

[Install]
WantedBy=timers.target
EOF

# Pool health timer
sudo tee /lib/systemd/system/prometheus-node-exporter-zfs-pool-health.timer > /dev/null << 'EOF'
[Unit]
Description=Run ZFS Pool Health metrics collection every 2 minutes
ConditionPathExists=/usr/sbin/zfs
ConditionPathExists=/usr/sbin/zpool

[Timer]
OnBootSec=0
OnUnitActiveSec=2min

[Install]
WantedBy=timers.target
EOF

# Pool capacity timer
sudo tee /lib/systemd/system/prometheus-node-exporter-zfs-pool-capacity.timer > /dev/null << 'EOF'
[Unit]
Description=Run ZFS Pool Capacity metrics collection every 5 minutes
ConditionPathExists=/usr/sbin/zpool

[Timer]
OnBootSec=0
OnUnitActiveSec=5min

[Install]
WantedBy=timers.target
EOF
```

### Step 7: Enable and Start the Timers

```bash
# Reload systemd
sudo systemctl daemon-reload

# Enable timers
sudo systemctl enable prometheus-node-exporter-zfs-dataset.timer
sudo systemctl enable prometheus-node-exporter-zfs-quota.timer
sudo systemctl enable prometheus-node-exporter-zfs-mount.timer
sudo systemctl enable prometheus-node-exporter-zfs-pool-health.timer
sudo systemctl enable prometheus-node-exporter-zfs-pool-capacity.timer

# Start timers
sudo systemctl start prometheus-node-exporter-zfs-dataset.timer
sudo systemctl start prometheus-node-exporter-zfs-quota.timer
sudo systemctl start prometheus-node-exporter-zfs-mount.timer
sudo systemctl start prometheus-node-exporter-zfs-pool-health.timer
sudo systemctl start prometheus-node-exporter-zfs-pool-capacity.timer
```

### Step 8: Verify Node Exporter Configuration

```bash
# Check if Node Exporter is configured for textfile collector
grep textfile /etc/default/prometheus-node-exporter || grep textfile /lib/systemd/system/prometheus-node-exporter.service

# If not configured, add the flag
sudo sed -i 's/ARGS=""/ARGS="--collector.textfile.directory=\/var\/lib\/prometheus\/node-exporter"/' /etc/default/prometheus-node-exporter

# Restart Node Exporter
sudo systemctl restart prometheus-node-exporter
```

### Step 9: Test the Setup

```bash
# Run each script manually once
sudo /usr/share/prometheus-node-exporter-collectors/zfs_dataset.sh > /tmp/test_dataset.prom
sudo /usr/share/prometheus-node-exporter-collectors/zfs_quota.sh > /tmp/test_quota.prom
sudo /usr/share/prometheus-node-exporter-collectors/zfs_mount.sh > /tmp/test_mount.prom
sudo /usr/share/prometheus-node-exporter-collectors/zfs_pool_health.sh > /tmp/test_health.prom
sudo /usr/share/prometheus-node-exporter-collectors/zfs_pool_capacity.sh > /tmp/test_capacity.prom

# Check output for errors
cat /tmp/test_*.prom

# Check if services run successfully
sudo systemctl start prometheus-node-exporter-zfs-dataset.service
sudo systemctl status prometheus-node-exporter-zfs-dataset.service

# Verify metrics files are created
ls -la /var/lib/prometheus/node-exporter/
```

### Step 10: Setup Basic Grafana Dashboard

