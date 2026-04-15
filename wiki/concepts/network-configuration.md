---
title: Network Configuration
type: concept
created: 2026-04-15
updated: 2026-04-15
sources: [netplan配置指南.md]
tags: [networking, linux, configuration, yaml, dhcp, static-ip, vlan, bonding, bridging, routing]
---

# Network Configuration

Practice of defining and managing network interface settings, IP addressing, routing, and DNS through declarative configuration files or tools, enabling reproducible and version-controlled network setup.

---

## Core Idea

Network configuration transforms network requirements (IP addresses, routes, DNS, interface bonding) into persistent system state. Modern approaches favor:

- **Declarative configuration**: Describe desired state, let the system converge
- **Abstraction layers**: Tools like Netplan abstract over multiple backends
- **Version control**: Text-based configs can be tracked in git
- **Safe testing**: Mechanisms to test changes with automatic rollback

---

## Configuration Layers

### 1. Interface Identity

- Physical interface naming (`eth0`, `enp0s3`, `ens192`)
- MAC address matching and interface renaming
- Predictable interface names (systemd-based)

### 2. IP Addressing

| Mode | Description |
|------|-------------|
| DHCP | Automatic address assignment from server |
| Static | Manually specified address and prefix |
| Link-local | Auto-configured addresses for local segment |
| Multiple IPs | Multiple addresses on single interface |

### 3. Routing

- Default gateway for internet-bound traffic
- Static routes for specific network segments
- Route metrics for path preference
- Policy-based routing for complex topologies

### 4. DNS Resolution

- Nameserver addresses
- Search domains for short hostname resolution
- Per-interface DNS settings

### 5. Advanced Topologies

| Topology | Purpose |
|----------|---------|
| VLAN | Logical network segmentation on shared physical infrastructure |
| Bond | Link aggregation for redundancy or throughput |
| Bridge | Layer 2 connectivity between interfaces (common for VMs/containers) |

---

## Configuration Approaches by Distribution

| Distribution | Primary Tool | Config Location |
|--------------|--------------|-----------------|
| Ubuntu 17.10+ | Netplan | `/etc/netplan/*.yaml` |
| Ubuntu (legacy) | ifupdown | `/etc/network/interfaces` |
| RHEL/CentOS 7 | NetworkManager, ifcfg | `/etc/sysconfig/network-scripts/` |
| RHEL/CentOS 8+ | NetworkManager | `nmcli`, `/etc/NetworkManager/` |
| Debian | ifupdown or NetworkManager | `/etc/network/interfaces` |

---

## Netplan Model (Ubuntu)

Netplan provides a YAML-based abstraction:

```yaml
network:
  version: 2
  renderer: networkd  # or NetworkManager
  ethernets:
    eth0:
      addresses: [192.168.1.100/24]
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8]
```

Key properties:
- **Declarative**: Describe what you want, not how to achieve it
- **Backend-agnostic**: Same YAML works with systemd-networkd or NetworkManager
- **Safe testing**: `netplan try` applies changes with automatic rollback

---

## Common Configuration Scenarios

### Basic Static IP

Essential elements:
1. IP address with prefix length (CIDR notation)
2. Default gateway
3. DNS nameservers

### Multi-Homed Host

Considerations:
- Separate routes for each network segment
- Default gateway on only one interface (or use metrics)
- Source-based routing if needed

### High Availability

- **Bonding**: Combine multiple NICs for redundancy/throughput
- **VLAN trunking**: Carry multiple networks over single physical link
- **Bridging**: Connect VMs/containers to physical network

---

## Operational Patterns

### Safe Change Workflow

1. **Backup**: Copy existing configuration
2. **Edit**: Modify configuration file
3. **Validate**: Syntax check before applying
4. **Test**: Apply with rollback mechanism if available
5. **Verify**: Confirm connectivity
6. **Commit**: Make change permanent

### Emergency Recovery

When configuration error causes loss of connectivity:
- Use console access (not SSH)
- Apply temporary IP with `ip addr add`
- Fix configuration file
- Re-apply proper configuration

### Verification Commands

```bash
ip addr show       # View addresses
ip route show      # View routing table
ip link show       # View interface status
ss -tunlp          # View listening ports
ping <gateway>     # Test gateway reachability
```

---

## Risks and Mitigations

| Risk | Mitigation |
|------|------------|
| Lockout from remote system | Use `netplan try` or equivalent; have console access |
| Syntax errors | Validate before applying; use `--debug` flags |
| Conflicting configurations | Single source of truth; avoid mixing tools |
| Credentials in config | Restrict file permissions (600); consider secrets management |

---

## Related Pages

- [[netplan-configuration-guide]] — Ubuntu Netplan configuration reference
- [[ubuntu]] — Ubuntu distribution page
- [[linux]] — Linux platform page
- [[container-network-namespace-support]] — container networking and namespace concepts

