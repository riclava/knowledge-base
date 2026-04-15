---
title: Ubuntu 20.04 Docker development environment setup
type: source
created: 2026-04-15
updated: 2026-04-15
sources: [基于docker构建ubuntu20.04开发环境.md]
tags: [source, docker, ubuntu, development-environment, containers, apt-mirror, china-mirror]
---

基于 Docker 快速构建 Ubuntu 20.04 开发环境的最小配方，包含容器启动脚本和系统初始化脚本。

## Source Facts

| 项目 | 内容 |
|---|---|
| 文件 | `raw/OS/Linux/Ubuntu/基于docker构建ubuntu20.04开发环境.md` |
| 主题 | 使用 Docker 创建 Ubuntu 20.04 开发环境 |
| 基础镜像 | `ubuntu:20.04` |
| 容器保活方式 | `sleep infinity` |
| 镜像源 | USTC（中国科学技术大学）镜像 |
| 安装工具 | `tzdata`, `vim`, `curl`, `wget`, `git` |

## Key Content

### 启动脚本

```bash
#!/bin/bash

docker run -d \
    --name=ubuntu20 \
    --restart=unless-stopped \
    ubuntu:20.04 \
    sleep infinity
```

**要点：**
- 使用 `-d` 后台运行
- 使用 `--restart=unless-stopped` 确保容器在 Docker 重启后自动恢复
- 使用 `sleep infinity` 作为主进程保持容器存活，而不是直接进入交互式 shell

### 初始化脚本

```bash
sed -i 's@//.*archive.ubuntu.com@//mirrors.ustc.edu.cn@g' /etc/apt/sources.list
sed -i 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list

apt-get -y update && apt-get -y dist-upgrade
apt -y install tzdata vim curl wget git
```

**要点：**
- 先切换 APT 镜像源到 USTC（中国大陆网络环境下的常见选择）
- 执行系统更新和升级
- 安装基础开发工具集

## Pattern: Containerized Development Environment

这份来源展示了一种轻量级开发环境构建模式：

1. **隔离性**：开发环境与宿主机隔离，避免污染宿主机系统
2. **可复现性**：基于标准镜像，可以快速重建
3. **保活机制**：使用 `sleep infinity` 而非交互式 shell，容器可以后台持续运行
4. **按需进入**：通过 `docker exec -it ubuntu20 bash` 进入容器工作

## Site-Specific Values

| 项目 | 来源中的值 | 说明 |
|---|---|---|
| APT 镜像 | `mirrors.ustc.edu.cn` | 中国大陆网络环境下的常见选择；其他环境应替换为本地可达的镜像源 |
| 容器名称 | `ubuntu20` | 示例名称，可按需调整 |

## What This Source Does NOT Cover

- 没有涉及 Dockerfile 构建或镜像分发
- 没有涉及数据卷挂载（`-v`）或端口映射（`-p`）
- 没有涉及多容器编排或 Docker Compose
- 没有涉及 IDE 远程开发集成（如 VS Code Remote Containers）
- 没有涉及容器内用户权限或 sudo 配置
- 没有涉及更完整的开发工具链（编译器、语言运行时、调试器等）

## Documentation Notes

- 这是一份快速启动配方，适合需要临时隔离环境的场景
- 正式文档应补充数据持久化、端口暴露和工具链扩展说明
- 镜像源替换应标注为"站点特定配置"，而非通用默认值
- 如果面向团队使用，应考虑将初始化脚本固化为 Dockerfile

## Related Pages

- [[docker]]
- [[linux]]
- [[containerized-development-environment]]
- [[os-initialization-workflow]]
- [[glossary]]
- [[overview]]
