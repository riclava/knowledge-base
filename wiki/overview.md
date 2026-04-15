---
title: Overview
type: overview
created: 2026-04-07
updated: 2026-04-15
sources: [2025年技术线总结.md, Moshi 与神经音频编码（Neural Audio Codec）技术架构解析.md, vim.md, bash.md, commands.md, CentOS6由于镜像废弃无法使用的解决办法.md, CentOS7离线安装docker问题排查.md]
tags: [overview, synthesis, engineering-management, ai, speech-llm, developer-tooling, linux, command-line, operations, vim, bash, shell, centos, yum, repository, docker, containers, kernel, networking]
---

# Knowledge Base Overview

*This page is the LLM's working synthesis of everything in the wiki. It updates after every ingest that shifts the big picture.*

---

## Current State

This wiki currently covers AI-era engineering management, speech-native AI architecture, and practical Linux/developer-tooling knowledge, combining strategic planning material with hands-on workflow references for editing, shell automation, command-line system operations, legacy package-source recovery on Linux distributions, and Docker-on-CentOS troubleshooting tied to host-kernel compatibility.

**Source count:** 7
**Wiki pages:** 31
**Last ingest:** 2026-04-15 — [[centos7-offline-docker-install-troubleshooting]]
**Last lint:** —

---

## What This Wiki Covers

- 技术线年度复盘与规划材料
- AI 时代的软件交付方式变化
- 平台底座、稳定性和治理能力建设
- 技术管理者关注的组织、流程和指标体系
- 语音原生 LLM、Neural Audio Codec 与实时语音交互架构
- Linux / 终端环境中的开发者工具、文本编辑与 shell 自动化工作流
- Linux 命令行中的文件、文本、进程、网络、权限、日志与定时任务操作
- 遗留 Linux 发行版在镜像退役后的仓库恢复与 archive/vault 运维补救
- CentOS 7 上 Docker 离线安装后的容器网络排障与宿主机内核兼容性判断

---

## Key Themes

- AI 不会替代工程基本盘，而是在其上叠加模型、数据、平台和新的组织分工。
- 小团队高效交付依赖完整交付能力，而不是单点英雄主义。
- 平台底座是复用、治理和 AI 落地的共同支点，目标是减少烟囱式系统。
- 可观测性、事故管理和容灾能力仍然是不可让步的工程基本功。
- 当前最大的治理短板是项目运营规范化尚未制度化。
- 在语音交互方向，关键基础设施正从传统 ASR/TTS 模块转向可供 LLM 直接消费的音频 tokenizer。
- 自然语音对话的竞争点不只是模型推理能力，还包括全双工交互、延迟预算和 codec 设计。
- 在终端工作流中，Vim 的价值不只是“能编辑文件”，而是通过模态编辑、组合命令和轻量配置把高频文本修改提炼成可复用操作语言。
- 在终端自动化方向，Bash 的价值不只是“命令拼接”，而是通过参数展开、选项处理、管道/重定向和错误控制，把重复操作沉淀为脚本化流程。
- 在 Linux 命令行运维方向，真正的能力表面来自围绕文件、文本、进程、网络和日志的一组小工具，它们通过“观察 -> 过滤 -> 修改 -> 验证”形成操作闭环。
- Linux 发行版运维不只包括执行命令，还包括处理版本生命周期、软件源可达性和仓库配置这类容易被忽视的基础前提。
- 在容器场景里，“Docker 安装成功”与“容器网络真正可用”是两回事；宿主机内核对 network namespace 的支持程度会直接改变排障方向。

---

## Open Questions

- 技术线在 2025 年完成交付的 AI 项目具体包括哪些项目和功能？
- 项目运营规范化未来会形成哪些具体制度、模板或会议机制？
- AI融合的“用户活跃度较高”采用什么统一衡量口径？
- 鸿蒙移动端适配和信创 Linux 落地是否已有后续补充材料？
- Moshi 的训练数据、评估指标、部署成本和开源状态是什么？
- Mimi 与 SNAC 在真实语音对话任务中的取舍边界是什么？
- 当前团队的编辑器基线是 Vim、Neovim，还是多编辑器并存？
- 是否还会补充 tmux、git、远程开发或 CI 脚本资料，形成更完整的终端工作流体系？
- 是否还会补充 `yum`/`dnf`/`apt` 的通用包管理、缓存刷新、第三方仓库与版本迁移文档？
- 当前 Docker 相关经验是否只限于 CentOS 7 离线安装/旧内核场景，还是还会补充更现代发行版与镜像分发实践？

---

## Knowledge Gaps

- 缺少平台底座能力清单和接入方式文档。
- 缺少事故管理、可观测性和容灾方案的具体规范。
- 缺少代码风格、接口规范、文档标准等治理类正式文档。
- 缺少对 1-2 人小团队模式的流程细化和案例材料。
- 缺少 speech-native LLM 的系统架构图、组件职责说明和延迟预算模板。
- 缺少 Neural Audio Codec 的选型矩阵、评估指标和任务分类方法。
- 虽然已补上 Bash、常用命令和基础 shell scripting，但仍缺少 POSIX shell / Bash 兼容性边界与脚本测试规范。
- Linux 运维侧目前虽已补上 CentOS 6 仓库恢复和一个 CentOS 7 Docker 网络兼容性案例，但仍缺少 `yum`/`dnf`/`apt` 的通用包管理、SSH、tmux、git、远程开发、系统化 Docker 运维和 CI 自动化等配套文档。

---

## Related Pages

- [[index]] — full catalog of all wiki pages
- [[glossary]] — terminology and style conventions
- [[2025-technical-line-summary]] — current primary source summary
- [[moshi-neural-audio-codec-architecture-analysis]] — speech-native LLM source summary
- [[technical-line-leader]] — current primary persona
- [[full-lifecycle-delivery-capability]] — key delivery concept
- [[ai-enabled-software-delivery]] — AI transition concept
- [[platform-foundation]] — platform reuse and governance concept
- [[observability-and-reliability]] — stability and operations concept
- [[moshi]] — speech-native LLM product page
- [[mimi]] — audio tokenizer product page
- [[speech-native-llm]] — speech-first model architecture
- [[neural-audio-codec]] — codec/tokenizer infrastructure
- [[full-duplex-speech-interaction]] — real-time duplex interaction model
- [[bash-syntax-and-scripting-reference]] — Bash source summary and scripting reference
- [[bash]] — shell and scripting tool page
- [[shell-scripting]] — automation concept behind command-line scripts
- [[linux-common-commands-reference]] — Linux commands source summary and operations reference
- [[centos7-offline-docker-install-troubleshooting]] — CentOS 7 Docker troubleshooting source summary
- [[centos6-archive-repository-workaround]] — CentOS 6 archive mirror recovery source summary
- [[centos]] — CentOS distribution page
- [[docker]] — Docker runtime/tooling page
- [[linux]] — Linux platform/tool page
- [[linux-command-line-operations]] — operations concept behind Linux command usage
- [[container-network-namespace-support]] — host-kernel support concept behind Docker bridge networking
- [[legacy-repository-repointing]] — archive/vault repo recovery concept for legacy systems
- [[vim-usage-and-configuration-reference]] — Vim source summary and command reference
- [[vim]] — modal editor tool page
- [[modal-editing]] — editing model behind Vim command composition
