---
title: systemd-resolved DNS Management
type: concept
created: 2026-04-15
updated: 2026-04-15
sources: [Ubuntu常见问题与优化.md]
tags: [linux, ubuntu, dns, systemd, systemd-resolved, networking, troubleshooting]
---

# systemd-resolved DNS Management

Practice of configuring and troubleshooting `systemd-resolved`, the default DNS resolver service on modern Ubuntu and other systemd-based Linux distributions, including managing port 53 conflicts and DNS stub listener behavior.

---

## Core Idea

`systemd-resolved` provides DNS resolution services for local applications, including:
- Local DNS caching
- DNSSEC validation
- DNS-over-TLS support
- Per-interface DNS configuration
- A local DNS stub listener on 127.0.0.53:53

The stub listener creates a common conflict scenario when deploying local DNS services.

---

## Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Applications                          │
│                         │                                │
│                         ▼                                │
│              /etc/resolv.conf                            │
│         (symlink to resolved config)                     │
│                         │                                │
│                         ▼                                │
│    ┌─────────────────────────────────────┐              │
│    │        systemd-resolved             │              │
│    │  ┌─────────────────────────────┐    │              │
│    │  │   DNS Stub Listener         │    │              │
│    │  │   127.0.0.53:53             │    │              │
│    │  └─────────────────────────────┘    │              │
│    │              │                       │              │
│    │              ▼                       │              │
│    │     Upstream DNS Servers            │              │
│    └─────────────────────────────────────┘              │
└─────────────────────────────────────────────────────────┘
```

---

## Configuration Files

| File | Purpose |
|------|---------|
| `/etc/systemd/resolved.conf` | Main configuration |
| `/run/systemd/resolve/resolv.conf` | Generated config with upstream servers |
| `/run/systemd/resolve/stub-resolv.conf` | Generated config pointing to stub (127.0.0.53) |
| `/etc/resolv.conf` | Symlink to one of the above |

---

## Common Scenarios

### Port 53 Conflict

**Problem:** Local DNS service (dnsmasq, Pi-hole, BIND, custom DNS) cannot bind to port 53.

**Diagnosis:**
```bash
ss -tunlp | grep :53
# Shows systemd-resolve listening on 127.0.0.53:53
```

**Solution:** Disable the DNS stub listener:

```bash
# Edit configuration
sudo sed -i 's/#DNSStubListener=yes/DNSStubListener=no/' /etc/systemd/resolved.conf

# Switch resolv.conf to use upstream config directly
sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf

# Restart service
sudo systemctl restart systemd-resolved.service

# Verify port is free
ss -tunlp | grep :53
```

### Checking DNS Status

```bash
# View current DNS configuration
resolvectl status

# Or on older systems
systemd-resolve --status

# Test DNS resolution
resolvectl query example.com
```

### Per-Interface DNS

`systemd-resolved` can use different DNS servers for different interfaces, useful for:
- VPN split-DNS scenarios
- Multi-homed hosts
- Container networking

---

## resolv.conf Symlink Options

| Target | Behavior |
|--------|----------|
| `/run/systemd/resolve/stub-resolv.conf` | Use stub listener (127.0.0.53) — default |
| `/run/systemd/resolve/resolv.conf` | Use upstream servers directly |
| Static file | Bypass systemd-resolved entirely |

---

## Key Configuration Options

In `/etc/systemd/resolved.conf`:

```ini
[Resolve]
# Upstream DNS servers
DNS=8.8.8.8 1.1.1.1

# Fallback DNS servers
FallbackDNS=8.8.4.4

# Search domains
Domains=example.com

# Enable/disable stub listener
DNSStubListener=yes|no

# DNSSEC validation
DNSSEC=allow-downgrade|yes|no

# DNS over TLS
DNSOverTLS=opportunistic|yes|no
```

---

## Interaction with Other Services

| Service | Interaction |
|---------|-------------|
| NetworkManager | Can push DNS settings to systemd-resolved |
| Netplan | Configures DNS via systemd-resolved on Ubuntu |
| Docker | May need custom DNS configuration if stub is disabled |
| dnsmasq | Conflicts with stub listener; disable stub or use different port |

---

## Risks and Mitigations

| Risk | Mitigation |
|------|------------|
| Breaking DNS resolution | Test changes with console access available |
| Losing DNS caching | Upstream servers handle caching; or run local caching DNS |
| Container DNS issues | Configure Docker/Podman DNS explicitly if needed |

---

## Related Pages

- [[ubuntu]] — Ubuntu distribution page
- [[ubuntu-common-issues-and-optimization]] — source summary with DNS troubleshooting
- [[linux]] — Linux platform page
- [[network-configuration]] — declarative network configuration concept
- [[netplan-configuration-guide]] — Netplan configuration reference
