---
title: Ubuntu
type: product
created: 2026-04-15
updated: 2026-04-15
sources: [netplan配置指南.md, 基于docker构建ubuntu20.04开发环境.md, Ubuntu22.04升级OpenSSH版本到最新.md, Ubuntu常见问题与优化.md, Ubuntu切换指定版本内核.md]
tags: [linux, ubuntu, debian, apt, netplan, systemd-networkd, networkmanager, openssh, source-build, systemd-resolved, dns, swap, nfs, optimization, kernel, grub, boot-management]
---

# Ubuntu

Debian-based Linux distribution widely used for servers, desktops, and containerized development environments, featuring APT package management, Netplan-based network configuration, and source-build upgrade paths for security components.

---

## Overview

Ubuntu is a popular Linux distribution derived from Debian, known for:
- Regular release cadence (LTS every 2 years, interim releases every 6 months)
- APT/dpkg package management
- Netplan as the default network configuration layer (since 17.10)
- Strong container and cloud support

---

## Key Characteristics

| Aspect | Details |
|--------|---------|
| Package manager | APT (`apt`, `apt-get`, `dpkg`) |
| Network configuration | Netplan (YAML-based, backends: `systemd-networkd` or `NetworkManager`) |
| Init system | systemd |
| Default shell | Bash |
| LTS support | 5 years standard, 10 years with Ubuntu Pro |

---

## Network Configuration

Since Ubuntu 17.10, network configuration uses **Netplan**:

- Configuration files: `/etc/netplan/*.yaml`
- Backends: `systemd-networkd` (server) or `NetworkManager` (desktop)
- Key commands:
  - `netplan try` — test with auto-rollback
  - `netplan apply` — apply configuration
  - `netplan generate` — generate backend configs

See [[netplan-configuration-guide]] for comprehensive configuration examples.

---

## Kernel Management

Ubuntu supports installing and switching between multiple kernel versions via APT.

### Check Current Kernel

```bash
uname -r
```

### Search Available Kernels

```bash
apt search linux-image-5.15
```

### Install Specific Kernel Version

```bash
apt install -y linux-image-5.15.0-117-generic
apt install -y linux-headers-5.15.0-117-generic
apt install -y linux-modules-5.15.0-117-generic
apt install -y linux-modules-extra-5.15.0-117-generic
```

### Switch Default Boot Kernel

1. Find the exact menu entry name:
   ```bash
   grep menuentry /boot/grub/grub.cfg
   ```

2. Edit `/etc/default/grub`:
   ```bash
   GRUB_DEFAULT="Advanced options for Ubuntu>Ubuntu, with Linux 5.15.0-117-generic"
   ```

3. Apply changes:
   ```bash
   update-grub
   reboot
   ```

4. Verify after reboot:
   ```bash
   uname -r
   ```

See [[ubuntu-kernel-version-switching]] for detailed runbook.

---

## Package Management

### APT Mirror Configuration

Default sources list: `/etc/apt/sources.list`

For faster downloads in China, replace with regional mirrors:
```bash
sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
apt update
```

### Common Operations

```bash
apt update                    # Refresh package index
apt upgrade                   # Upgrade installed packages
apt install <package>         # Install package
apt remove <package>          # Remove package
apt search <keyword>          # Search packages
apt show <package>            # Show package details
```

### Download Without Installing

Useful for preparing offline installation packages:

```bash
# Download package and dependencies
sudo apt-get install --download-only --reinstall -y <package-name>

# Copy downloaded packages
cp /var/cache/apt/archives/*.deb ~/my-packages/
```

### Maintenance Commands

```bash
# Full upgrade (including kernel)
sudo apt dist-upgrade -y

# Cleanup unused packages
sudo apt autoremove -y
sudo apt autoclean
```

---

## Containerized Development

Ubuntu images are commonly used as base for development containers:

```bash
# Run Ubuntu 20.04 container with keep-alive
docker run -d --name dev-env ubuntu:20.04 sleep infinity

# Enter container
docker exec -it dev-env bash
```

See [[ubuntu2004-docker-dev-environment-setup]] for detailed setup.

---

## OpenSSH Source Upgrade

When system-provided OpenSSH versions are insufficient, Ubuntu supports source-built upgrades:

- Build dependencies via APT: `build-essential`, `libssl-dev`, `libpam0g-dev`, `libselinux1-dev`
- Uses system OpenSSL (typically recent enough, unlike CentOS 7)
- In-place binary replacement with `--prefix=/usr`
- Existing systemd service unit preserved

Key safety considerations:
- Keep current terminal session open during upgrade
- Use `screen` or `tmux` for session persistence
- Backup `/etc/ssh/sshd_config` before changes

See [[ubuntu2204-openssh-upgrade-from-source]] for detailed runbook.

---

## System Optimization

### Swap Management

Disable swap for Kubernetes nodes or memory-sensitive workloads:

```bash
# Temporary disable
swapoff -a

# Permanent disable (comment out swap in fstab)
sudo sed -i '/swap/s/^/#/' /etc/fstab
```

### NFS Auto-Mount

Configure persistent NFS mounts in `/etc/fstab`:

```
192.168.1.100:/data    /mnt/data   nfs   defaults,_netdev   0   0
```

Key option: `_netdev` ensures network is available before mounting.

### Optimization Areas

| Area | Config File | Purpose |
|------|-------------|---------|
| Kernel parameters | `/etc/sysctl.conf` | Network buffers, file handles |
| Filesystem | `/etc/fstab` | Mount options, swap control |
| Network | `/etc/netplan/*.yaml` | TCP/IP tuning |

See [[ubuntu-common-issues-and-optimization]] for detailed optimization guidance.

---

## Common Troubleshooting

### DNS Port 53 Conflict

Ubuntu 18.04+ runs `systemd-resolved` which binds to port 53, conflicting with local DNS services:

```bash
# Disable DNS stub listener
sudo sed -i 's/#DNSStubListener=yes/DNSStubListener=no/' /etc/systemd/resolved.conf
sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf
sudo systemctl restart systemd-resolved.service
```

See [[systemd-resolved-dns-management]] for detailed explanation.

### VMware Multipath Errors

Suppress spurious multipath errors in VMware VMs:

```bash
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
sudo /etc/init.d/multipath-tools restart
```

---

## Version History (LTS)

| Version | Codename | Release | End of Standard Support |
|---------|----------|---------|------------------------|
| 24.04 | Noble Numbat | Apr 2024 | Apr 2029 |
| 22.04 | Jammy Jellyfish | Apr 2022 | Apr 2027 |
| 20.04 | Focal Fossa | Apr 2020 | Apr 2025 |
| 18.04 | Bionic Beaver | Apr 2018 | Apr 2023 |

---

## Differences from CentOS/RHEL

| Aspect | Ubuntu | CentOS/RHEL |
|--------|--------|-------------|
| Package format | `.deb` | `.rpm` |
| Package manager | APT | YUM/DNF |
| Network config | Netplan | `nmcli`, `ifcfg-*` files |
| Firewall | `ufw` (wrapper for iptables/nftables) | `firewalld` |
| SELinux | AppArmor by default | SELinux by default |

---

## Related Pages

- [[linux]] — Linux platform page
- [[netplan-configuration-guide]] — Netplan configuration source summary
- [[ubuntu-common-issues-and-optimization]] — Ubuntu troubleshooting and optimization source summary
- [[ubuntu-kernel-version-switching]] — Ubuntu kernel version switching runbook
- [[network-configuration]] — declarative network configuration concept
- [[systemd-resolved-dns-management]] — DNS resolver management concept
- [[ubuntu2004-docker-dev-environment-setup]] — Docker-based Ubuntu dev environment
- [[ubuntu2204-openssh-upgrade-from-source]] — OpenSSH source upgrade runbook
- [[containerized-development-environment]] — containerized development concept
- [[docker]] — container runtime page
- [[openssh]] — OpenSSH product page
- [[source-built-package-replacement]] — source-compiled software replacement concept
- [[kernel-upgrade-and-boot-management]] — kernel upgrade and boot management concept

