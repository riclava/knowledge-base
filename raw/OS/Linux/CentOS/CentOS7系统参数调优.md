# CentOS7 系统参数调优

本文档涵盖 CentOS7 系统的常见性能参数调优，包括文件描述符限制和 TCP 连接数配置。

## 1. 文件描述符限制 (open-files)

### 1.1 用户级别限制

通过 `/etc/security/limits.conf` 控制用户级别的文件描述符限制。

**查看当前限制：**

```bash
ulimit -n
```

**修改配置：**

编辑 `/etc/security/limits.conf`，添加：

```conf
*       soft    nofile  65536
*       hard    nofile  65536
```

### 1.2 系统级别限制

通过 `/etc/sysctl.conf` 控制系统级别的文件描述符限制。

**查看当前限制：**

```bash
# 默认值为 1000000
cat /proc/sys/fs/file-max
```

**修改配置：**

编辑 `/etc/sysctl.conf`，添加：

```conf
fs.file-max = 2000000
```

使配置生效：

```bash
sysctl -p
```

### 1.3 systemd 管理的服务

对于 systemd 管理的服务，需要单独配置：

1. 找到对应的 service 文件
2. 在 `[Service]` 段添加 `LimitNOFILE=65535`
3. 重新加载配置：`systemctl daemon-reload`
4. 重启服务
5. 验证：`cat /proc/${PID}/limits`

## 2. TCP 连接数配置

编辑 `/etc/sysctl.conf`，添加：

```conf
# 增加系统级别的最大 TCP 连接数
net.core.somaxconn = 10240
net.ipv4.tcp_max_syn_backlog = 10240
```

使配置生效：

```bash
sysctl -p
```

## 3. 完整的调优配置示例

以下是一个综合的 `/etc/sysctl.conf` 配置示例：

```conf
# 文件描述符
fs.file-max = 2000000

# TCP 连接数
net.core.somaxconn = 10240
net.ipv4.tcp_max_syn_backlog = 10240
```

以下是 `/etc/security/limits.conf` 配置示例：

```conf
*       soft    nofile  65536
*       hard    nofile  65536
```
