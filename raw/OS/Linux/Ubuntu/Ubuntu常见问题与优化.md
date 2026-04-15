# Ubuntu 常见问题与优化

本文档整理了 Ubuntu 系统日常使用中的常见问题解决方案和优化技巧。

## 目录

- [系统优化](#系统优化)
- [APT 包管理](#apt-包管理)
- [常见问题排查](#常见问题排查)
- [旧版本参考](#旧版本参考)

---

## 系统优化

### 优化思路

| 优化方向 | 配置文件 | 说明 |
|---------|---------|------|
| 内核参数 | `/etc/sysctl.conf` | 网络缓存、文件句柄数、TCP 连接数等 |
| 文件系统 | `/etc/fstab` | 关闭不必要功能、SSD 优化等 |
| 网络参数 | `/etc/netplan/*.yaml` | TCP/IP 参数优化（详见 netplan 配置指南） |
| 调度器 | `/etc/default/grub` | CPU 调度策略 |
| 内存参数 | `/etc/sysctl.conf` | 内存缓存、减少泄漏 |

### 关闭 Swap

```bash
# 临时关闭
swapoff -a

# 永久关闭：注释掉 /etc/fstab 中的 swap 分区
sudo sed -i '/swap/s/^/#/' /etc/fstab
```

### 自动挂载 NFS

编辑 `/etc/fstab`：

```bash
# NFS 挂载配置
# 格式：RemotePath  MountPath  Type  Options  dump  pass
192.168.1.100:/data    /mnt/data   nfs   defaults,_netdev   0   0
```

---

## APT 包管理

### 仅下载不安装

适用于离线环境准备安装包：

```bash
# 下载指定包（包括依赖）
sudo apt-get install --download-only --reinstall -y <package-name>

# 复制到指定目录
cp /var/cache/apt/archives/*.deb ~/my-packages/
```

### 更换国内镜像源

```bash
# 备份原配置
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

# 替换为中科大镜像（Ubuntu 20.04 示例）
sudo sed -i 's@//.*archive.ubuntu.com@//mirrors.ustc.edu.cn@g' /etc/apt/sources.list
sudo sed -i 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list

# 更新索引
sudo apt update
```

### 常用维护命令

```bash
# 更新系统
sudo apt update && sudo apt upgrade -y

# 完整升级（包括内核）
sudo apt dist-upgrade -y

# 清理无用包
sudo apt autoremove -y
sudo apt autoclean
```

---

## 常见问题排查

### DNS 端口被占用（53 端口）

**问题**：Ubuntu 18.04+ 默认启用 `systemd-resolved` 服务，占用 53 端口，影响本地 DNS 服务部署。

**解决方案**：

```bash
# 修改配置
sudo sed -i 's/#DNSStubListener=yes/DNSStubListener=no/' /etc/systemd/resolved.conf

# 重新链接 resolv.conf
sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf

# 重启服务
sudo systemctl restart systemd-resolved.service

# 验证端口释放
ss -tunlp | grep :53
```

### Multipath 错误日志

**问题**：VMware 虚拟机中 syslog 持续报 multipath 相关错误。

**解决方案**：

```bash
# 创建配置文件
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

# 重启服务
sudo /etc/init.d/multipath-tools restart
```

---

## 旧版本参考

### Ubuntu 16.04 WiFi 自启动

> 注意：此配置仅适用于 Ubuntu 16.04 及更早版本，使用 `/etc/network/interfaces` 配置网络。
> Ubuntu 17.10+ 请使用 netplan 配置，参见 [netplan配置指南.md](./netplan配置指南.md)

```bash
# 生成 WiFi 配置
wpa_passphrase "SSID" "PASSWORD" | sudo tee /etc/wpa_supplicant/wpa_supplicant.conf

# 编辑 /etc/network/interfaces
auto lo
iface lo inet loopback

auto wlan0
iface wlan0 inet dhcp
    wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf

# 启用服务
sudo systemctl enable wpa_supplicant --now
```

---

## 相关文档

- [Netplan 配置指南](./netplan配置指南.md) - 网络配置详解
- [Ubuntu 切换指定版本内核](./Ubuntu切换指定版本内核.md) - 内核管理
- [Ubuntu 22.04 升级 OpenSSH](./Ubuntu22.04升级OpenSSH版本到最新.md) - 安全升级
- [基于 Docker 构建 Ubuntu 开发环境](./基于docker构建ubuntu20.04开发环境.md) - 容器化开发
