# 🎥 YouTube Tutorial: <Topic>

**Video Link:** [Watch on YouTube](https://youtube.com/yourchannel)

## 📂 Project Overview
Deploy a Production ready RKE2 on your Home Lab (Windows, Linux or Mac)

## 🛠️ Tech Stack
- RKE2 (Rancher Kubernetes Engine v2) – Lightweight, secure Kubernetes distribution
- VMware Fusion / VirtualBox – Virtual machine hypervisor for running Linux VMs on macOS/Windows
- Ubuntu 22.04 LTS – VM operating system (minimal install)
-  Ubuntu 24.04 LTS - use only config.vm.box = "bento/ubuntu-24.04" on MacBook
- Vagrant – VM provisioning and automation
- Shell Script / Ansible – Cluster bootstrapping and configuration
- Calico – CNI plugin for Kubernetes networking
- NFS / Local Path Provisioner – For persistent volume provisioning
- K9s – Terminal UI to interact with Kubernetes cluster
- Helm / Kustomize – Kubernetes manifest management
- Traefik / NGINX Ingress Controller – Ingress management
- Longhorn (Optional) – Cloud-native distributed block storage
- Rancher (Optional) – UI-based Kubernetes cluster manager 

**Check the Setup.md if for MacOS Specific Vagrant setup**

## 📦 Setup
# VMware Fusion VM Setup for RKE2 + Longhorn

## VM Specifications

### Master Node VM
```
Name: rke2-master
CPU: 6 vCPU
RAM: 24GB
Disk: 80GB (thin provisioned)
Network: NAT or Bridged
OS: Ubuntu 22.04 LTS Server
```

### Worker Node VMs
```
Name: rke2-worker-1, rke2-worker-2
CPU: 4 vCPU each
RAM: 32GB each  
Disk: 500GB each (thin provisioned)
Network: NAT or Bridged (same network as master)
OS: Ubuntu 22.04 LTS Server
```

## VMware Fusion Disk Settings

### ⚠️ Important Disk Configuration:

**Use Thin Provisioning:**
- ✅ Enable "Allocate disk space dynamically" 
- ✅ Split disk into multiple files (better performance)
- Start with actual usage ~50GB, grow as needed

**Storage Performance:**
- Place VMs on SSD if possible
- Consider separate disk for worker nodes if you have multiple drives

## Network Configuration

### Option 1: NAT Network (Easier)
```
Master:   192.168.x.x (auto-assigned)
Worker-1: 192.168.x.x (auto-assigned)  
Worker-2: 192.168.x.x (auto-assigned)
```

### Option 2: Bridged Network (Better for external access)
- Configure static IPs for each node
- Better for accessing services from host

## Post-VM Creation Steps

### 1. Ubuntu Installation Notes:
- **Minimal server installation** (no GUI needed)
- Enable SSH server during installation
- Create user with sudo privileges
- Set static IPs or note DHCP assignments

### 2. Pre-RKE2 VM Optimizations:
```bash
# On each VM after Ubuntu installation:

# Update system
sudo apt update && sudo apt upgrade -y

# Install required packages
sudo apt install -y curl nfs-common open-iscsi

# Enable iSCSI for Longhorn
sudo systemctl enable --now iscsid

# Disable swap (required for Kubernetes)
sudo swapoff -a
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab

# Configure VM for better performance
echo 'vm.swappiness=1' | sudo tee -a /etc/sysctl.conf
```

### 3. VM Snapshot Strategy:
1. **Base Snapshot:** After Ubuntu install + basic config
2. **Pre-RKE2 Snapshot:** After all prerequisites installed  
3. **Post-RKE2 Snapshot:** After successful cluster setup

## Longhorn Storage Optimization

### Best Practices for VMware Fusion:

**Don't use root disk for Longhorn storage:**
```bash
# After RKE2 installation, verify Longhorn disk usage:
kubectl get nodes.longhorn.io -o yaml

# Check that Longhorn is using /var/lib/longhorn on root disk
# This is OK for testing, but for production consider additional disks
```

**Monitor storage usage:**
```bash
# Check available space
df -h /var/lib/longhorn

# Monitor Longhorn storage allocation
kubectl get volumes.longhorn.io
```

## Resource Allocation Tips

### Host System Requirements:
```
Your Mac should have:
- 32GB+ RAM (to comfortably run 3 VMs with 88GB total)
- 100GB+ free disk space (for thin provisioned growth)
- Recent MacBook Pro/Mac Studio recommended
```

### VM Resource Tuning:
- **Start conservative:** Use minimum specs first
- **Monitor performance:** Use `htop`, `iostat` during testing
- **Scale up gradually:** Add RAM/CPU if needed

## Network Performance

### For Longhorn distributed storage:
- **Enable jumbo frames** if your network supports it
- **Use wired connection** when possible for stability
- **Consider dedicated storage network** for production

## Backup Strategy

### VM-Level Backups:
- Regular VMware snapshots before major changes
- Export VMs periodically for disaster recovery

### Longhorn-Level Backups:
- Configure Longhorn backup to external storage
- Test restore procedures regularly

## Troubleshooting Common Issues

### VM Performance:
```bash
# Check if virtualization extensions are enabled
grep -E '(vmx|svm)' /proc/cpuinfo

# Monitor disk I/O
sudo iotop

# Check memory usage
free -h
```

### Storage Issues:
```bash
# Verify disk space
df -h

# Check Longhorn component health
kubectl get pods -n longhorn-system

# Monitor Longhorn storage usage
kubectl get volumes.longhorn.io -A
```

This configuration will give you a robust **PRODUCTION** ready environment

**What You Get**

✅ 1 Master Node: 6 vCPU, 24GB RAM, 80GB disk

✅ 2 Worker Nodes: 4 vCPU, 32GB RAM, 500GB disk each

✅ ~300GB usable cluster storage with Longhorn

✅ Default CNI: Canal (Calico + Flannel)

✅ High Availability: 2 storage replicas across workers

✅ Production Ready: Optimized configurations included


**Advantages of This Vagrant Setup**

🚀 Reproducible: vagrant destroy && vagrant up rebuilds everything

🛠️ Automated: No manual token copying or configuration

📦 Isolated: Runs in VMs, doesn't affect your host system

🔄 Persistent: VM state survives reboots

⚡ Fast: Thin provisioning means efficient disk usage

📊 Monitoring Ready: Includes resource monitoring tools


