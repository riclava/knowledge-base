---
title: Ubuntu kernel version switching
type: source
created: 2026-04-15
updated: 2026-04-15
sources: [Ubuntu切换指定版本内核.md]
tags: [source, ubuntu, linux, kernel, grub, apt, boot-management]
---

# Ubuntu Kernel Version Switching

Concise Ubuntu runbook for installing a specific kernel version via APT, switching the GRUB default boot entry, and validating the rebooted host.

---

## Source Facts

| Field | Value |
|-------|-------|
| Original file | `raw/OS/Linux/Ubuntu/Ubuntu切换指定版本内核.md` |
| Language | Chinese |
| Scope | Ubuntu kernel version selection and boot management |
| Target kernel | 5.15.0-117-generic (example) |

---

## Summary

This source provides a step-by-step procedure for switching an Ubuntu system to a specific kernel version. Unlike CentOS where ELRepo provides alternative kernel branches, Ubuntu uses APT to install kernel packages from official repositories. The workflow covers:

1. **Check current kernel** — `uname -r`
2. **Search for desired kernel** — `apt search linux-image-<version>`
3. **Install kernel packages** — image, headers, modules, and modules-extra
4. **Configure GRUB default** — edit `/etc/default/grub` with full menu entry path
5. **Apply GRUB changes** — `update-grub`
6. **Reboot and verify** — confirm with `uname -r`

---

## Key Commands

### Check Current Kernel

```bash
uname -r
```

### Search Available Kernels

```bash
apt search linux-image-5.15
# Example output: linux-image-5.15.0-117-generic
```

### Install Kernel Packages

```bash
apt install -y linux-image-5.15.0-117-generic
apt install -y linux-headers-5.15.0-117-generic
apt install -y linux-modules-5.15.0-117-generic
apt install -y linux-modules-extra-5.15.0-117-generic

# Fix dependencies if needed
# apt --fix-broken install

# Verify installation
# dpkg -l | grep 5.15.0-117-generic
```

### Find GRUB Menu Entry

```bash
grep menuentry /boot/grub/grub.cfg
# Look for: Ubuntu, with Linux 5.15.0-117-generic
```

### Configure GRUB Default

Edit `/etc/default/grub`:

```bash
# Change from:
# GRUB_DEFAULT=0

# To (using full submenu path):
GRUB_DEFAULT="Advanced options for Ubuntu>Ubuntu, with Linux 5.15.0-117-generic"
```

### Apply and Reboot

```bash
update-grub
reboot
```

### Verify After Reboot

```bash
uname -r
# Should show: 5.15.0-117-generic
```

---

## Ubuntu vs CentOS Kernel Management

| Aspect | Ubuntu | CentOS 7 |
|--------|--------|----------|
| Package manager | APT | YUM |
| Kernel search | `apt search linux-image-*` | `yum list available kernel*` |
| Kernel packages | `linux-image-*`, `linux-headers-*`, `linux-modules-*` | `kernel`, `kernel-tools`, `kernel-headers` |
| Third-party kernels | PPA or manual download | ELRepo (`kernel-lt`, `kernel-ml`) |
| GRUB config file | `/etc/default/grub` | `/etc/default/grub` |
| GRUB update command | `update-grub` | `grub2-mkconfig -o /boot/grub2/grub.cfg` |
| Default entry format | `"Advanced options for Ubuntu>Ubuntu, with Linux X.Y.Z"` | Numeric index or `grub2-set-default` |

---

## GRUB Default Entry Syntax

Ubuntu uses a submenu structure for kernel selection:

- **Top-level menu**: "Ubuntu" (boots latest kernel)
- **Submenu**: "Advanced options for Ubuntu" (lists all installed kernels)

To boot a specific kernel by default, use the full path format:

```
GRUB_DEFAULT="Advanced options for Ubuntu>Ubuntu, with Linux 5.15.0-117-generic"
```

This differs from CentOS where numeric indices or `grub2-set-default` are commonly used.

---

## Documentation Risks

| Risk | Mitigation |
|------|------------|
| Kernel version may not exist in current repos | Search with `apt search` before attempting install |
| GRUB entry name must match exactly | Copy from `grep menuentry` output |
| Old kernels accumulate | Use `apt autoremove` to clean up unused kernels |
| Dependency issues during install | Run `apt --fix-broken install` if needed |

---

## Use Cases

- **Downgrade for compatibility** — Roll back to an older kernel when newer versions cause driver or application issues
- **Pin to LTS kernel** — Stay on a specific 5.15.x kernel for stability
- **Testing** — Switch between kernel versions for debugging or compatibility testing
- **Docker/container compatibility** — Ensure kernel supports required features (similar to CentOS kernel upgrade scenario)

---

## Related Pages

- [[ubuntu]] — Ubuntu distribution page
- [[linux]] — Linux platform page
- [[kernel-upgrade-and-boot-management]] — kernel upgrade and boot management concept
- [[centos7-kernel-upgrade-via-elrepo]] — CentOS 7 kernel upgrade comparison
- [[linux-command-line-operations]] — Linux command-line operations concept
