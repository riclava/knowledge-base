---
title: Ubuntu Common Issues and Optimization
type: source
created: 2026-04-15
updated: 2026-04-15
sources: [Ubuntu常见问题与优化.md]
tags: [ubuntu, linux, optimization, apt, sysctl, swap, nfs, systemd-resolved, dns, multipath, vmware, troubleshooting]
---

# Ubuntu Common Issues and Optimization

Practical reference for Ubuntu system optimization, APT package management, and common troubleshooting scenarios including DNS port conflicts and VMware multipath errors.

---

## Source Facts

| Field | Value |
|-------|-------|
| Original file | `raw/OS/Linux/Ubuntu/Ubuntu常见问题与优化.md` |
| Topic | Ubuntu system optimization and troubleshooting |
| Applies to | Ubuntu 18.04+, with legacy notes for 16.04 |
| Key areas | System tuning, APT management, DNS conflicts, VMware compatibility |

---

## Key Points

### System Optimization Overview

The source identifies five main optimization areas:

| Area | Config File | Purpose |
|------|-------------|---------|
| Kernel parameters | `/etc/sysctl.conf` | Network buffers, file handles, TCP connections |
| Filesystem | `/etc/fstab` | Mount options, SSD optimization, swap control |
| Network | `/etc/netplan/*.yaml` | TCP/IP tuning (see [[netplan-configuration-guide]]) |
| Scheduler | `/etc/default/grub` | CPU scheduling policy |
| Memory | `/etc/sysctl.conf` | Memory caching, leak reduction |

### Swap Management

**Temporary disable:**
```bash
swapoff -a
```

**Permanent disable (comment out swap in fstab):**
```bash
sudo sed -i '/swap/s/^/#/' /etc/fstab
```

Use case: Kubernetes nodes, memory-sensitive workloads, or systems with sufficient RAM where swap would degrade performance.

### NFS Auto-Mount

Configure persistent NFS mounts in `/etc/fstab`:

```
192.168.1.100:/data    /mnt/data   nfs   defaults,_netdev   0   0
```

Key options:
- `_netdev`: Wait for network before mounting (critical for NFS)
- `defaults`: Standard mount options

---

## APT Package Management

### Download Without Installing

Useful for preparing offline installation packages:

```bash
# Download package and dependencies
sudo apt-get install --download-only --reinstall -y <package-name>

# Copy downloaded packages
cp /var/cache/apt/archives/*.deb ~/my-packages/
```

### Mirror Configuration

Replace default mirrors with regional alternatives (USTC example for China):

```bash
# Backup original
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

# Replace mirrors
sudo sed -i 's@//.*archive.ubuntu.com@//mirrors.ustc.edu.cn@g' /etc/apt/sources.list
sudo sed -i 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list

# Update index
sudo apt update
```

### Maintenance Commands

```bash
# Standard update
sudo apt update && sudo apt upgrade -y

# Full upgrade (including kernel)
sudo apt dist-upgrade -y

# Cleanup
sudo apt autoremove -y
sudo apt autoclean
```

---

## Troubleshooting

### DNS Port 53 Conflict (systemd-resolved)

**Problem:** Ubuntu 18.04+ enables `systemd-resolved` by default, which binds to port 53 and conflicts with local DNS services (e.g., dnsmasq, Pi-hole, custom DNS servers).

**Symptoms:**
- Port 53 already in use when starting DNS service
- `ss -tunlp | grep :53` shows `systemd-resolve`

**Solution:**

```bash
# Disable DNS stub listener
sudo sed -i 's/#DNSStubListener=yes/DNSStubListener=no/' /etc/systemd/resolved.conf

# Relink resolv.conf to use resolved's upstream config
sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf

# Restart service
sudo systemctl restart systemd-resolved.service

# Verify port is released
ss -tunlp | grep :53
```

**Explanation:**
- `systemd-resolved` provides a local DNS stub listener on 127.0.0.53:53
- `DNSStubListener=no` disables this stub, freeing port 53
- Relinking `/etc/resolv.conf` ensures DNS resolution still works via systemd-resolved's upstream configuration

### Multipath Errors in VMware

**Problem:** VMware virtual machines generate persistent multipath-related errors in syslog.

**Symptoms:**
- Syslog filled with multipath errors
- Errors reference VMware virtual disks

**Solution:**

```bash
# Create multipath blacklist configuration
cat <<EOF | sudo tee /etc/multipath.conf
defaults {
    user_friendly_names yes
}

blacklist {
    device {
        vendor "VMware"
        product "Virtual disk"
    }
}
EOF

# Restart multipath service
sudo /etc/init.d/multipath-tools restart
```

**Explanation:**
- Multipath is designed for physical SAN environments with multiple paths to storage
- VMware virtual disks don't benefit from multipath and generate spurious errors
- Blacklisting VMware devices suppresses these errors

---

## Legacy Reference: Ubuntu 16.04 WiFi

> **Deprecation notice:** This section applies only to Ubuntu 16.04 and earlier using `/etc/network/interfaces`. Ubuntu 17.10+ uses Netplan—see [[netplan-configuration-guide]].

Legacy WiFi configuration via wpa_supplicant:

```bash
# Generate WiFi config
wpa_passphrase "SSID" "PASSWORD" | sudo tee /etc/wpa_supplicant/wpa_supplicant.conf

# Configure interface in /etc/network/interfaces
auto lo
iface lo inet loopback

auto wlan0
iface wlan0 inet dhcp
    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf

# Enable service
sudo systemctl enable wpa_supplicant --now
```

---

## Caveats and Risks

| Topic | Risk | Mitigation |
|-------|------|------------|
| Disabling swap | OOM killer may terminate processes under memory pressure | Ensure sufficient RAM; monitor memory usage |
| Changing resolv.conf symlink | May break DNS if systemd-resolved is stopped | Verify DNS works after change; have console access |
| Multipath blacklist | May hide legitimate multipath issues on physical hardware | Only apply to VMware VMs, not physical servers |
| APT mirror changes | Regional mirrors may have sync delays | Use official mirrors for security-critical updates |

---

## Related Documents

The source references these related files:
- [Netplan 配置指南](./netplan配置指南.md) — network configuration
- [Ubuntu 切换指定版本内核](./Ubuntu切换指定版本内核.md) — kernel management
- [Ubuntu 22.04 升级 OpenSSH](./Ubuntu22.04升级OpenSSH版本到最新.md) — security upgrades
- [基于 Docker 构建 Ubuntu 开发环境](./基于docker构建ubuntu20.04开发环境.md) — containerized development

---

## Related Pages

- [[ubuntu]] — Ubuntu distribution page
- [[linux]] — Linux platform page
- [[netplan-configuration-guide]] — Netplan network configuration reference
- [[network-configuration]] — declarative network configuration concept
- [[ubuntu2204-openssh-upgrade-from-source]] — OpenSSH source upgrade runbook
- [[ubuntu2004-docker-dev-environment-setup]] — Docker development environment setup
- [[file-descriptor-and-tcp-backlog-tuning]] — related system tuning concepts
- [[containerized-development-environment]] — containerized development concept
