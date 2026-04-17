
---
title: Overview
type: overview
created: 2026-04-07
updated: 2026-04-17
sources: [2025年技术线总结.md, Moshi 与神经音频编码（Neural Audio Codec）技术架构解析.md, vim.md, bash.md, commands.md, CentOS6由于镜像废弃无法使用的解决办法.md, CentOS7离线安装docker问题排查.md, CentOS7配置Samba共享.md, CentOS7升级内核.md, CentOS7升级OpenSSL和OpenSSH.md, CentOS7系统参数调优.md, CentOS操作系统初始化流程.md, 基于docker构建ubuntu20.04开发环境.md, netplan配置指南.md, Ubuntu22.04升级OpenSSH版本到最新.md, Ubuntu常见问题与优化.md, Ubuntu切换指定版本内核.md, 构建技术研发思维.md, 快速基于 AI 入门全栈研发.md, Linux基础与测试专题.md, macOS安装U盘制作.md, macOS常用命令.md, macOS开发环境配置.md, macOS系统设置.md, Windows开发相关.md, Windows系统设置.md, 分布式核心原理.md]
tags: [overview, synthesis, engineering-management, ai, speech-llm, developer-tooling, engineering-thinking, systems-thinking, abstraction, modeling, validation, system-design, linux, unix, command-line, operations, observability, testing, quality, vim, bash, shell, centos, ubuntu, yum, repository, elrepo, grub, docker, containers, kernel, networking, netplan, yaml, vlan, bonding, bridging, samba, smb, file-sharing, windows, windows-update, task-scheduler, powershell, firewall, inno-setup, wmi, mfc, win32, installer, machine-identifier, openssl, openssh, ssh, tls, source-build, sysctl, systemd, tuning, file-descriptors, tcp, initialization, post-install, ntp, chrony, selinux, firewalld, epel, development-environment, pam, systemd-resolved, dns, swap, nfs, multipath, optimization, apt, boot-management, mcp, steering, devops, ci-cd, full-stack, macos, apple, recovery, createinstallmedia, bootable-media, startup-security, external-boot, homebrew, launchctl, diskutil, pmset, defaults, xcode, ruby, rbenv, cocoapods, ios, version-management, system-settings, input-method, trackpad, chrome, spotlight, browser-troubleshooting, distributed-systems, cap, flp, pacelc, consistency, consensus, replication, sharding, logical-clocks, resilience]

---

# Knowledge Base Overview

*This page is the LLM's working synthesis of everything in the wiki. It updates after every ingest that shifts the big picture.*


---

## Current State

This wiki currently covers AI-era engineering management, engineering-thinking frameworks for abstraction/modeling/validation, distributed-systems theory and engineering from CAP/FLP/PACELC through consistency, consensus, replication/sharding, logical clocks, and resilience patterns, AI-assisted full-stack onboarding patterns, speech-native AI architecture, and practical Linux/developer-tooling knowledge, combining strategic planning material with hands-on workflow references for editing, shell automation, command-line system operations, Unix-style design philosophy, host-level observability through USE and `/proc`-backed metrics, layered software-testing architecture, legacy package-source recovery on Linux distributions, Docker-on-CentOS troubleshooting tied to host-kernel compatibility, CentOS 7 kernel upgrade workflows through ELRepo and GRUB, CentOS 7 resource-limit and TCP backlog tuning, Samba-based cross-platform file sharing from CentOS to Windows, high-risk source-built OpenSSL/OpenSSH maintenance on legacy CentOS hosts, CentOS 7 OS initialization workflows from bare metal to usable baseline, Docker-based containerized development environment patterns, Ubuntu Netplan-based declarative network configuration, Ubuntu 22.04 OpenSSH source upgrade workflows, Ubuntu system optimization and troubleshooting including DNS port conflicts and VMware compatibility, Ubuntu kernel version switching via APT and GRUB configuration, macOS bootable USB installer creation and external-boot downgrade preparation, broader macOS command-line administration across `diskutil` / `launchctl` / `pmset` / Homebrew / Xcode CLI tools / `defaults`, lightweight macOS interactive system settings covering shortcut preferences, input-source defaults, trackpad behavior, and a Chrome preload workaround for typing lag, macOS development environment setup covering Ruby version management with rbenv and CocoaPods installation, Windows-native development notes covering Inno Setup packaging, service bootstrap sequencing, and WMI-based hardware fingerprint generation across MFC and Win32 paths, Windows workstation-administration notes covering Windows 10 auto-update suppression through services and scheduled tasks plus PowerShell-based firewall port opening for debugging, plus AI-era onboarding material for MCP, project Steering context, DevOps pipeline basics, and starter full-stack stack composition.

**Source count:** 27
**Wiki pages:** 90
**Last ingest:** 2026-04-17 — [[distributed-systems-core-principles]]
**Last lint:** —


---

## What This Wiki Covers

- 技术线年度复盘与规划材料
- 面向工程师成长的研发思维训练框架，覆盖抽象、建模、分层、验证和系统设计题拆解
- 分布式系统基础与工程实践，覆盖网络不确定性、CAP/FLP/PACELC、一致性模型、共识、复制/分片、逻辑时钟以及超时/重试/幂等/限流/熔断/降级/故障转移
- AI 时代的软件交付方式变化
- AI 时代全栈研发入门材料，覆盖本地环境、AI IDE / CLI Agent、MCP、Steering 与最小 DevOps 闭环
- 平台底座、稳定性和治理能力建设
- 技术管理者关注的组织、流程和指标体系
- 语音原生 LLM、Neural Audio Codec 与实时语音交互架构
- Linux / 终端环境中的开发者工具、文本编辑与 shell 自动化工作流
- Linux 命令行中的文件、文本、进程、网络、权限、日志、定时任务与系统状态验证
- Linux 设计哲学与管道思维，覆盖单一职责、文本接口、统一抽象、文件描述符和最小权限原则
- Linux 主机资源观测方法，覆盖四大资源、USE 方法、`/proc` / `/sys` 与 `node_exporter -> Prometheus -> Grafana` 观测链
- 遗留 Linux 发行版在镜像退役后的仓库恢复与 archive/vault 运维补救
- CentOS 7 上 Docker 离线安装后的容器网络排障与宿主机内核兼容性判断
- CentOS 7 上通过 ELRepo 升级内核、切换 GRUB 默认启动项并验证重启结果的操作流程
- CentOS 7 上通过 `limits.conf`、`sysctl` 和 `systemd` unit 提升文件句柄与 TCP backlog 上限的最小调优方法
- CentOS 7 上通过源码编译升级 OpenSSL 和 OpenSSH，并在线切换 `sshd` 服务的高风险维护流程
- Ubuntu 22.04 上通过源码编译升级 OpenSSH，使用系统 OpenSSL 并直接覆盖系统二进制的简化升级流程
- CentOS 7 上通过 Samba 向 Windows 暴露共享目录的最小配置与权限链路
- CentOS 7 从裸机到可用基线的标准化初始化流程，覆盖 minimal 安装、账户、网络、SSH 端口、镜像源、时间同步、安全策略和基础工具
- 基于 Docker 快速构建隔离开发环境的容器化模式，包括容器保活、镜像源配置和基础工具安装
- Ubuntu Netplan 声明式网络配置，覆盖 YAML 语法、DHCP/静态 IP、VLAN、Bond、Bridge 和安全测试工作流
- Ubuntu 系统优化与常见问题排查，覆盖 swap 管理、NFS 挂载、APT 包管理、systemd-resolved DNS 端口冲突和 VMware multipath 错误
- Ubuntu 内核版本切换，覆盖 APT 内核包安装、GRUB 默认启动项配置和重启验证
- macOS 命令行日常运维与开发环境准备，覆盖 `sw_vers`、`system_profiler`、`diskutil`、`launchctl`、`pmset`、Homebrew、Xcode CLI 工具和 `defaults` 偏好修改
- macOS 日常系统设置与浏览器体验优化，覆盖快捷键偏好、默认输入法、触控板行为和 Chrome 网页预加载相关的输入卡顿缓解
- macOS 开发环境配置，覆盖 rbenv Ruby 版本管理和 CocoaPods 安装
- macOS 安装 U 盘制作与外部启动安装流程，覆盖 `createinstallmedia`、Recovery 模式、启动安全性设置和基础故障恢复
- Windows 原生开发速记，覆盖 Inno Setup 安装包脚本、服务安装/卸载动作，以及基于 WMI 的机器码生成
- Windows 工作站管理速记，覆盖 Windows 10 自动更新抑制、计划任务/服务控制和 PowerShell 防火墙端口放行
- 软件测试架构，覆盖测试金字塔、属性测试、Contract Test、回归闭环、CI 时间分层和环境分工


---

## Key Themes

- AI 不会替代工程基本盘，而是在其上叠加模型、数据、平台和新的组织分工。
- 小团队高效交付依赖完整交付能力，而不是单点英雄主义。
- 平台底座是复用、治理和 AI 落地的共同支点，目标是减少烟囱式系统。
- 可观测性、事故管理和容灾能力仍然是不可让步的工程基本功。
- 当前最大的治理短板是项目运营规范化尚未制度化。
- 分布式系统设计的第一步不是追求完美方案，而是先明确业务优先级，再接受一致性、可用性、延迟和复杂度之间的结构性取舍。
- 超时、重试、幂等、Quorum、缓存失效、限流和故障转移不是实现细节，而是把理论边界转成可运行系统的日常工程表面。
- 在语音交互方向，关键基础设施正从传统 ASR/TTS 模块转向可供 LLM 直接消费的音频 tokenizer。
- 自然语音对话的竞争点不只是模型推理能力，还包括全双工交互、延迟预算和 codec 设计。
- 在终端工作流中，Vim 的价值不只是“能编辑文件”，而是通过模态编辑、组合命令和轻量配置把高频文本修改提炼成可复用操作语言。
- 在终端自动化方向，Bash 的价值不只是“命令拼接”，而是通过参数展开、选项处理、管道/重定向和错误控制，把重复操作沉淀为脚本化流程。
- 在开发者工作站场景里，效率问题不只出现在终端和工具链，也出现在输入法、触控板和浏览器设置这类交互层细节上。
- 在 Linux 基础培训语境里，命令行不应被当成“需要硬背的语法库”，而应被理解为一组能迁移到软件设计和系统思考中的原则。
- 在 Linux 命令行运维方向，真正的能力表面来自围绕文件、文本、进程、网络、日志、软件源与启动配置的一组小工具，它们通过“观察 -> 过滤 -> 修改 -> 验证”形成操作闭环。
- 在系统观测方向里，监控平台不是凭空产生洞察，而是把 Linux 宿主机通过 `/proc` / `/sys` 暴露的事实组织为指标、图表和告警；USE 方法提供了第一层排障顺序。
- 在容量调优场景里，“把句柄数调大”不是一句话能说明白的动作；登录会话限制、内核全局文件表、`systemd` 服务继承值和 TCP backlog 参数必须按层拆开写和验证。
- Linux 发行版运维不只包括执行命令，还包括处理版本生命周期、软件源可达性、第三方仓库信任和内核启动路径这类容易被忽视的基础前提。
- 在容器场景里，“Docker 安装成功”与“容器网络真正可用”是两回事；宿主机内核对 network namespace 的支持程度会直接改变排障方向。
- 在内核升级场景里，“新内核包已经安装”和“系统已从该内核成功启动”同样不是一回事，GRUB 默认项和重启验证是不可跳过的步骤。
- 在遗留 Linux 主机上，源码编译 OpenSSL/OpenSSH 不只是“装新版本”，还会牵动动态库路径、RPM 边界、认证策略和远程登录回滚能力。
- 在跨系统共享场景里，“Windows 能看到网络路径”与“Linux 目录、Samba 认证和主机安全策略都配置正确”同样不是一回事。

- 在新装机场景里，"装完系统"和"系统可用"之间还有账户、网络、软件源、时间同步、安全策略和基础工具等一系列标准化配置步骤。
- 关闭 SELinux/firewalld 是快速验证或隔离测试环境的简化手段，不应成为生产默认；正式文档应优先解释如何配置而非关闭。
- 在开发环境场景里，容器化不只是"把应用装进容器"，而是通过隔离、可复现和快速重建来降低环境配置的心智负担。
- 在网络配置场景里，声明式工具（如 Netplan）把"描述期望状态"和"让系统收敛到该状态"分开，使配置可版本化、可测试、可回滚。
- 在 Ubuntu 系统服务场景里，`systemd-resolved` 的 DNS stub listener 是常见的端口冲突来源；理解其架构有助于在部署本地 DNS 服务时快速定位问题。
- 在虚拟化场景里，VMware 虚拟磁盘不需要 multipath 支持，将其加入黑名单可以消除无意义的错误日志。
- 在桌面系统重装场景里，“安装介质已经制作完成”和“设备被允许从外部介质启动”不是一回事；恢复模式中的启动安全策略同样属于安装文档的一部分。
- 在 macOS 日常运维场景里，命令行能力分散在 BSD/Unix 工具和 Apple 专用接口之间；准确文档应写出 `diskutil`、`launchctl`、`pmset`、`defaults` 等精确控制面，而不是泛写成“Mac 命令”。
- 在 Windows 原生交付场景里，“可执行文件已经打包出来”和“安装器已经完成服务注册、启动与卸载闭环”不是一回事。
- 在 Windows 管理场景里，同一个目标可能同时涉及 `Services`、`Task Scheduler`、文件系统清理和 PowerShell；关闭自动更新应被写成特定上下文下的抑制措施，而不是普遍推荐配置。
- 在当前中文工程语境里，“机器码”可能指硬件派生设备标识而不是 machine code；文档必须显式澄清这个词的含义。
- 研发的核心不是直接写代码，而是先完成问题抽象、模型建立、系统边界划分，再进入实现。
- “降低不确定性”是工程师的重要职责，因此验证、极端 case 和权衡属于设计阶段，而不是实现后的附属动作。
- AI 协同不会削弱这些基本功，反而要求团队更明确地表达问题、状态、数据流和验收边界。
- AI 时代的全栈入门不应只停留在框架清单，而要把环境、Git/PR、CI/CD、部署、监控和测试看成一条连续链路。
- `MCP` 和项目 Steering 上下文分别解决“AI 能操作什么”和“AI 知道什么”的问题，是 AI Agent 真正进入工程流程时的两个关键支点。
- 把监控、日志和告警放进交付流程图，能帮助新人更早建立“上线后仍需持续验证”的心智模型。
- 在质量体系里，测试数量本身不是目标；更重要的是按单元/属性、集成、E2E 和回归数据闭环分配职责与反馈成本。
- “Bug -> 用例 -> 回归 -> CI -> 质量提升”是当前知识库里最清晰的测试演进闭环之一。

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
- 当前 Docker/内核兼容性经验会不会继续沉淀为“官方维护内核 vs ELRepo 替代内核”的选择指南或兼容矩阵？
- 当前 `nofile`/`sysctl` 调优经验会不会继续沉淀为面向 Nginx、数据库、消息队列或 JVM 服务的容量基线与验证模板？
- 当前 OpenSSL/OpenSSH 源码升级经验会不会继续沉淀为“何时必须源码替换、何时应坚持发行版包更新”的判断准则？
- 当前 Samba 相关经验是否只限于单目录映射场景，还是还会补充防火墙、SELinux、ACL 或域集成实践？
- 当前 Windows 经验是否会继续扩展到注册表、PowerShell 自动化、组策略/WSUS、服务控制管理器、代码签名或 MSI / WiX 打包？
- 当前 CentOS 7 初始化流程是否会继续沉淀为可复用的检查清单模板或自动化脚本？
- 是否会补充更多以数据库、缓存、队列或微服务为例的系统设计训练材料，形成更系统的研发思维案例库？
- 当前全栈入门来源中的技术栈示例，哪些属于教学用组合，哪些会沉淀为团队推荐基线？
- 是否会补充 Git/PR 规范、分支模型、CI 质量门禁和发布回滚等更细的交付文档？
- 是否会把 Steering 文件模板、MCP 接入约束和 AI 协作规则沉淀为跨项目标准？
- 是否会进一步沉淀团队级的测试金字塔比例、属性测试适用范围、Contract Test 规范和回归数据管理方式？
- 是否会补充 Prometheus / Grafana / node_exporter 的独立专题页与标准化监控指标清单？
- 是否会继续补充 macOS 专题，覆盖 Apple silicon / Intel 差异、`launchctl` domain model、Homebrew formula/cask、APFS / Disk Utility 和 Time Machine 相关流程？


---

## Knowledge Gaps

- 缺少平台底座能力清单和接入方式文档。
- 缺少事故管理、可观测性和容灾方案的具体规范。
- 缺少代码风格、接口规范、文档标准等治理类正式文档。
- 缺少对 1-2 人小团队模式的流程细化和案例材料。
- 缺少 speech-native LLM 的系统架构图、组件职责说明和延迟预算模板。
- 缺少 Neural Audio Codec 的选型矩阵、评估指标和任务分类方法。
- 虽然已补上 Bash、常用命令和基础 shell scripting，但仍缺少 POSIX shell / Bash 兼容性边界与脚本测试规范。
- Linux 运维侧目前虽已补上 CentOS 6 仓库恢复、一个 CentOS 7 Docker 网络兼容性案例、一个 ELRepo 内核升级 runbook、一个 `nofile`/TCP backlog 调优备忘、一个 Samba 最小共享案例、一个 OpenSSL/OpenSSH 源码升级案例、一个 CentOS 7 初始化流程和一个 Docker 容器化开发环境配方，但仍缺少 `yum`/`dnf`/`apt` 的通用包管理、SSH 加固基线、tmux、git、远程开发、系统化 Docker 运维、内核升级回滚规范、SMB/Samba 最小权限加固、基于 workload 的容量调优模板和 CI 自动化等配套文档。
- 容器化开发环境场景还没有覆盖数据卷挂载、端口映射、Dockerfile 构建、Docker Compose 编排或 IDE 远程开发集成（如 VS Code Remote Containers）。
- Ubuntu 网络配置目前只覆盖 Netplan，还没有涉及 `ufw` 防火墙、AppArmor、云环境网络（如 AWS VPC）或 IPv6-only 部署场景。
- Ubuntu 系统优化目前只覆盖基础调优和常见问题，还没有涉及性能基准测试、内核参数深度调优或生产环境加固最佳实践。
- macOS 侧现在已覆盖安装 U 盘制作、广义命令速查、开发环境配置和一份轻量级系统设置备忘，但仍缺少 Apple silicon / Intel 启动差异、`launchctl` domain model、APFS / Disk Utility、Homebrew 深度用法、SIP/TCC、Time Machine、系统恢复和数据迁移等专题。
- macOS 交互层目前只覆盖输入法、触控板和一个 Chrome 预加载相关案例，还没有形成窗口管理、Mission Control、辅助功能、快捷键治理和系统级浏览器排障的完整专题。
- Windows 侧现在已覆盖 Inno Setup、WMI/机器码速记，以及基础的 Win10 更新抑制/防火墙放行操作，但仍缺少注册表、PowerShell 自动化、组策略/WSUS、服务控制管理器、代码签名、UAC、事件日志与崩溃诊断等专题。
- 缺少把研发思维落到更多真实业务案例上的练习库、评审模板和分层设计示例。
- 虽然已补上分布式系统总览，但仍缺少 etcd / Raft 实现细节、Kafka ISR / controller、Redis Cluster、Cassandra / Dynamo、Spanner / TrueTime 等具体系统专题。
- 仍缺少分布式事务 / Outbox / Saga、CRDT、CDC / event sourcing、Jepsen 式故障验证和时钟同步 / HLC 深入材料。
- 刚补上 AI 时代的全栈入门全景，但仍缺少 Git/PR、Docker Compose、多容器本地开发、PostgreSQL/Redis、Go/Gin/GORM/wire、React/TypeScript/Tailwind/shadcn、Nginx 部署、`wrk` 和 Playwright 的独立专题页。
- 虽然已补上测试架构总览，但仍缺少团队级测试规范、缺陷分级到回归用例的沉淀模板，以及 Contract Test / 属性测试的落地案例库。
- 虽然已补上 USE 方法与主机观测链路，但仍缺少 Prometheus、Grafana、node_exporter、ELK/Loki/Jaeger 等工具的独立专题页和指标命名规范。


---

## Related Pages

- [[index]] — full catalog of all wiki pages
- [[glossary]] — terminology and style conventions
- [[2025-technical-line-summary]] — current primary source summary
- [[engineering-thinking-framework]] — engineering-thinking source summary
- [[ai-full-stack-development-onboarding]] — AI-era full-stack onboarding source summary
- [[moshi-neural-audio-codec-architecture-analysis]] — speech-native LLM source summary
- [[ai-era-full-stack-beginner]] — learner persona for AI-assisted full-stack onboarding
- [[growing-engineer]] — learner persona focused on system-thinking growth
- [[technical-line-leader]] — current primary persona
- [[full-lifecycle-delivery-capability]] — key delivery concept
- [[ai-enabled-software-delivery]] — AI transition concept
- [[model-context-protocol]] — protocol layer for AI tool/data access
- [[project-steering-context]] — persistent project context for AI collaboration
- [[devops-delivery-pipeline]] — delivery-loop concept connecting development through runtime feedback
- [[software-testing-architecture]] — layered testing model across fast and slow feedback loops
- [[property-based-testing]] — invariant-driven testing method for covering input spaces
- [[engineering-mindset]] — foundational engineering-thinking concept
- [[state-and-data-flow-modeling]] — state/data-flow system-modeling concept
- [[distributed-systems-foundations]] — distributed-systems boundary concept
- [[consistency-models]] — consistency spectrum and selection concept
- [[distributed-consensus]] — multi-node agreement concept
- [[data-replication-and-partitioning]] — replicas, shards, and migration trade-off concept
- [[logical-time-and-causality]] — ordering concept beyond physical clocks
- [[distributed-systems-resilience-patterns]] — resilience mechanisms for failure handling
- [[validation-driven-design]] — design-time validation concept
- [[unix-philosophy-and-pipeline-thinking]] — Unix-style design principles behind Linux and composable tooling
- [[platform-foundation]] — platform reuse and governance concept
- [[observability-and-reliability]] — stability and operations concept
- [[use-methodology]] — resource-first observability and troubleshooting frame
- [[moshi]] — speech-native LLM product page
- [[mimi]] — audio tokenizer product page
- [[speech-native-llm]] — speech-first model architecture
- [[neural-audio-codec]] — codec/tokenizer infrastructure
- [[full-duplex-speech-interaction]] — real-time duplex interaction model
- [[bash-syntax-and-scripting-reference]] — Bash source summary and scripting reference
- [[bash]] — shell and scripting tool page
- [[shell-scripting]] — automation concept behind command-line scripts
- [[linux-foundations-and-testing-special-topic]] — Linux and testing training source summary
- [[distributed-systems-core-principles]] — distributed-systems source summary
- [[linux-common-commands-reference]] — Linux commands source summary and operations reference
- [[centos7-offline-docker-install-troubleshooting]] — CentOS 7 Docker troubleshooting source summary
- [[centos7-kernel-upgrade-via-elrepo]] — CentOS 7 kernel upgrade source summary
- [[centos7-system-parameter-tuning]] — CentOS 7 system tuning source summary
- [[centos7-os-initialization-workflow]] — CentOS 7 OS initialization source summary
- [[centos7-openssl-and-openssh-upgrade-from-source]] — CentOS 7 OpenSSL/OpenSSH source upgrade summary
- [[ubuntu2204-openssh-upgrade-from-source]] — Ubuntu 22.04 OpenSSH source upgrade summary
- [[centos7-samba-share-setup]] — CentOS 7 Samba share setup source summary
- [[centos6-archive-repository-workaround]] — CentOS 6 archive mirror recovery source summary
- [[ubuntu2004-docker-dev-environment-setup]] — Ubuntu 20.04 Docker development environment setup source summary
- [[centos]] — CentOS distribution page
- [[docker]] — Docker runtime/tooling page
- [[linux]] — Linux platform/tool page
- [[openssl]] — TLS/crypto library page
- [[openssh]] — SSH client/server suite page
- [[linux-command-line-operations]] — operations concept behind Linux command usage
- [[file-descriptor-and-tcp-backlog-tuning]] — concept page for layered descriptor-limit and TCP backlog tuning
- [[source-built-package-replacement]] — concept page for source-compiled software taking over system paths and services
- [[container-network-namespace-support]] — host-kernel support concept behind Docker bridge networking
- [[containerized-development-environment]] — concept page for isolated, reproducible development environments using containers
- [[ubuntu]] — Ubuntu distribution page
- [[netplan-configuration-guide]] — Ubuntu Netplan configuration source summary
- [[ubuntu-common-issues-and-optimization]] — Ubuntu troubleshooting and optimization source summary
- [[ubuntu-kernel-version-switching]] — Ubuntu kernel version switching runbook
- [[network-configuration]] — declarative network configuration concept
- [[systemd-resolved-dns-management]] — DNS resolver management concept
- [[kernel-upgrade-and-boot-management]] — concept page for installing kernels and managing default boot entries
- [[legacy-repository-repointing]] — archive/vault repo recovery concept for legacy systems
- [[os-initialization-workflow]] — concept page for structured OS initialization from bare metal to usable baseline
- [[macos-common-commands-reference]] — macOS command cheat sheet source summary
- [[macos-system-settings]] — macOS workstation settings source summary
- [[macos-usb-installer-creation]] — macOS bootable USB installer source summary
- [[macos]] — macOS desktop operating system page
- [[macos-command-line-operations]] — reusable macOS operations concept
- [[windows-development-related]] — Windows-native development source summary
- [[windows-system-settings]] — Windows workstation administration source summary
- [[windows]] — Windows product page
- [[inno-setup]] — Windows installer tool page
- [[windows-management-instrumentation]] — Windows metadata and hardware-query concept
- [[hardware-derived-machine-identifier]] — hardware-based device identifier concept
- [[bootable-os-installer-media]] — concept page for creating external installation media from full installers
- [[startup-security-and-external-boot-policy]] — concept page for recovery-time authorization of external boot paths
- [[samba]] — Samba product/tool page
- [[smb-file-sharing]] — concept page for SMB-based cross-platform directory sharing
- [[vim-usage-and-configuration-reference]] — Vim source summary and command reference
- [[vim]] — modal editor tool page
- [[modal-editing]] — editing model behind Vim command composition
- [[macos-development-environment-setup]] — macOS Ruby/CocoaPods development environment setup source summary
- [[language-runtime-version-management]] — concept page for version manager patterns across language ecosystems
