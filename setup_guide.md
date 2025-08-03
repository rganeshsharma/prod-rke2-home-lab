# Complete Setup Guide

## Prerequisites

1. **Install Vagrant:**
   ```bash
   brew install vagrant
   ```

2. **Install VMware Fusion:**
   - Download from: https://www.vmware.com/products/fusion.html
   - Purchase and activate license

3. **Install Vagrant VMware Plugin:**
   ```bash
   vagrant plugin install vagrant-vmware-desktop
   ```

## Setup Steps

### 1. Create Project (1 minute)
```bash
mkdir rke2-cluster
cd rke2-cluster
```

### 2. Create Files (2 minutes)

**Create `Vagrantfile`:**
```bash
# Copy content from "Vagrantfile - Ubuntu VMs Only" artifact above
# Save as: Vagrantfile (no extension)
```

**Create `MANUAL_COMMANDS.md`:**
```bash
# Copy content from "Manual RKE2 Setup Commands" artifact above  
# Save as: MANUAL_COMMANDS.md
```

**Create `README.md`:**
```bash
# Copy content from "Project Structure & Setup" artifact above
# Save as: README.md
```

### 3. Start VMs (5 minutes)
```bash
vagrant up
```

### 4. Install RKE2 Manually (15-20 minutes)

Open `MANUAL_COMMANDS.md` and follow the step-by-step commands:

- **Phase 2:** Prepare all nodes (run on each VM)
- **Phase 3:** Install RKE2 master
- **Phase 4:** Configure kubectl  
- **Phase 5:** Install RKE2 workers
- **Phase 6:** Verify cluster
- **Phase 7:** Install Longhorn storage
- **Phase 8:** Access Longhorn UI
- **Phase 9:** Final verification

## File Structure

```
rke2-cluster/
├── Vagrantfile              # VM configuration
├── MANUAL_COMMANDS.md       # Step-by-step commands
├── README.md                # Project documentation
└── .vagrant/                # Vagrant state (auto-created)
```

## Quick Reference

### VM Access
```bash
vagrant ssh master           # SSH to master node
vagrant ssh rke2-worker-1    # SSH to worker 1  
vagrant ssh rke2-worker-2    # SSH to worker 2
```

### VM Management
```bash
vagrant up                   # Start all VMs
vagrant halt                 # Stop all VMs
vagrant status               # Check status
vagrant destroy -f           # Delete all VMs
```

### Cluster Info
- **Master:** 192.168.100.10 (6 vCPU, 24GB RAM)
- **Worker-1:** 192.168.100.11 (4 vCPU, 32GB RAM, 500GB disk)
- **Worker-2:** 192.168.100.12 (4 vCPU, 32GB RAM, 500GB disk)
- **Storage:** ~300GB usable with Longhorn (2 replicas)

## Workflow Summary

1. **Create project** and files
2. **Start VMs:** `vagrant up`
3. **Follow manual commands** to install RKE2
4. **Access cluster:** `vagrant ssh master` then `kubectl get nodes`
5. **Stop when done:** `vagrant halt`
6. **Start again:** `vagrant up` (VMs persist state)

**Total Time:** ~25 minutes for complete setup

This gives you full control over each installation step while automating the tedious VM creation process!