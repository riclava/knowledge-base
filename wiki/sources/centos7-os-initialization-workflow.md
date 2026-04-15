---
title: CentOS 7 OS initialization workflow
type: source
created: 2026-04-15
updated: 2026-04-15
sources: [CentOS操作系统初始化流程.md]
tags: [source, centos, linux, initialization, post-install, hardening, ssh, ntp, chrony, selinux, firewalld, yum, epel, java, openjdk, iptables]
---

CentOS 7 操作系统初始化流程的实操笔记，覆盖从 ISO 获取到最小化安装、账户与网络配置、SSH 端口调整、内部镜像源切换、时间同步、安全策略简化和常用工具安装的完整链路。

## Source Facts

| 项目 | 内容 |
|---|---|
| 文档类型 | 操作流程 / 初始化 runbook |
| 目标系统 | CentOS 7（minimal 安装） |
| 核心场景 | 新装机器从裸机到可用状态的标准化配置 |
| 站点特定元素 | 内部镜像 `umirrors.un-net.com`、DNS `202.202.32.33`、VPN 端口 `5122`、NTP `time.un-net.com` |
| 安全简化 | 关闭 SELinux、关闭 firewalld（文档标注"如果不熟悉"） |

## Initialization Phases

### 1. ISO 获取与安装

- 推荐 minimal 安装，避免多余套件。
- 磁盘分区策略：测试环境可删除 `/home`，把剩余空间全部分给根分区；其他环境可单独处理额外磁盘。

### 2. 账户配置

- root 密码必须满足复杂度：至少包含大写、小写、数字、特殊字符中的三种。
- 如无需求，不创建无关账户。

### 3. 网络配置

- IaaS 环境下需申请 IP。
- DNS 服务器：`202.202.32.33`（站点特定）。
- 搜索域：`cqupt.edu.cn`（站点特定）。

### 4. SSH 端口调整

- 默认端口 22，按 VPN 要求调整为 5122。
- 可通过 iptables NAT 同时支持 22 和 5122：

```bash
iptables -t nat -A PREROUTING -p tcp --dport 5122 -j REDIRECT --to-ports 22
iptables-save > /etc/sysconfig/iptables
```

- 备注：VPN 默认开通端口为 80、443、5122、8989（Windows RDP 走 8989）。

### 5. 镜像源配置

- 清空默认 repo，切换到内部镜像：

```bash
cd /etc/yum.repos.d
rm -rf *.repo
curl -O http://umirrors.un-net.com/centos-usage/CentOS7/CentOS-Base.repo
yum makecache
```

- EPEL 配置：安装后用 `sed` 把 `mirrorlist` 改为内部 `baseurl`，并注释 `metalink`。

### 6. 时间同步

- 使用 NTP 或 chrony，同步地址为 `time.un-net.com` 或 `202.202.43.209`（站点特定）。

```conf
server time.un-net.com iburst
```

### 7. 安全策略简化

**SELinux**

```bash
# 永久关闭（需重启）
sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
# 临时关闭（当前生效，重启失效）
setenforce 0
# 两者结合：立即生效且下次启动仍关闭
```

**firewalld**

```bash
systemctl disable firewalld
systemctl stop firewalld
```

> ⚠️ **文档风险提示**：来源把关闭 SELinux 和 firewalld 作为默认步骤，适合快速验证或隔离测试环境。正式生产环境应优先配置而非关闭这些安全机制。

### 8. 常用工具安装

```bash
yum -y install net-tools tcpdump vim telnet lrzsz unzip
```

### 9. OpenJDK 环境配置（可选）

```bash
# 安装 OpenJDK（注意安装 devel 包）
yum install -y java-11-openjdk-devel

# 设置 JAVA_HOME
export JAVA_HOME=$(readlink -f /usr/bin/javac | sed "s:/bin/javac::")

# 写入 profile 永久生效
echo 'export JAVA_HOME=$(readlink -f /usr/bin/javac | sed "s:/bin/javac::")' >> /etc/profile
```

## Key Takeaways

1. **Minimal 安装 + 后续按需补充**：减少攻击面和维护负担。
2. **镜像源切换是第一优先级**：内部镜像可加速后续所有 `yum` 操作。
3. **SSH 端口调整是网络策略适配**：VPN 环境下非标准端口可能是必要条件。
4. **时间同步是基础设施依赖**：日志、证书、分布式系统都依赖一致时间。
5. **安全策略简化是双刃剑**：快速验证有用，但不应成为生产默认。

## Generic Pattern Extracted

这份来源虽然包含站点特定值（内部镜像、DNS、VPN 端口），但其结构可抽象为通用的 **OS 初始化检查清单**：

| 阶段 | 通用动作 |
|---|---|
| 安装 | 选择最小化安装，规划磁盘分区 |
| 账户 | 设置强密码，限制无关账户 |
| 网络 | 配置 IP、DNS、搜索域 |
| 远程访问 | 调整 SSH 端口或配置端口映射 |
| 软件源 | 切换到可用/快速镜像，配置 EPEL 等扩展仓库 |
| 时间同步 | 配置 NTP/chrony 指向可靠时间源 |
| 安全策略 | 根据环境决定 SELinux/firewalld 配置或临时关闭 |
| 基础工具 | 安装网络诊断、编辑器、传输工具 |
| 运行时环境 | 按需安装 JDK、Python 等语言运行时 |

## Relationship to Existing Wiki Pages

- [[centos]] — 本来源补充了 CentOS 7 从裸机到可用状态的初始化视角。
- [[linux]] — 本来源展示了 Linux 初始化阶段的典型配置表面。
- [[linux-command-line-operations]] — 本来源中的 `sed`、`iptables`、`systemctl`、`yum` 操作都属于命令行运维实践。
- [[legacy-repository-repointing]] — 本来源的镜像切换与 EPEL 配置是仓库管理的另一种形态。
- [[shell-scripting]] — 本来源中的 `sed -i`、`iptables-save`、`echo >> /etc/profile` 都是脚本化配置的典型手法。

## Related Pages

- [[centos]]
- [[linux]]
- [[linux-command-line-operations]]
- [[legacy-repository-repointing]]
- [[shell-scripting]]
- [[bash]]
- [[glossary]]
- [[overview]]
