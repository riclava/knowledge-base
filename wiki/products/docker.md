---
title: Docker
type: product
created: 2026-04-15
updated: 2026-04-15
sources: [CentOS7离线安装docker问题排查.md, CentOS7升级内核.md]
tags: [product, docker, containers, centos, linux, container-networking, kernel, elrepo, troubleshooting]
---

Docker 是一个容器运行时与镜像分发工具链；在当前知识库里，它首先以“安装成功但宿主机网络能力不兼容时仍会运行异常”的运维对象出现，而新的内核升级来源又补上了其中一条明确的宿主机修复路径。

## Product Snapshot

| 项目 | 内容 |
|---|---|
| 产品类型 | 容器运行时 / 镜像与容器管理工具 |
| 当前来源中的版本 | `Docker version 20.10.8, build 3967b7d` |
| 当前来源中的部署方式 | 从另一台 CentOS 7 主机离线下载依赖后安装 |
| 关键宿主机对象 | `docker0`、`veth*`、`iptables`、network namespace、宿主机内核、GRUB 默认启动项 |
| 最小验证路径 | `systemctl restart docker` -> `docker run -p` -> `curl localhost` |
| 当前知识库中的修复思路 | 先确认宿主机内核是否满足要求，再选择较新的官方内核或 ELRepo `kernel-lt` 等升级路径 |

## Core Capability Surface in This Source

### Installation Path Is Separate from Runtime Fitness

- 来源的起点是“离线安装 Docker”，但故障并不出在安装命令本身。
- 这说明 Docker 文档应把“包是否装上”与“运行时功能是否真的可用”分开验证。

### Bridge Networking Depends on Host Kernel Behavior

- `docker0` 和 `veth*` 是 Docker 把容器接到宿主机网络上的直接操作面。
- 当前知识库中，真正的故障线索来自这些网络对象与 namespace ID 关系异常，而不是 `docker --version` 或重启服务失败。

### Published-Port Testing Is a High-Signal Health Check

- 用 `docker run -ti --rm -p 8080:80 nginx:1.18.0` 再配合本机 `curl`，可以把问题迅速收敛到端口发布链路。
- 这种方法对技术文档很重要，因为它比“大容器能不能启动”更接近 Docker 网络是否真的工作。

### Reinstall and Cleanup Do Not Eliminate Host-Side Root Causes

- 当前来源里，卸载 Docker、删除 `/var/lib/docker`、清空 `iptables`、删除桥接设备后重装，问题仍然存在。
- 这提醒我们：Docker 的数据目录和守护进程状态并不是唯一故障面，宿主机内核能力缺口可能更关键。

### Host-Kernel Remediation Path Should Be Explicit

- 旧来源证明了“升级宿主机内核”是修复方向，但没有把升级流程本身写出来。
- 新来源补上了通过 ELRepo 安装 `kernel-lt`、检查 `menuentry`、切换 GRUB 默认项并重启验证的完整链路。
- 两份来源合起来说明：Docker 文档不仅要会“查问题”，还要说明宿主机升级路径的选择原则，例如先尝试发行版官方维护内核，还是直接转向 ELRepo 替代分支。

## Why It Matters

- Docker 在老旧 Linux 发行版上的问题，常常并不是“命令不会用”，而是宿主机版本与运行时要求之间存在隐含兼容性边界。
- 对技术写作来说，Docker 文档不能只给安装步骤，还需要告诉读者该如何验证 bridge 网络、端口发布和 namespace 能力，以及当根因落在宿主机内核时应该怎样升级。
- 对知识库建设来说，Docker 也把 [[linux]] 的网络对象、[[centos]] 的版本差异、[[container-network-namespace-support]] 和 [[kernel-upgrade-and-boot-management]] 这样的底层概念连了起来。

## Documentation Notes

- 应在安装文档中显式记录宿主机发行版和内核版本，而不是只写 Docker 版本。
- 应提供最小验证命令，而不是默认“服务启动成功 == 功能可用”。
- 如果面向离线环境，应补上 RPM 来源、校验方式和版本匹配原则。
- 如果排障涉及 `iptables` 清空或删除 `docker0` / `veth`，应说明影响范围和恢复方式。
- 当修复路径涉及内核升级时，应明确区分“更新到较新的 CentOS 官方维护内核”和“改走 ELRepo `kernel-lt` / `kernel-ml`”两类方案。

## Known Gaps from This Source

- 还没有覆盖镜像仓库、`docker pull/save/load`、Compose、存储驱动、日志驱动、cgroups、SELinux 或 `containerd` 关系。
- 没有给出 Docker 与特定 CentOS 7 内核基线的兼容矩阵；当前只展示了一个旧内核案例和一条 ELRepo 升级流程。
- 没有覆盖在线安装、企业内网镜像仓库或更现代发行版上的 Docker 运维实践。

## Related Pages

- [[centos7-offline-docker-install-troubleshooting]]
- [[centos7-kernel-upgrade-via-elrepo]]
- [[kernel-upgrade-and-boot-management]]
- [[centos]]
- [[linux]]
- [[container-network-namespace-support]]
- [[linux-command-line-operations]]
- [[glossary]]
- [[overview]]
