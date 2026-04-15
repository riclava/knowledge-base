# Netplan 配置指南

## 简介

Netplan 是 Ubuntu 17.10+ 和 Ubuntu Server 18.04+ 默认的网络配置工具，使用 YAML 格式配置文件，后端可以使用 NetworkManager 或 systemd-networkd。

## 配置文件位置

- `/etc/netplan/*.yaml` - 主配置目录
- `/run/netplan/*.yaml` - 运行时配置（优先级更高）
- `/lib/netplan/*.yaml` - 系统默认配置（优先级最低）

常见配置文件：
- `00-installer-config.yaml` - 安装程序生成
- `01-netcfg.yaml` - 自定义配置

## 基本语法规则

```yaml
network:
  version: 2
  renderer: networkd  # 或 NetworkManager
  ethernets:
    # 以太网配置
  wifis:
    # WiFi 配置
  bridges:
    # 网桥配置
  bonds:
    # 绑定配置
  vlans:
    # VLAN 配置
```

**重要注意事项：**
- YAML 格式对缩进敏感，必须使用空格（不能用 Tab）
- 每级缩进 2 个空格
- 冒号后必须有空格
- 配置文件权限应为 600 或 644

## 常用配置场景

### 1. DHCP 自动获取 IP（默认）

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: true
      dhcp6: false
```

### 2. 静态 IP 配置

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      addresses:
        - 192.168.1.100/24
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 114.114.114.114
        search:
          - example.com
```

### 3. 多网卡配置

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      addresses:
        - 192.168.1.100/24
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 114.114.114.114]
    
    eth1:
      addresses:
        - 10.0.0.100/24
      routes:
        - to: 10.0.0.0/8
          via: 10.0.0.1
```

### 4. 静态路由配置

```yaml
network:
  version: 2
  ethernets:
    eth0:
      addresses:
        - 192.168.1.100/24
      routes:
        - to: default
          via: 192.168.1.1
          metric: 100
        - to: 10.0.0.0/8
          via: 192.168.1.254
        - to: 172.16.0.0/12
          via: 192.168.1.253
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

### 5. 设置 MTU

```yaml
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: true
      mtu: 1450
```

### 6. VLAN 配置

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: false
  
  vlans:
    vlan10:
      id: 10
      link: eth0
      addresses:
        - 192.168.10.100/24
    
    vlan20:
      id: 20
      link: eth0
      addresses:
        - 192.168.20.100/24
```

### 7. Bond（链路聚合）配置

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: false
    eth1:
      dhcp4: false
  
  bonds:
    bond0:
      interfaces:
        - eth0
        - eth1
      addresses:
        - 192.168.1.100/24
      routes:
        - to: default
          via: 192.168.1.1
      parameters:
        mode: active-backup  # 或 802.3ad, balance-rr 等
        mii-monitor-interval: 100
        primary: eth0
```

**Bond 模式说明：**
- `balance-rr` (0): 轮询模式，负载均衡
- `active-backup` (1): 主备模式，容错
- `balance-xor` (2): XOR 模式
- `broadcast` (3): 广播模式
- `802.3ad` (4): LACP 模式（交换机需支持）
- `balance-tlb` (5): 传输负载均衡
- `balance-alb` (6): 自适应负载均衡

### 8. Bridge（网桥）配置

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    eth0:
      dhcp4: false
    eth1:
      dhcp4: false
  
  bridges:
    br0:
      interfaces:
        - eth0
        - eth1
      addresses:
        - 192.168.1.100/24
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8]
      parameters:
        stp: true  # 启用生成树协议
        forward-delay: 4
```

### 9. WiFi 配置

```yaml
network:
  version: 2
  renderer: NetworkManager
  wifis:
    wlan0:
      access-points:
        "SSID名称":
          password: "WiFi密码"
      dhcp4: true
```

### 10. 多 IP 地址配置

```yaml
network:
  version: 2
  ethernets:
    eth0:
      addresses:
        - 192.168.1.100/24
        - 192.168.1.101/24
        - 192.168.1.102/24
      routes:
        - to: default
          via: 192.168.1.1
```

### 11. 配置 IPv6

```yaml
network:
  version: 2
  ethernets:
    eth0:
      addresses:
        - 192.168.1.100/24
        - 2001:db8::100/64
      routes:
        - to: default
          via: 192.168.1.1
        - to: default
          via: 2001:db8::1
      nameservers:
        addresses:
          - 8.8.8.8
          - 2001:4860:4860::8888
```

## 常用命令

### 应用配置

```bash
# 测试配置（不实际应用，只检查语法）
sudo netplan try

# 应用配置（自动选择 apply 或 generate）
sudo netplan apply

# 生成后端配置文件
sudo netplan generate

# 调试模式应用
sudo netplan --debug apply
```

### 查看配置

```bash
# 查看当前配置
cat /etc/netplan/*.yaml

# 查看网络接口
ip addr show
ip link show

# 查看路由表
ip route show

# 查看 DNS 配置
systemd-resolve --status
# 或
resolvectl status
```

### 重启网络服务

```bash
# 使用 systemd-networkd 时
sudo systemctl restart systemd-networkd

# 使用 NetworkManager 时
sudo systemctl restart NetworkManager

# 查看网络服务状态
sudo systemctl status systemd-networkd
```

## 配置生效流程

1. 编辑配置文件：`sudo nano /etc/netplan/01-netcfg.yaml`
2. 检查语法：`sudo netplan try`（建议，会自动回滚）
3. 应用配置：`sudo netplan apply`

## 故障排查

### 1. 语法检查

```bash
# 检查 YAML 语法
sudo netplan --debug generate

# 查看详细日志
sudo journalctl -u systemd-networkd -f
```

### 2. 常见错误

**错误：Invalid YAML**
- 检查缩进是否使用空格（不是 Tab）
- 检查冒号后是否有空格
- 检查引号是否配对

**错误：网络不通**
```bash
# 检查接口状态
ip link show

# 检查 IP 地址
ip addr show

# 检查路由
ip route show

# Ping 测试
ping -c 4 网关IP
ping -c 4 8.8.8.8
```

**错误：DNS 解析失败**
```bash
# 检查 DNS 配置
systemd-resolve --status

# 手动测试 DNS
nslookup www.baidu.com
dig www.baidu.com
```

### 3. 回退配置

```bash
# 使用 netplan try 会在 120 秒后自动回退
sudo netplan try

# 手动回退：恢复旧配置文件后
sudo netplan apply
```

### 4. 紧急恢复网络

如果配置错误导致无法连接，可以：

```bash
# 临时配置 IP（重启失效）
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip link set eth0 up
sudo ip route add default via 192.168.1.1

# 或使用 dhclient 临时获取 IP
sudo dhclient eth0
```

## 配置文件权限

```bash
# 推荐权限设置
sudo chmod 600 /etc/netplan/*.yaml
sudo chown root:root /etc/netplan/*.yaml
```

## 从旧版本迁移

### 从 ifupdown 迁移

旧的 `/etc/network/interfaces` 配置：

```
auto eth0
iface eth0 inet static
    address 192.168.1.100
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 114.114.114.114
```

转换为 netplan：

```yaml
network:
  version: 2
  ethernets:
    eth0:
      addresses:
        - 192.168.1.100/24
      routes:
        - to: default
          via: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 114.114.114.114]
```

## 高级技巧

### 1. 按网卡名称匹配

```yaml
network:
  version: 2
  ethernets:
    all-ethernet:
      match:
        name: "en*"
      dhcp4: true
```

### 2. 按 MAC 地址匹配

```yaml
network:
  version: 2
  ethernets:
    wan:
      match:
        macaddress: "00:11:22:33:44:55"
      set-name: wan0
      addresses:
        - 192.168.1.100/24
```

### 3. 禁用 IPv6

```yaml
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: true
      dhcp6: false
      ipv6-privacy: false
      link-local: []
```

### 4. 自定义 DHCP 选项

```yaml
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: true
      dhcp4-overrides:
        use-dns: false
        use-routes: false
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

## 参考资料

- [Netplan 官方文档](https://netplan.io/)
- [Ubuntu Netplan 参考](https://ubuntu.com/server/docs/network-configuration)
- [Netplan Examples](https://netplan.io/examples/)

## 相关命令速查

```bash
# 查看网卡信息
ip addr
ip link
ifconfig

# 查看路由
ip route
route -n

# 查看 DNS
systemd-resolve --status
cat /etc/resolv.conf

# 网络诊断
ping <IP>
traceroute <IP>
mtr <IP>
nslookup <domain>
dig <domain>

# 查看监听端口
ss -tunlp
netstat -tunlp
```

