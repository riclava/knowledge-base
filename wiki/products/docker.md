---
title: Docker
type: product
created: 2026-04-15
updated: 2026-04-15
sources: [CentOS7离线安装docker问题排查.md]
tags: [product, docker, containers, centos, linux, container-networking, kernel, troubleshooting]
---

Docker 是一个容器运行时与镜像分发工具链；在当前知识库里，它首先以“安装成功但宿主机网络能力不兼容时仍会运行异常”的运维对象出现。

## Product Snapshot

| 项目 | 内容 |
|---|---|
| 产品类型 | 容器运行时 / 镜像与容器管理工具 |
| 当前来源中的版本 | `Docker version 20.10.8, build 3967b7d` |
| 当前来源中的部署方式 | 从另一台 CentOS 7 主机离线下载依赖后安装 |
| 关键宿主机对象 | `docker0`、`veth*`、`iptables`、network namespace、宿主机内核 |
| 最小验证路径 | `systemctl restart docker` -> `docker run -p` -> `curl localhost` |
| 当前来源中的核心风险 | Docker 安装成功但 CentOS 7 旧内核无法支撑正常的容器网络行为 |

## Core Capability Surface in This Source

### Installation Path Is Separate from Runtime Fitness

- 来源的起点是“离线安装 Docker”，但故障并不出在安装命令本身。
- 这说明 Docker 文档应把“包是否装上”与“运行时功能是否真的可用”分开验证。

### Bridge Networking Depends on Host Kernel Behavior

- `docker0` 和 `veth*` 是 Docker 把容器接到宿主机网络上的直接操作面。
- 当前来源中，真正的故障线索来自这些网络对象与 namespace ID 关系异常，而不是 `docker --version` 或重启服务失败。

### Published-Port Testing Is a High-Signal Health Check

- 用 `docker run -ti --rm -p 8080:80 nginx:1.18.0` 再配合本机 `curl`，可以把问题迅速收敛到端口发布链路。
- 这种方法对技术文档很重要，因为它比“大容器能不能启动”更接近 Docker 网络是否真的工作。

### Reinstall and Cleanup Do Not Eliminate Host-Side Root Causes

- 当前来源里，卸载 Docker、删除 `/var/lib/docker`、清空 `iptables`、删除桥接设备后重装，问题仍然存在。
- 这提醒我们：Docker 的数据目录和守护进程状态并不是唯一故障面，宿主机内核能力缺口可能更关键。

## Why It Matters

- Docker 在老旧 Linux 发行版上的问题，常常并不是“命令不会用”，而是宿主机版本与运行时要求之间存在隐含兼容性边界。
- 对技术写作来说，Docker 文档不能只给安装步骤，还需要告诉读者该如何验证 bridge 网络、端口发布和 namespace 能力。
- 对知识库建设来说，Docker 也把 [[linux]] 的网络对象、[[centos]] 的版本差异和 [[container-network-namespace-support]] 这样的底层概念连了起来。

## Documentation Notes

- 应在安装文档中显式记录宿主机发行版和内核版本，而不是只写 Docker 版本。
- 应提供最小验证命令，而不是默认“服务启动成功 == 功能可用”。
- 如果面向离线环境，应补上 RPM 来源、校验方式和版本匹配原则。
- 如果排障涉及 `iptables` 清空或删除 `docker0` / `veth`，应说明影响范围和恢复方式。

## Known Gaps from This Source

- 还没有覆盖镜像仓库、`docker pull/save/load`、Compose、存储驱动、日志驱动、cgroups、SELinux 或 `containerd` 关系。
- 没有给出 Docker 与特定 CentOS 7 内核基线的兼容矩阵，只展示了一个旧内核案例。
- 没有覆盖在线安装、企业内网镜像仓库或更现代发行版上的 Docker 运维实践。

## Related Pages

- [[centos7-offline-docker-install-troubleshooting]]
- [[centos]]
- [[linux]]
- [[container-network-namespace-support]]
- [[linux-command-line-operations]]
- [[glossary]]
- [[overview]]
