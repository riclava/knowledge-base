---
title: Ubuntu 22.04 OpenSSH upgrade from source
type: source
created: 2026-04-15
updated: 2026-04-15
sources: [Ubuntu22.04升级OpenSSH版本到最新.md]
tags: [source, ubuntu, openssh, ssh, source-build, upgrade, security, systemd, pam]
---

Ubuntu 22.04 runbook for upgrading OpenSSH from upstream portable source, replacing system binaries in-place while preserving the existing systemd service configuration.

---

## Source Facts

| Item | Value |
|------|-------|
| Subject | OpenSSH source upgrade on Ubuntu 22.04 |
| Reference version | openssh-9.5p1 (example; check OpenBSD portable directory for latest) |
| Source URL | https://cdn.openbsd.org/pub/OpenBSD/OpenSSH/portable/ |
| Build approach | In-place replacement (`--prefix=/usr`) using system OpenSSL |
| Service management | Existing systemd unit preserved; only binaries replaced |

---

## Workflow Summary

### 1. Install Build Dependencies

```bash
apt update
apt install -y build-essential wget gcc make
apt install -y zlib1g-dev libssl-dev libpam0g-dev libselinux1-dev
```

Verify PAM headers are present:
```bash
ls -la /usr/include/security/pam_appl.h
```

### 2. Download and Extract Source

```bash
cd /tmp
wget https://cdn.openbsd.org/pub/OpenBSD/OpenSSH/portable/openssh-9.5p1.tar.gz
tar -xzf openssh-9.5p1.tar.gz
cd openssh-9.5p1
```

### 3. Backup Configuration

```bash
cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
```

> **Critical**: Do not disconnect the current terminal session. Use `screen` or `tmux` for safety.

### 4. Configure and Build

```bash
./configure \
  --prefix=/usr \
  --sysconfdir=/etc/ssh \
  --with-pam \
  --with-zlib \
  --with-ssl-dir=/usr \
  --without-hardening

make -j$(nproc) && make install
```

**Notes on removed options**:
- `--with-md5-passwords` — removed in newer OpenSSH versions
- `--with-tcp-wrappers` — Ubuntu 22.04 no longer supports TCP Wrappers

**Custom OpenSSL path**: If using a custom-built OpenSSL (e.g., `/usr/local/openssl`), replace `--with-ssl-dir=/usr` accordingly.

### 5. Post-install Configuration

Fix host key permissions:
```bash
chmod 600 /etc/ssh/ssh_host_rsa_key /etc/ssh/ssh_host_ecdsa_key /etc/ssh/ssh_host_ed25519_key
```

Temporary convenience settings (for recovery access — revert after verification):
```bash
sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config
sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/g' /etc/ssh/sshd_config
```

> **Warning**: `PermitRootLogin yes` and `PasswordAuthentication yes` are temporary settings to ensure recovery access during upgrade. Revert to secure baseline after successful verification.

### 6. Restart and Verify

```bash
systemctl restart ssh
systemctl enable ssh
ssh -V
systemctl status ssh
```

> Ubuntu uses service name `ssh`, not `sshd`.

### 7. Failure Recovery

If unable to connect after upgrade:
```bash
cp /etc/ssh/sshd_config.bak /etc/ssh/sshd_config
systemctl restart ssh
```

---

## Key Differences from CentOS 7 Approach

| Aspect | Ubuntu 22.04 | CentOS 7 |
|--------|--------------|----------|
| OpenSSL dependency | Uses system OpenSSL (typically recent enough) | Often requires building OpenSSL from source first |
| Install prefix | `/usr` (in-place replacement) | Often `/usr/local/openssl` + `/usr` for OpenSSH |
| Package removal | Not required (overwrites directly) | May require `rpm -e --nodeps` to remove old packages |
| Service unit | Preserved; systemd unit already exists | May need init script or unit file setup |
| TCP Wrappers | Not supported | Not supported (removed in newer versions) |

---

## Operational Risks

| Risk | Mitigation |
|------|------------|
| Session loss during upgrade | Keep current terminal open; use `screen`/`tmux` |
| Configuration syntax error | Run `sshd -t` before restart (not mentioned in source but recommended) |
| Permission issues on host keys | Explicitly `chmod 600` after install |
| Lockout from relaxed auth settings | Revert `PermitRootLogin` and `PasswordAuthentication` after verification |

---

## Documentation Gaps in This Source

- No `sshd -t` configuration validation step before restart
- No checksum/signature verification for downloaded tarball
- No guidance on reverting to package-managed OpenSSH if needed
- No coverage of key-based authentication migration or cipher suite hardening
- No mention of SELinux/AppArmor context considerations
- Temporary auth settings not explicitly marked for reversion

---

## Related Pages

- [[ubuntu]] — Ubuntu distribution page
- [[openssh]] — OpenSSH product page
- [[source-built-package-replacement]] — concept for source-compiled system software replacement
- [[centos7-openssl-and-openssh-upgrade-from-source]] — CentOS 7 equivalent runbook (independent approach)
- [[linux]] — Linux platform page
- [[glossary]] — terminology reference

