---
title: CentOS7离线安装docker问题排查
type: source
created: 2026-04-15
updated: 2026-04-15
sources: [CentOS7离线安装docker问题排查.md]
tags: [centos, linux, docker, offline-installation, container-networking, kernel, troubleshooting, source]
---

这是一份 CentOS 7 上离线安装 Docker 后容器网络异常的排障记录，最终把根因定位为宿主机内核过旧，而不是 Docker 安装步骤本身。

## Source Metadata

- Raw source: `raw/OS/Linux/CentOS/CentOS7离线安装docker问题排查.md`
- 时间标记：未注明
- 文档性质：问题排查笔记 / 环境对比记录
- 主要对象：CentOS 7、Docker 20.10.8、`docker0`、`veth*`、`iptables`、`ip netns list-id`
- 初始宿主机内核：`3.10.0-123.el7.x86_64`
- 升级后宿主机内核：`3.10.0-1160.36.2.el7.x86_64`
- 适用场景：CentOS 7 上 Docker 已安装、服务可重启，但容器端口映射访问异常时的排障

## What the Source Covers

| 阶段 | 操作 | 观察到的结果 |
|---|---|---|
| 离线安装准备 | 在另一台 CentOS 7 服务器上通过 `yum downloadonly` 获取安装包 | 说明安装依赖通过离线方式转移到目标机 |
| 清理网络遗留 | 下线并删除 `docker0`、`br-*`、`veth*` 网桥/网卡 | 试图排除 Docker 旧网络状态残留 |
| 清空防火墙规则 | 执行 `iptables -F` 与 `iptables -t nat -F` | 试图排除 NAT / filter 规则影响 |
| 复现实验 | `systemctl restart docker` 后运行 `docker run -ti --rm -p 8080:80 nginx:1.18.0`，再 `curl http://localhost:8080` | `curl` 返回 reset，说明端口发布链路仍有问题 |
| 排除常见误判 | 卸载 Docker、清空 `iptables`、删除 `/var/lib/docker`、删除网桥和网卡后重装 | 问题依然存在，表明根因不在 Docker 数据目录或残留配置 |
| 对比正常环境 | 对比容器启动后的 `ip a` 输出 | 异常环境的 `veth` 设备缺少 `link-netnsid` 标记 |
| 继续内核侧排查 | 执行 `ip netns list-id` | 报错 `RTM_GETNSID is not supported by the kernel.` |
| 升级内核并复测 | 升级到更新的 CentOS 7 内核后再次检查 | `ip netns list-id` 恢复正常，`ip a` 中重新出现 `link-netnsid 0` |

## Core Mental Model Implied by the Source

1. “Docker 能安装、`docker --version` 正常”不等于“容器网络一定可用”。
2. 当容器端口映射失败且重装无效时，排障重点应从 Docker 包本身下探到宿主机内核和网络命名空间能力。
3. `ip a` 的差异比单纯看服务状态更有信息量；与正常环境做对比能快速暴露宿主机网络对象层面的异常。
4. 旧内核缺失的能力可能通过 `ip netns list-id` 这类工具侧信号暴露出来，再由内核升级真正修复。

## Notable Operational Patterns

### Use a Minimal Published-Port Test

- 来源用 `nginx:1.18.0` 和 `curl http://localhost:8080` 搭出最小复现场景，把问题压缩到“容器发布端口后本机访问是否成功”。
- 这种测试比直接上业务容器更容易排除应用层变量。

### Compare Host Networking Objects, Not Just Daemon Status

- 正常与异常环境都能看到 `docker0` 和 `veth` 设备，因此“接口存在”本身不足以证明网络链路健康。
- 更有价值的差异是 `veth@ifXX ... link-netnsid 0` 是否出现，以及 `ip netns list-id` 是否能正常返回 namespace ID。

### Treat Reinstall as a Hypothesis, Not a Fix

- 来源先后尝试卸载、删除 `/var/lib/docker`、清空 `iptables`、删除桥接接口并重装 Docker。
- 这些步骤都没有解决问题，反而帮助证明根因位于更底层的内核能力而非 Docker 状态脏污。

### Kernel Version Is a Runtime Dependency Signal

- 根因定位最终依赖 `uname -a` 与 `ip netns list-id` 的组合观察。
- 从 `3.10.0-123` 升级到 `3.10.0-1160.36.2` 后，namespace ID 查询恢复正常，也让 Docker 网络对象呈现出更完整的关联信息。

## Caveats and Verification Risks

- 来源没有列出离线安装时实际使用的 RPM 包集合、仓库来源、校验方式或完整安装命令，因此它更像“排障记录”而不是可直接复刻的离线安装教程。
- 来源没有展示 `docker info`、`docker network inspect bridge`、`ss -lntp`、`journalctl -u docker` 或 SELinux / `firewalld` 状态，因此无法证明问题只会由内核触发。
- `link-netnsid` 缺失是这份来源中的关键线索，但在其他环境里不应把它机械地当作唯一判据。
- `curl` 测试只覆盖本机回环访问，没有展示跨主机访问、云安全组或上游负载均衡链路。

## Documentation Implications

- 如果整理成正式文档，应把它写成“Docker 容器网络排障案例”而不是“离线安装 Docker 教程”。
- 应明确区分“安装路径问题”“Docker 守护进程问题”“宿主机内核/网络命名空间兼容性问题”三类故障面。
- 应把 `uname -a`、最小 `docker run -p` 测试、`ip a` 对比、`ip netns list-id` 检查列成标准验证步骤。
- 如果补充教程版本，最好增加离线包清单、内核升级步骤、升级前后验证和回滚说明。

## Related Pages

- [[docker]]
- [[centos]]
- [[linux]]
- [[container-network-namespace-support]]
- [[linux-command-line-operations]]
- [[glossary]]
- [[overview]]
