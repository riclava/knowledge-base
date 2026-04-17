---
title: Glossary
type: glossary
created: 2026-04-07
updated: 2026-04-17
sources: [2025年技术线总结.md, Moshi 与神经音频编码（Neural Audio Codec）技术架构解析.md, vim.md, bash.md, commands.md, CentOS6由于镜像废弃无法使用的解决办法.md, CentOS7离线安装docker问题排查.md, CentOS7配置Samba共享.md, CentOS7升级内核.md, CentOS7升级OpenSSL和OpenSSH.md, CentOS7系统参数调优.md, CentOS操作系统初始化流程.md, 基于docker构建ubuntu20.04开发环境.md, netplan配置指南.md, Ubuntu22.04升级OpenSSH版本到最新.md, Ubuntu常见问题与优化.md, 构建技术研发思维.md, 快速基于 AI 入门全栈研发.md, Linux基础与测试专题.md, macOS安装U盘制作.md, macOS常用命令.md, macOS开发环境配置.md, macOS系统设置.md, Windows开发相关.md, Windows系统设置.md]
tags: [terminology, style, glossary, ai, engineering-management, speech-llm, developer-tooling, engineering-thinking, systems-thinking, abstraction, modeling, validation, linux, unix, command-line, observability, testing, quality, vim, bash, shell, centos, ubuntu, yum, repository, elrepo, grub, bootloader, docker, containers, kernel, networking, netplan, yaml, vlan, bonding, bridging, samba, smb, file-sharing, windows, windows-update, task-scheduler, powershell, firewall, inno-setup, wmi, mfc, win32, installer, machine-identifier, selinux, openssl, openssh, ssh, tls, source-build, sysctl, systemd, tuning, file-descriptors, tcp, initialization, post-install, ntp, chrony, epel, development-environment, apt-mirror, pam, systemd-resolved, dns, swap, nfs, multipath, mcp, steering, devops, ci-cd, full-stack, macos, apple, recovery, createinstallmedia, bootable-media, startup-security, external-boot, nvram, smc, homebrew, launchctl, pmset, diskutil, defaults, xcode, ruby, rbenv, cocoapods, gem, version-management, system-settings, input-method, trackpad, chrome, spotlight, browser-troubleshooting]
---

# Glossary

Living reference of terms, definitions, and style conventions. The LLM checks this before using any technical term. Updated on every ingest that introduces new or refined terminology.

---

## How to Read This Glossary

Each entry follows this format:

**Term** *(canonical form)*
: Definition. Usage notes. Related terms.
- Preferred: `term` / Avoid: `deprecated term`
- See also: `\[\[related-page\]\]`

---

## Terminology

**AI辅助开发** *(canonical form)*
: 指在编码、调试、测试、运维等研发活动中引入 AI 协同，以提升局部效率和交付速度。
- Preferred: `AI辅助开发` / Avoid: `只把 AI 当成代码补全工具`
- See also: [[ai-enabled-software-delivery]]

**AI融合** *(canonical form)*
: 指把 AI 能力嵌入现有项目或业务功能中，形成用户可见、可使用、可衡量的功能结果。
- Preferred: `AI融合` / Avoid: `AI 试点` when the work is already part of正式项目交付
- See also: [[ai-enabled-software-delivery]]

**AI IDE** *(canonical form)*
: 指把 AI 助手深度集成进编辑器工作流的开发环境形态，强调边写边问、边改边测。
- Preferred: `AI IDE`
- See also: [[ai-enabled-software-delivery]], [[ai-full-stack-development-onboarding]]

**CLI Agent** *(canonical form)*
: 指运行在终端中的 AI 代理形态，能够直接面向代码库、命令行和脚本任务进行协作。
- Preferred: `CLI Agent`
- See also: [[ai-enabled-software-delivery]], [[ai-full-stack-development-onboarding]]

**完整交付能力** *(canonical form)*
: 指覆盖需求、设计、开发、测试、发布和运维的端到端软件产品交付能力。
- Preferred: `完整交付能力` / Avoid: `只强调开发能力`
- See also: [[full-lifecycle-delivery-capability]]

**底座** *(canonical form)*
: 指支撑复用、治理和 AI 落地的平台基础能力集合，可写作“平台底座”。
- Preferred: `底座` or `平台底座` / Avoid: `公共部分` when referring to strategic shared capability
- See also: [[platform-foundation]]

**可观测性** *(canonical form)*
: 通过日志、指标和 Trace 理解系统运行状态、定位问题和支撑治理的能力。
- Preferred: `可观测性` / Avoid: `只说监控` when logs, metrics, and traces are all involved
- See also: [[observability-and-reliability]]

**LLMOps** *(canonical form)*
: 面向大语言模型应用的工程化运营能力，通常涉及模型接入、评估、运行、监控和治理。
- Preferred: `LLMOps`
- See also: [[ai-enabled-software-delivery]], [[platform-foundation]]

**MCP（Model Context Protocol）** *(canonical form)*
: 让 AI 模型连接外部工具和数据源的协议层，用于把模型能力扩展到文件、数据库、浏览器、Git 等操作表面。
- Preferred: `MCP` or `Model Context Protocol` / Avoid: 只说“AI 插件” when the protocol boundary matters
- See also: [[model-context-protocol]], [[ai-enabled-software-delivery]]

**Steering 文件** *(canonical form)*
: 用于保存项目技术栈、团队规范和业务背景的持久化上下文文件，帮助 AI 在长期协作中理解仓库约束。
- Preferred: `Steering 文件` or `Steering` / Avoid: 把长期项目上下文和一次性 Prompt 混为一谈
- See also: [[project-steering-context]], [[ai-enabled-software-delivery]]

**DevOps** *(canonical form)*
: 指把开发、测试、构建、部署、运行反馈和协作流程串成持续交付闭环的方法与实践视角。
- Preferred: `DevOps`
- See also: [[devops-delivery-pipeline]], [[full-lifecycle-delivery-capability]]

**CI/CD 流水线** *(canonical form)*
: 指从代码提交后的自动检查、构建、发布到部署的连续流程，用于把质量门禁和交付动作标准化。
- Preferred: `CI/CD 流水线` or `CI/CD pipeline`
- See also: [[devops-delivery-pipeline]], [[full-lifecycle-delivery-capability]]

**全栈研发** *(canonical form)*
: 在当前知识库中，优先指能够把需求、前后端实现、部署、测试和运行反馈串成一条最小交付闭环，而不只是“同时会写前端和后端代码”。
- Preferred: `全栈研发` / Avoid: 把它缩减为“会两门语言”
- See also: [[full-lifecycle-delivery-capability]], [[ai-era-full-stack-beginner]]

**烟囱式系统** *(canonical form)*
: 指重复建设、彼此割裂、复用性差的系统形态，是平台化建设试图减少的反模式。
- Preferred: `烟囱式系统`
- See also: [[platform-foundation]]

**鸿蒙移动端适配** *(canonical form)*
: 指面向鸿蒙生态进行移动端适配的工作方向。在当前来源中，它被标记为尚未形成理想落地结果。
- Preferred: `鸿蒙移动端适配`
- See also: [[2025-technical-line-summary]]

**Linux** *(canonical form)*
: 以命令行、小工具组合和文本接口著称的 Unix-like 操作系统家族；在当前知识库中，它既是 [[bash]] 和 [[vim]] 的运行环境，也是系统巡检与运维操作的对象。
- Preferred: `Linux`
- See also: [[linux]], [[linux-command-line-operations]], [[bash]]

**信创 Linux** *(canonical form)*
: 指面向信创环境的 Linux 适配与落地工作。在当前来源中，四川和重庆区域尚未形成较好落地。
- Preferred: `信创 Linux`
- See also: [[2025-technical-line-summary]]

**CentOS** *(canonical form)*
: 基于 RPM/YUM 生态的企业级 Linux 发行版家族；在当前知识库中，既出现在遗留版本维护和仓库恢复场景，也出现在通过内核升级修补宿主机兼容性的运维场景。
- Preferred: `CentOS`
- See also: [[centos]], [[linux]]

**源码编译安装（source build）** *(canonical form)*
: 指直接从上游源码包编译并安装软件，而不是只通过发行版包管理器获取二进制包；在当前来源中，它用于在 CentOS 7 上手工升级 OpenSSL 和 OpenSSH，并带来服务切换和回滚复杂度。
- Preferred: `源码编译安装` or `source build` / Avoid: 把它和常规 `yum` / `apt` 升级混为一谈
- See also: [[source-built-package-replacement]], [[centos]], [[linux]]

**ELRepo** *(canonical form)*
: 面向 Enterprise Linux 的第三方软件仓库，在当前来源中用于为 CentOS 7 提供 `kernel-lt`、`kernel-ml` 等替代内核分支。
- Preferred: `ELRepo`
- See also: [[centos]], [[centos7-kernel-upgrade-via-elrepo]], [[kernel-upgrade-and-boot-management]]

**Samba** *(canonical form)*
: Linux/Unix 平台上实现 SMB/CIFS 文件共享的服务套件；在当前来源中，它用于把 CentOS 7 上的 `/data` 暴露给 Windows 客户端。
- Preferred: `Samba`
- See also: [[samba]], [[smb-file-sharing]], [[centos]]

**SMB/CIFS** *(canonical form)*
: 一类常见于 Windows 网络共享与映射网络驱动器场景的文件共享协议；`Samba` 是 Linux 侧的常见实现，而不是协议本身。
- Preferred: `SMB` or `SMB/CIFS` / Avoid: 在需要区分协议与实现时把 `Samba` 当成协议名
- See also: [[smb-file-sharing]], [[samba]]

**smb.conf** *(canonical form)*
: Samba 服务的主配置文件，通常指 `/etc/samba/smb.conf`，用于声明全局设置和具体共享段。
- Preferred: `smb.conf` or `/etc/samba/smb.conf`
- See also: [[samba]], [[smb-file-sharing]], [[centos7-samba-share-setup]]

**smbpasswd** *(canonical form)*
: 用于为用户创建或更新 Samba 认证密码的命令；在当前来源中，它说明 Linux 本地用户和 Samba 登录凭据并不是一回事。
- Preferred: `smbpasswd`
- See also: [[samba]], [[smb-file-sharing]], [[centos7-samba-share-setup]]

**Docker** *(canonical form)*
: 一种常见的容器运行时与镜像工具链；在当前知识库中，它既出现在 CentOS 7 离线安装后仍发生 bridge 网络异常的排障场景，也出现在基于容器快速构建隔离开发环境的场景。
- Preferred: `Docker`
- See also: [[docker]], [[centos]], [[container-network-namespace-support]], [[containerized-development-environment]]

**Docker Compose** *(canonical form)*
: 用于定义和启动多容器应用组合的 Docker 编排工具；在当前来源中被用作全栈教学栈里的中间件组装方式。
- Preferred: `Docker Compose` or `docker compose` / Avoid: 把它和单容器 `Docker` 运行时混为一谈
- See also: [[docker]], [[ai-full-stack-development-onboarding]]

**Playwright** *(canonical form)*
: 用于浏览器自动化和端到端流程测试的工具；在当前来源里被列为自动化流程测试方案，原文写作 `playwrite` 属于拼写不规范。
- Preferred: `Playwright` / Avoid: `playwrite`
- See also: [[ai-full-stack-development-onboarding]], [[devops-delivery-pipeline]]

**容器化开发环境（containerized development environment）** *(canonical form)*
: 指使用容器技术创建隔离、可复现的开发工作环境，使开发者可以在不污染宿主机的前提下获得一致的工具链和系统配置。
- Preferred: `容器化开发环境` or `containerized development environment`
- See also: [[containerized-development-environment]], [[docker]]

**sleep infinity** *(canonical form)*
: 一种常见的容器保活技巧，将 `sleep infinity` 作为容器主进程，使容器可以后台持续运行而不会因为没有前台任务而退出。
- Preferred: `sleep infinity`
- See also: [[containerized-development-environment]], [[docker]]

**APT 镜像源** *(canonical form)*
: Debian/Ubuntu 系发行版的软件包仓库地址配置；在当前来源中，它被替换为 USTC 镜像以加速中国大陆网络环境下的包下载。
- Preferred: `APT 镜像源` or `APT mirror`
- See also: [[ubuntu2004-docker-dev-environment-setup]], [[ubuntu]], [[linux]]

**Ubuntu** *(canonical form)*
: 基于 Debian 的 Linux 发行版，以 APT 包管理和 Netplan 网络配置为特征；在当前知识库中，它既出现在容器化开发环境场景，也出现在 Netplan 网络配置场景。
- Preferred: `Ubuntu`
- See also: [[ubuntu]], [[linux]], [[netplan-configuration-guide]]

**Netplan** *(canonical form)*
: Ubuntu 17.10+ 默认的网络配置工具，使用 YAML 格式配置文件，后端可以是 `systemd-networkd` 或 `NetworkManager`。
- Preferred: `Netplan`
- See also: [[netplan-configuration-guide]], [[ubuntu]], [[network-configuration]]

**systemd-networkd** *(canonical form)*
: systemd 提供的网络管理守护进程，是 Ubuntu Server 上 Netplan 的默认后端；负责根据 Netplan 生成的配置实际管理网络接口。
- Preferred: `systemd-networkd`
- See also: [[netplan-configuration-guide]], [[ubuntu]], [[network-configuration]]

**NetworkManager** *(canonical form)*
: Linux 上常见的网络管理服务，是 Ubuntu Desktop 上 Netplan 的默认后端；也可通过 `nmcli` 命令行工具直接操作。
- Preferred: `NetworkManager`
- See also: [[netplan-configuration-guide]], [[ubuntu]], [[network-configuration]]

**VLAN** *(canonical form)*
: Virtual LAN，在物理网络基础设施上划分逻辑网络的技术；在 Netplan 中通过 `vlans` 段配置，需要指定 VLAN ID 和父接口。
- Preferred: `VLAN`
- See also: [[netplan-configuration-guide]], [[network-configuration]]

**Bond（链路聚合）** *(canonical form)*
: 将多个物理网卡组合成一个逻辑接口的技术，用于提高带宽或实现冗余；在 Netplan 中通过 `bonds` 段配置，支持 `active-backup`、`802.3ad` 等多种模式。
- Preferred: `Bond` or `链路聚合` / Avoid: 把 Bond 和 Bridge 混淆
- See also: [[netplan-configuration-guide]], [[network-configuration]]

**Bridge（网桥）** *(canonical form)*
: 在二层连接多个网络接口的虚拟设备，常用于虚拟机和容器场景；在 Netplan 中通过 `bridges` 段配置，可启用 STP 防止环路。
- Preferred: `Bridge` or `网桥`
- See also: [[netplan-configuration-guide]], [[network-configuration]], [[container-network-namespace-support]]

**netplan try** *(canonical form)*
: Netplan 的安全测试命令，应用配置后等待 120 秒，如果用户不确认则自动回滚；是远程配置网络时避免锁死的关键手段。
- Preferred: `netplan try`
- See also: [[netplan-configuration-guide]], [[network-configuration]]

**ifupdown** *(canonical form)*
: Debian/Ubuntu 传统的网络配置系统，使用 `/etc/network/interfaces` 文件；在 Ubuntu 17.10+ 中被 Netplan 取代，但仍可能在旧系统或迁移场景中遇到。
- Preferred: `ifupdown` / Avoid: 在 Ubuntu 18.04+ 文档中把它当作默认配置方式
- See also: [[netplan-configuration-guide]], [[ubuntu]]

**OpenSSL** *(canonical form)*
: 常见的 TLS/加密库与命令行工具；在当前来源中，它被源码安装到 `/usr/local/openssl`，并作为 OpenSSH 重编译时的依赖提供者。
- Preferred: `OpenSSL`
- See also: [[openssl]], [[openssh]], [[source-built-package-replacement]]

**OpenSSH** *(canonical form)*
: 常见的 SSH 客户端与服务端套件；在当前来源中，它通过 portable 源码重编译，并与目标 OpenSSL 路径一起完成 `sshd` 服务切换，覆盖 CentOS 7（依赖自建 OpenSSL）和 Ubuntu 22.04（使用系统 OpenSSL）两种路径。
- Preferred: `OpenSSH`
- See also: [[openssh]], [[openssl]], [[source-built-package-replacement]]

**--without-hardening** *(canonical form)*
: OpenSSH 编译选项，用于禁用某些编译器安全加固特性；在 Ubuntu 22.04 来源中使用，可能是为了避免与系统编译器或库的兼容性问题。
- Preferred: `--without-hardening`
- See also: [[openssh]], [[ubuntu2204-openssh-upgrade-from-source]]

**TCP Wrappers** *(canonical form)*
: 一种传统的主机访问控制机制，通过 `/etc/hosts.allow` 和 `/etc/hosts.deny` 过滤网络连接；在新版 OpenSSH 和 Ubuntu 22.04 中已不再支持。
- Preferred: `TCP Wrappers` / Avoid: 在新系统文档中把它当作可用选项
- See also: [[openssh]], [[ubuntu2204-openssh-upgrade-from-source]]

**libpam0g-dev** *(canonical form)*
: Ubuntu/Debian 上的 PAM 开发包，提供编译需要 PAM 支持的软件（如 OpenSSH）所需的头文件和库。
- Preferred: `libpam0g-dev`
- See also: [[ubuntu]], [[openssh]], [[ubuntu2204-openssh-upgrade-from-source]]

**systemd-resolved** *(canonical form)*
: systemd 提供的 DNS 解析服务，在 Ubuntu 18.04+ 上默认启用，提供本地 DNS 缓存、DNSSEC 验证和 DNS stub listener（127.0.0.53:53）。
- Preferred: `systemd-resolved`
- See also: [[systemd-resolved-dns-management]], [[ubuntu]], [[network-configuration]]

**DNSStubListener** *(canonical form)*
: `systemd-resolved` 配置选项，控制是否在 127.0.0.53:53 上启用本地 DNS stub listener；设为 `no` 可释放 53 端口供其他 DNS 服务使用。
- Preferred: `DNSStubListener`
- See also: [[systemd-resolved-dns-management]], [[ubuntu-common-issues-and-optimization]]

**multipath** *(canonical form)*
: Linux 上用于管理多路径存储设备的工具和服务；在 VMware 虚拟机中，需要将虚拟磁盘加入黑名单以避免无意义的错误日志。
- Preferred: `multipath` or `multipath-tools`
- See also: [[ubuntu-common-issues-and-optimization]], [[linux]]

**swap** *(canonical form)*
: Linux 上用于内存不足时将内存页交换到磁盘的机制；在 Kubernetes 节点或内存敏感场景中通常需要禁用。
- Preferred: `swap`
- See also: [[ubuntu-common-issues-and-optimization]], [[linux]]

**_netdev** *(canonical form)*
: `/etc/fstab` 挂载选项，指示系统在网络可用后再尝试挂载该文件系统；对 NFS 等网络文件系统至关重要。
- Preferred: `_netdev`
- See also: [[ubuntu-common-issues-and-optimization]], [[linux]]

**GRUB2** *(canonical form)*
: CentOS 7 等 Linux 系统中常见的启动加载器；在当前来源中，它决定已安装的多个内核中哪个会作为默认启动项，并通过 `grub2-set-default`、`/etc/default/grub` 与 `grub2-mkconfig` 管理。
- Preferred: `GRUB2` or `GRUB` when version distinction is not important
- See also: [[kernel-upgrade-and-boot-management]], [[centos7-kernel-upgrade-via-elrepo]], [[ubuntu-kernel-version-switching]], [[centos]], [[ubuntu]]

**update-grub** *(canonical form)*
: Ubuntu 上用于重新生成 GRUB 配置的命令，等效于 CentOS 上的 `grub2-mkconfig -o /boot/grub2/grub.cfg`；在切换默认内核后必须执行以使配置生效。
- Preferred: `update-grub`
- See also: [[kernel-upgrade-and-boot-management]], [[ubuntu-kernel-version-switching]], [[ubuntu]]

**linux-image / linux-headers / linux-modules** *(canonical form)*
: Ubuntu/Debian 系发行版的内核包命名约定；`linux-image-*` 是内核本体，`linux-headers-*` 是编译模块所需的头文件，`linux-modules-*` 和 `linux-modules-extra-*` 是内核模块。
- Preferred: `linux-image-*`, `linux-headers-*`, `linux-modules-*`
- See also: [[ubuntu-kernel-version-switching]], [[ubuntu]], [[kernel-upgrade-and-boot-management]]

**离线安装（offline installation）** *(canonical form)*
: 指目标机器无法直接联网安装时，先在其他可联网环境下载 RPM 包或依赖，再转移到目标机器完成安装的做法。
- Preferred: `离线安装` or `offline installation` / Avoid: `手工安装` when the key constraint is lack of network access
- See also: [[docker]], [[centos7-offline-docker-install-troubleshooting]]

**YUM** *(canonical form)*
: CentOS/RHEL 家族常见的软件包管理与仓库访问工具，负责读取 repo 配置、获取元数据并安装 RPM 包。
- Preferred: `YUM` or `yum`
- See also: [[centos]], [[legacy-repository-repointing]]

**repo 文件** *(canonical form)*
: 指 `/etc/yum.repos.d/*.repo` 这类仓库配置文件，用来声明 `mirrorlist`、`baseurl`、`gpgcheck`、`gpgkey` 等字段。
- Preferred: `repo 文件` or `仓库配置文件`
- See also: [[centos]], [[legacy-repository-repointing]]

**mirrorlist / baseurl** *(canonical form)*
: `YUM` 仓库中两类常见的软件源定位方式；`mirrorlist` 倾向动态获取镜像列表，`baseurl` 倾向直接指定固定仓库地址。
- Preferred: `mirrorlist` / `baseurl`
- See also: [[centos]], [[legacy-repository-repointing]]

**vault 仓库** *(canonical form)*
: 指为历史版本保留包文件的 archive/vault 型静态仓库，常用于老版本发行版在默认镜像退役后的临时维护。
- Preferred: `vault 仓库` or `archive/vault 仓库`
- See also: [[legacy-repository-repointing]], [[centos]]

**fastestmirror** *(canonical form)*
: `YUM` 的镜像选择插件，用于从可用镜像中挑选更快的源；当发行版镜像已退役时，可能需要临时关闭。
- Preferred: `fastestmirror`
- See also: [[centos6-archive-repository-workaround]], [[legacy-repository-repointing]]

**kernel-lt / kernel-ml** *(canonical form)*
: ELRepo 提供的两类常见内核包分支；`kernel-lt` 指长期维护分支，`kernel-ml` 指较新的主线稳定分支。
- Preferred: `kernel-lt` / `kernel-ml`
- See also: [[centos7-kernel-upgrade-via-elrepo]], [[kernel-upgrade-and-boot-management]], [[centos]]

**network namespace** *(canonical form)*
: Linux 内核用来隔离网络接口、路由、地址和相关网络视图的机制；在当前来源中，它是理解 Docker bridge 网络和 `veth` 关联关系的关键术语。
- Preferred: `network namespace` or `网络命名空间`
- See also: [[container-network-namespace-support]], [[docker]], [[linux]]

**docker0 / veth** *(canonical form)*
: `docker0` 是 Docker 默认 bridge 接口，`veth` 是连接宿主机 bridge 与容器网络命名空间的虚拟网卡对；它们的 `ip a` 输出是排查容器网络问题的重要观察面。
- Preferred: `docker0` / `veth`
- See also: [[docker]], [[container-network-namespace-support]]

**link-netnsid** *(canonical form)*
: `ip a` 输出中的一个标记，用于显示某个网络接口与 network namespace ID 的关联；在当前来源中，它的缺失与 `RTM_GETNSID is not supported by the kernel` 一同指向宿主机兼容性问题。
- Preferred: `link-netnsid`
- See also: [[container-network-namespace-support]], [[centos7-offline-docker-install-troubleshooting]]

**iptables** *(canonical form)*
: Linux 上常见的包过滤与 NAT 规则管理工具；在当前来源中，它被作为排除法步骤清空，但并不是最终根因。
- Preferred: `iptables`
- See also: [[linux-command-line-operations]], [[docker]]

**ldconfig** *(canonical form)*
: Linux 上用于刷新动态链接器缓存的命令；在当前来源中，它用于让系统识别新加入的 OpenSSL 共享库路径。
- Preferred: `ldconfig`
- See also: [[openssl]], [[source-built-package-replacement]], [[linux]]

**sshd_config** *(canonical form)*
: OpenSSH 服务端的主配置文件，通常指 `/etc/ssh/sshd_config`，用于控制 root 登录、密码认证、监听端口等服务行为。
- Preferred: `sshd_config` or `/etc/ssh/sshd_config`
- See also: [[openssh]], [[centos7-openssl-and-openssh-upgrade-from-source]]

**rpm -e --nodeps** *(canonical form)*
: 在 RPM 系统上强制卸载包且跳过依赖检查的做法；在当前来源中，它被用于移除旧 OpenSSH 包，但属于高风险维护动作，不应当作常规卸载手段。
- Preferred: `rpm -e --nodeps` / Avoid: 把它写成安全默认操作
- See also: [[source-built-package-replacement]], [[openssh]], [[centos]]

**speech-native LLM** *(canonical form)*
: 指直接对语音 token 进行建模、推理与生成的大模型，内部可保留文本流，但不再依赖 `ASR -> LLM -> TTS` 串行级联作为主路径。
- Preferred: `speech-native LLM` or `语音原生大模型` / Avoid: `只是接了语音接口的文本 LLM`
- See also: [[speech-native-llm]], [[moshi]]

**全双工对话** *(canonical form)*
: 指系统可以边听边说、支持打断与语音重叠的实时对话模式，区别于严格轮次式交互。
- Preferred: `全双工对话` / Avoid: `实时语音` when the key point is simultaneous listening and speaking
- See also: [[full-duplex-speech-interaction]], [[moshi]]

**Neural Audio Codec** *(canonical form)*
: 指把音频压缩并离散化为 token 的神经编解码技术。在语音 LLM 语境中，它承担类似文本 tokenizer 的基础设施角色。
- Preferred: `Neural Audio Codec` or `神经音频编码`
- See also: [[neural-audio-codec]], [[mimi]]

**语音 LLM tokenizer** *(canonical form)*
: 指可输出低频离散 token，并同时保留语义与声学信息、适合接入语音大模型的音频 tokenizer。
- Preferred: `语音 LLM tokenizer`
- See also: [[neural-audio-codec]], [[mimi]]

**Moshi** *(canonical form)*
: 代表语音原生、全双工、低延迟对话路线的 speech-native LLM 产品/模型名称。
- Preferred: `Moshi`
- See also: [[moshi]], [[speech-native-llm]]

**Mimi** *(canonical form)*
: Moshi 路线中的 Neural Audio Codec + Tokenizer，用于把 24kHz 音频编码为低频离散 token。
- Preferred: `Mimi`
- See also: [[mimi]], [[neural-audio-codec]]

**inner monologue** *(canonical form)*
: 指语音原生模型内部保留的文本推理流，用于逻辑组织和语义规划，不直接等同于最终输出语音。
- Preferred: `inner monologue`
- See also: [[moshi]], [[speech-native-llm]]

**Temporal Transformer** *(canonical form)*
: 在 Moshi 架构中负责时间维建模、语义理解和对话逻辑的主模型层。
- Preferred: `Temporal Transformer`
- See also: [[moshi]]

**Depth Transformer** *(canonical form)*
: 在 Moshi 架构中负责音频 token 细粒度声学关系建模的模型层。
- Preferred: `Depth Transformer`
- See also: [[moshi]]

**残差向量量化（RVQ）** *(canonical form)*
: 一种常见的音频离散化与压缩方法，EnCodec 使用该路线；在当前来源中，RVQ 是理解 codec 演化谱系的基础术语。
- Preferred: `RVQ` or `残差向量量化`
- See also: [[neural-audio-codec]]

**轮次式交互（turn-based）** *(canonical form)*
: 指系统遵循“用户说完一轮，模型再回复一轮”的串行交互方式，通常不支持边听边说和自然打断。
- Preferred: `轮次式交互` or `turn-based`
- See also: [[full-duplex-speech-interaction]]

**EnCodec** *(canonical form)*
: Meta 提出的通用 Neural Audio Codec baseline，在当前来源中被视为 Mimi 的重要前身之一。
- Preferred: `EnCodec`
- See also: [[neural-audio-codec]], [[mimi]]

**SNAC** *(canonical form)*
: 一种强调多尺度量化的新一代 Neural Audio Codec 路线，在当前来源中被视为值得跟踪的前沿探索方向。
- Preferred: `SNAC`
- See also: [[neural-audio-codec]], [[mimi]]

**SpeechTokenizer / SemanticCodec** *(canonical form)*
: 指更强调语义表达的 codec/tokenizer 路线，在当前来源中因 token 率较高而被认为不利于直接接入长序列语音 LLM。
- Preferred: `SpeechTokenizer` or `SemanticCodec`
- See also: [[neural-audio-codec]]

**SimWhisper-Codec** *(canonical form)*
: 基于 Whisper encoder 的语义优先 codec 路线，在当前来源中更适合语音理解类任务。
- Preferred: `SimWhisper-Codec`
- See also: [[neural-audio-codec]]

**Lyra / Satin** *(canonical form)*
: 主要面向通信压缩的 codec 路线，在当前来源中因不输出离散 token 而不被归入语音 LLM tokenizer。
- Preferred: `Lyra` / `Satin`
- See also: [[neural-audio-codec]]

**Vim** *(canonical form)*
: 一个以模态编辑和组合式命令为核心的文本编辑器，常用于 Linux/终端环境中的代码、脚本和配置文件修改。
- Preferred: `Vim`
- See also: [[vim]], [[modal-editing]], [[vim-usage-and-configuration-reference]]

**vimrc** *(canonical form)*
: 用户的 Vim 主配置文件，通常指 `~/.vimrc`，用于设置界面、搜索、缩进、映射和插件加载行为。
- Preferred: `vimrc` or `~/.vimrc` / Avoid: 只写“配置文件”而不指明 Vim 语境
- See also: [[vim]], [[vim-usage-and-configuration-reference]]

**模态编辑（modal editing）** *(canonical form)*
: 一种根据当前模式切换按键语义的编辑模型。普通模式偏向导航和命令执行，插入模式偏向文本输入，可视模式偏向选择。
- Preferred: `模态编辑` or `modal editing`
- See also: [[modal-editing]], [[vim]]

**文本对象（text object）** *(canonical form)*
: 指可被操作符直接作用的一类结构化文本范围，例如引号内内容、括号内内容或连同包围符在内的整体区域。
- Preferred: `文本对象` or `text object`
- See also: [[modal-editing]], [[vim]]

**buffer** *(canonical form)*
: Vim 中已打开文件或文本内容的内存表示，可以脱离当前窗口独立存在，并在多个 buffer 之间切换。
- Preferred: `buffer`
- See also: [[vim]], [[vim-usage-and-configuration-reference]]

**vim-plug** *(canonical form)*
: 一个轻量级 Vim 插件管理器，用于声明、安装、更新和清理插件。
- Preferred: `vim-plug`
- See also: [[vim]], [[vim-usage-and-configuration-reference]]

**Bash** *(canonical form)*
: 一种常见于 Linux/Unix 环境的 shell 与脚本语言，既可交互执行命令，也可编排自动化脚本。
- Preferred: `Bash` / Avoid: 在需要区分语言或解释器时只泛写 `shell`
- See also: [[bash]], [[bash-syntax-and-scripting-reference]], [[shell-scripting]]

**shell scripting** *(canonical form)*
: 指使用 shell 内建能力、外部命令、管道和重定向编写自动化脚本的实践；在当前来源中主要指 Bash 脚本。
- Preferred: `shell scripting` or `Shell 脚本` / Avoid: 把所有 shell script 都默认视为可移植的 POSIX `sh`
- See also: [[shell-scripting]], [[bash]]

**参数展开（parameter expansion）** *(canonical form)*
: Bash 内置的变量展开与字符串处理机制，如默认值、截取、替换和前后缀删除，是减少外部文本处理命令的重要手段。
- Preferred: `参数展开` or `parameter expansion`
- See also: [[bash]], [[bash-syntax-and-scripting-reference]]

**Here Document** *(canonical form)*
: 用 `<<EOF` 形式向命令或文件提供多行输入的 shell 语法，可控制变量是否展开，并支持 `<<-EOF` 这种更易排版的缩进写法。
- Preferred: `Here Document` or `heredoc`
- See also: [[bash]], [[shell-scripting]]

**getopts** *(canonical form)*
: Bash 内建的短选项解析机制，适合为脚本建立规范命令行入口，并配合 `OPTARG`、`OPTIND` 处理参数消费。
- Preferred: `getopts`
- See also: [[bash]], [[shell-scripting]], [[bash-syntax-and-scripting-reference]]

**set -euo pipefail** *(canonical form)*
: Bash 中常见的防御性脚本组合，用于在命令失败、未定义变量或管道中间步骤失败时尽早暴露问题。
- Preferred: `set -euo pipefail`
- See also: [[bash]], [[shell-scripting]]

**rsync** *(canonical form)*
: 一种支持本地或远程目录同步的复制工具，强调保留元数据、增量传输和可视化进度，常用于替代简单递归复制。
- Preferred: `rsync`
- See also: [[linux]], [[linux-command-line-operations]], [[linux-common-commands-reference]]

**inode** *(canonical form)*
: 文件系统为文件或目录分配的元数据索引号；当文件名损坏、包含特殊字符或难以直接引用时，可以借助 inode 定位和删除目标。
- Preferred: `inode`
- See also: [[linux-command-line-operations]], [[linux-common-commands-reference]]

**grep / sed / awk** *(canonical form)*
: Linux/Unix 经典文本处理工具组，分别偏向过滤搜索、流式替换和按字段/条件处理，经常通过管道协同工作。
- Preferred: `grep` / `sed` / `awk`
- See also: [[linux-command-line-operations]], [[bash]], [[shell-scripting]]

**systemd** *(canonical form)*
: Linux 上常见的初始化与服务管理体系；在当前来源中，它既作为 `journalctl` 的日志体系出现，也作为通过 `LimitNOFILE` 覆盖服务资源限制的配置层出现。
- Preferred: `systemd`
- See also: [[linux]], [[linux-common-commands-reference]], [[file-descriptor-and-tcp-backlog-tuning]]

**limits.conf** *(canonical form)*
: Linux 上常见的登录会话资源限制配置文件，通常指 `/etc/security/limits.conf`；在当前来源中，它用于设置用户级 `soft/hard nofile` 上限。
- Preferred: `limits.conf` or `/etc/security/limits.conf`
- See also: [[file-descriptor-and-tcp-backlog-tuning]], [[centos7-system-parameter-tuning]], [[linux]]

**nofile** *(canonical form)*
: Linux 资源限制中的一个项目名，表示可打开文件描述符数量的上限；常以 `soft`/`hard` 形式出现在 `limits.conf` 或进程限制信息里。
- Preferred: `nofile`
- See also: [[file-descriptor-and-tcp-backlog-tuning]], [[centos7-system-parameter-tuning]], [[linux]]

**sysctl / sysctl.conf** *(canonical form)*
: Linux 内核运行时参数接口及其常见持久化配置文件；在当前来源中，它用于设置 `fs.file-max`、`net.core.somaxconn` 和 `net.ipv4.tcp_max_syn_backlog`。
- Preferred: `sysctl` / `/etc/sysctl.conf`
- See also: [[file-descriptor-and-tcp-backlog-tuning]], [[centos7-system-parameter-tuning]], [[linux]]

**fs.file-max** *(canonical form)*
: Linux 内核允许系统级文件句柄总量的 tunable；它控制的是全局文件表上限，而不是单个进程的 `nofile`。
- Preferred: `fs.file-max`
- See also: [[file-descriptor-and-tcp-backlog-tuning]], [[centos7-system-parameter-tuning]], [[linux]]

**LimitNOFILE** *(canonical form)*
: `systemd` unit 的服务级资源限制项，用来为由 `systemd` 启动的进程设置 `nofile` 上限；它与登录用户的 `ulimit -n` 不是同一层。
- Preferred: `LimitNOFILE`
- See also: [[file-descriptor-and-tcp-backlog-tuning]], [[centos7-system-parameter-tuning]], [[linux]]

**somaxconn / tcp_max_syn_backlog** *(canonical form)*
: Linux 内核中与 TCP 监听和握手排队相关的两个常见参数；它们影响 backlog 上限，但不等同于“系统总连接数”。
- Preferred: `somaxconn` / `tcp_max_syn_backlog`
- See also: [[file-descriptor-and-tcp-backlog-tuning]], [[centos7-system-parameter-tuning]], [[linux]]

**journalctl** *(canonical form)*
: `systemd` 日志查询工具，可按服务、时间范围或实时流查看系统日志。
- Preferred: `journalctl`
- See also: [[linux]], [[linux-command-line-operations]], [[linux-common-commands-reference]]

**crontab** *(canonical form)*
: 基于 cron 的定时任务配置入口，用于按分钟、小时、日期或星期调度命令与脚本。
- Preferred: `crontab`
- See also: [[linux-command-line-operations]], [[shell-scripting]], [[linux-common-commands-reference]]

**firewalld / firewall-cmd** *(canonical form)*
: Linux 上一种动态防火墙管理服务及其命令行入口；在当前来源中，它用于开放和关闭端口。
- Preferred: `firewalld` / `firewall-cmd`
- See also: [[linux]], [[linux-common-commands-reference]]

**SELinux** *(canonical form)*
: Linux 上常见的强制访问控制机制；在当前来源中，它被当作 Samba 联通性排查时先关闭的策略层，但正式文档应优先解释如何配置而不是直接停用。
- Preferred: `SELinux`
- See also: [[linux]], [[centos]], [[smb-file-sharing]]

**直接 I/O（direct I/O）** *(canonical form)*
: 通过 `iflag=direct` 或 `oflag=direct` 绕过文件系统缓存的 I/O 模式，常用于磁盘读写基准测试或区分缓存命中影响。
- Preferred: `直接 I/O` or `direct I/O`
- See also: [[linux-command-line-operations]], [[linux-common-commands-reference]]

**OS 初始化（OS initialization）** *(canonical form)*
: 指把新装 Linux 系统从裸机状态配置到可用基线的标准化流程，通常覆盖安装选项、账户、网络、远程访问、软件源、时间同步、安全策略和基础工具。
- Preferred: `OS 初始化` or `OS initialization` / Avoid: 把它和日常运维混为一谈
- See also: [[os-initialization-workflow]], [[centos7-os-initialization-workflow]], [[centos]]

**minimal 安装** *(canonical form)*
: 指 Linux 发行版安装时选择最小化套件集，只包含核心系统组件，后续按需补充；在当前来源中，它是 CentOS 7 初始化的推荐起点。
- Preferred: `minimal 安装` or `最小化安装`
- See also: [[os-initialization-workflow]], [[centos7-os-initialization-workflow]]

**NTP / chrony** *(canonical form)*
: Linux 上常见的时间同步服务；`ntpd` 是传统实现，`chrony` 是更现代的替代；在当前来源中，它们用于把系统时间同步到可靠时间源。
- Preferred: `NTP` / `chrony`
- See also: [[os-initialization-workflow]], [[centos7-os-initialization-workflow]], [[linux]]

**EPEL** *(canonical form)*
: Extra Packages for Enterprise Linux 的缩写，是面向 RHEL/CentOS 的第三方扩展仓库，提供官方仓库未包含的常用软件包。
- Preferred: `EPEL`
- See also: [[centos]], [[centos7-os-initialization-workflow]], [[legacy-repository-repointing]]

**研发思维（engineering mindset）** *(canonical form)*
: 指把现实问题转化为可计算系统的能力，强调抽象、建模、拆解、分层、权衡和验证，而不把“写代码”当成起点。
- Preferred: `研发思维` or `engineering mindset` / Avoid: 把它简化成“编码经验”
- See also: [[engineering-mindset]], [[engineering-thinking-framework]]

**抽象（abstraction）** *(canonical form)*
: 指去掉偶然细节、保留稳定结构的思维动作，是从现实问题进入系统设计的第一步。
- Preferred: `抽象` or `abstraction` / Avoid: 把“概括描述”直接等同于抽象
- See also: [[engineering-mindset]], [[engineering-thinking-framework]]

**建模（modeling）** *(canonical form)*
: 指把对象、关系、状态、事件和数据流显式表达出来，使问题能够被分析、设计和验证。
- Preferred: `建模` or `modeling`
- See also: [[engineering-mindset]], [[state-and-data-flow-modeling]]

**分治思维（decomposition）** *(canonical form)*
: 指把复杂问题拆成若干可独立理解和处理的子问题，再组合成整体方案。
- Preferred: `分治思维` or `decomposition`
- See also: [[engineering-mindset]], [[engineering-thinking-framework]]

**状态机思维（state-machine thinking）** *(canonical form)*
: 指从状态、事件和转移来理解系统行为的建模方式，适合描述会话、连接、消息状态等变化过程。
- Preferred: `状态机思维`
- See also: [[state-and-data-flow-modeling]], [[engineering-thinking-framework]]

**数据流思维（data-flow thinking）** *(canonical form)*
: 指把系统理解为数据在不同组件、边界和存储节点之间流动的过程，用于梳理处理链路和职责划分。
- Preferred: `数据流思维`
- See also: [[state-and-data-flow-modeling]], [[engineering-thinking-framework]]

**权衡（trade-off）** *(canonical form)*
: 指工程决策中在速度、复杂度、可靠性、成本和扩展性等约束之间做选择，而不是追求抽象意义上的“最优解”。
- Preferred: `权衡` or `trade-off` / Avoid: `最优方案` when constraints are material
- See also: [[engineering-mindset]]

**验证思维（validation thinking）** *(canonical form)*
: 指通过手推、画图、测试和极端情况分析来降低设计不确定性的工程方法。
- Preferred: `验证思维`
- See also: [[validation-driven-design]], [[engineering-thinking-framework]]

**Unix 哲学** *(canonical form)*
: 指围绕单一职责、文本接口、组合优于继承、统一抽象和小工具协作形成的一组系统设计原则。
- Preferred: `Unix 哲学`
- See also: [[unix-philosophy-and-pipeline-thinking]], [[linux]]

**管道思维（pipeline thinking）** *(canonical form)*
: 指把任务理解为若干输入、处理和输出阶段的串联方式，每个阶段只关注自己的输入输出契约。
- Preferred: `管道思维` or `pipeline thinking`
- See also: [[unix-philosophy-and-pipeline-thinking]], [[state-and-data-flow-modeling]]

**文件描述符（file descriptor）** *(canonical form)*
: 进程用来引用打开资源的整数句柄，标准输入、输出和错误分别对应 `0`、`1`、`2`；重定向本质上是在改变文件描述符指向的目标。
- Preferred: `文件描述符` or `file descriptor`
- See also: [[linux]], [[linux-command-line-operations]], [[file-descriptor-and-tcp-backlog-tuning]]

**最小权限原则（Principle of Least Privilege）** *(canonical form)*
: 指程序和用户只应拥有完成任务所需的最小权限，用于降低误操作范围、攻击面和审计复杂度。
- Preferred: `最小权限原则`
- See also: [[linux]], [[unix-philosophy-and-pipeline-thinking]]

**幂等性（idempotence）** *(canonical form)*
: 指同一操作执行一次和执行多次时，系统最终结果保持一致；在当前知识库里既用于脚本设计，也用于 API 和测试性质描述。
- Preferred: `幂等性` or `idempotence`
- See also: [[shell-scripting]], [[property-based-testing]], [[software-testing-architecture]]

**优雅停机（graceful shutdown）** *(canonical form)*
: 指进程在收到终止信号后先清理连接、状态和资源，再结束运行的退出方式，通常与 `SIGTERM` 相对应。
- Preferred: `优雅停机`
- See also: [[linux]], [[linux-command-line-operations]]

**USE 方法** *(canonical form)*
: 一种按资源类别检查 Utilization、Saturation、Errors 的系统排障框架，适合 CPU、内存、磁盘 I/O 和网络 I/O 的第一轮诊断。
- Preferred: `USE 方法` or `USE methodology`
- See also: [[use-methodology]], [[observability-and-reliability]]

**测试金字塔（testing pyramid）** *(canonical form)*
: 一种把测试分成“单元/属性测试最多、集成测试适中、E2E 最少”的分层质量模型，用于控制反馈成本和覆盖范围。
- Preferred: `测试金字塔` or `testing pyramid`
- See also: [[software-testing-architecture]]

**属性测试（property-based testing）** *(canonical form)*
: 通过自动生成输入来验证某个通用性质、不变量或规则是否始终成立的测试方法，用于补足手写样例的覆盖边界。
- Preferred: `属性测试` or `property-based testing`
- See also: [[property-based-testing]], [[software-testing-architecture]]

**Contract Test** *(canonical form)*
: 用于验证服务之间接口结构、字段约束和协议约定不被意外破坏的测试方式，重点是“协作契约”而不是完整用户流程。
- Preferred: `Contract Test`
- See also: [[software-testing-architecture]], [[devops-delivery-pipeline]]

**回归测试（regression testing）** *(canonical form)*
: 指把历史缺陷和关键风险转化为长期自动化测试，确保修复过的问题不会在后续修改中再次出现。
- Preferred: `回归测试` or `regression testing`
- See also: [[software-testing-architecture]], [[full-lifecycle-delivery-capability]]

**固定用例（Golden Cases）** *(canonical form)*
: 指一组稳定、确定性、可重复执行的基准测试数据，通常用于 CI 中提供快速且可比对的质量基线。
- Preferred: `固定用例` or `Golden Cases`
- See also: [[software-testing-architecture]]

**macOS** *(canonical form)*
: Apple 的桌面操作系统名称；在当前来源中，它既通过安装 U 盘制作、Recovery 和降级场景进入知识库，也通过本地命令行运维、开发工具链准备和交互层系统设置进入知识库。
- Preferred: `macOS` / Avoid: `MacOS`
- See also: [[macos]], [[macos-usb-installer-creation]], [[macos-common-commands-reference]], [[macos-command-line-operations]], [[macos-system-settings]]

**系统设置（System Settings）** *(canonical form)*
: macOS 上集中管理键盘、输入法、触控板、网络、显示等偏好的图形化设置应用；在当前来源中，快捷键、默认输入法和触控板行为都属于这一层，而不是命令行默认就能完整表达的控制面。
- Preferred: `系统设置` or `System Settings`
- See also: [[macos]], [[macos-system-settings]], [[macos-command-line-operations]]

**Spotlight** *(canonical form)*
: macOS 的系统级搜索与启动入口；在当前来源中，它与输入法切换快捷键一起被记录为需要按个人工作流配置的高频交互入口。
- Preferred: `Spotlight`
- See also: [[macos-system-settings]], [[macos]]

**输入法（input method / IME）** *(canonical form)*
: 指 macOS 上的文本输入来源与候选词系统；在当前来源中，默认输入法被建议设置为英文，以减少终端、开发和检索场景中的误切换。
- Preferred: `输入法` or `input method`
- See also: [[macos-system-settings]], [[macos]]

**辅助点按（Secondary click）** *(canonical form)*
: macOS 触控板或鼠标的右键等效操作；在当前来源中，推荐将触控板的辅助点按设置为右下角点击。
- Preferred: `辅助点按` or `Secondary click`
- See also: [[macos-system-settings]], [[macos]]

**轻点来点按（Tap to click）** *(canonical form)*
: macOS 触控板设置项，允许通过轻触而非按下触控板完成点击；在当前来源中，它被视为日常工作站的人机工效优化项。
- Preferred: `轻点来点按` or `Tap to click`
- See also: [[macos-system-settings]], [[macos]]

**网页预加载（page preloading）** *(canonical form)*
: 浏览器为加快潜在后续导航而提前加载页面资源的机制；在当前来源中，关闭 Chrome 的网页预加载被用作缓解特定版本输入卡顿的应用级 workaround。
- Preferred: `网页预加载` or `page preloading`
- See also: [[macos-system-settings]]

**可启动安装介质（bootable installer media）** *(canonical form)*
: 指写入完整安装器、可被设备直接识别为安装启动入口的外部介质；在当前来源中具体表现为 macOS 安装 U 盘。
- Preferred: `可启动安装介质` or `bootable installer media` / Avoid: 只说“把系统拷进 U 盘”
- See also: [[bootable-os-installer-media]], [[macos-usb-installer-creation]]

**createinstallmedia** *(canonical form)*
: macOS 安装器包内用于创建可启动安装介质的命令行工具；路径依赖具体的 `Install macOS <version>.app` 名称。
- Preferred: `createinstallmedia`
- See also: [[macos]], [[macos-usb-installer-creation]], [[bootable-os-installer-media]]

**Recovery 模式** *(canonical form)*
: macOS 的恢复环境，用于修复、重装前准备和启动安全相关设置调整；在当前来源中，它是降级前修改外部启动策略的入口。
- Preferred: `Recovery 模式` or `Recovery`
- See also: [[startup-security-and-external-boot-policy]], [[macos]]

**启动安全性实用工具（Startup Security Utility）** *(canonical form)*
: Recovery 环境中的安全控制工具，用于调整启动相关安全级别和外部启动许可；在当前来源中，它是 macOS 14 降级到 13 的关键前置步骤。
- Preferred: `启动安全性实用工具` or `Startup Security Utility`
- See also: [[startup-security-and-external-boot-policy]], [[macos-usb-installer-creation]]

**NVRAM** *(canonical form)*
: Mac 启动相关参数的非易失性存储；在当前来源中，它被作为启动异常时的恢复辅助手段之一。
- Preferred: `NVRAM`
- See also: [[macos]], [[startup-security-and-external-boot-policy]]

**SMC** *(canonical form)*
: Mac 平台上与电源、热管理等底层行为相关的控制器复位操作；在当前来源中，它与 `NVRAM` reset 一起被列为启动异常时的排障入口。
- Preferred: `SMC`
- See also: [[macos]], [[macos-usb-installer-creation]]

**Homebrew** *(canonical form)*
: macOS 上最常见的社区包管理工具，用于通过命令行安装、升级、卸载和诊断开发者工具与本地软件包；它不是 Apple 官方内建包管理器。
- Preferred: `Homebrew` or `brew`
- See also: [[macos]], [[macos-common-commands-reference]], [[macos-command-line-operations]]

**launchctl** *(canonical form)*
: macOS 上用于与 `launchd` 交互的命令行接口，可用于列出、加载、卸载、启动和停止 LaunchAgents / LaunchDaemons；正式文档应补充 service domain 与版本语义。
- Preferred: `launchctl`
- See also: [[macos]], [[macos-common-commands-reference]], [[macos-command-line-operations]]

**pmset** *(canonical form)*
: macOS 的电源管理命令行接口，用于查看和修改休眠、电源计划及电池相关状态；`caffeinate` 则更偏向临时抑制休眠。
- Preferred: `pmset`
- See also: [[macos]], [[macos-common-commands-reference]], [[macos-command-line-operations]]

**diskutil** *(canonical form)*
: macOS 的磁盘与卷管理命令行接口，用于列出介质、查看磁盘信息、弹出设备等；它补充了 `df` / `du` 这类只看文件系统占用的命令。
- Preferred: `diskutil`
- See also: [[macos]], [[macos-common-commands-reference]], [[macos-command-line-operations]]

**system_profiler** *(canonical form)*
: macOS 的系统信息采集命令，可按数据类型输出软件、硬件等详细清单，适合比 `sw_vers` 或单个 `sysctl` 查询更完整的盘点。
- Preferred: `system_profiler`
- See also: [[macos]], [[macos-common-commands-reference]], [[macos-command-line-operations]]

**defaults** *(canonical form)*
: macOS 用于读取和写入 user defaults / app preferences 的命令行接口，常用于修改 Finder、Dock、截图等行为；很多变更需要重启相关进程后才会体现。
- Preferred: `defaults`
- See also: [[macos]], [[macos-common-commands-reference]], [[macos-command-line-operations]]

**Xcode Command Line Tools** *(canonical form)*
: Apple 提供的命令行开发工具集合，通常通过 `xcode-select --install` 安装，为编译器、SDK 头文件和常见开发命令提供基础环境。
- Preferred: `Xcode Command Line Tools` or `Xcode CLI tools`
- See also: [[macos]], [[macos-common-commands-reference]], [[macos-command-line-operations]]

**rbenv** *(canonical form)*
: macOS/Linux 上常见的 Ruby 版本管理工具，通过 shim 机制拦截 `ruby` 命令并路由到用户指定的版本；在当前来源中，它是 macOS 开发环境配置的推荐 Ruby 版本管理方案。
- Preferred: `rbenv`
- See also: [[macos]], [[macos-development-environment-setup]], [[language-runtime-version-management]]

**CocoaPods** *(canonical form)*
: iOS/macOS 项目的传统依赖管理工具，通过 `Podfile` 声明第三方库依赖并管理其下载、编译和集成；在当前来源中，它作为 Ruby gem 安装在 rbenv 管理的 Ruby 环境之上。
- Preferred: `CocoaPods` / Avoid: `cocoapods`（小写）when referring to the tool name
- See also: [[macos]], [[macos-development-environment-setup]]

**gem** *(canonical form)*
: Ruby 的包管理命令和包格式名称；`gem install` 用于安装 Ruby 库或工具，`Gemfile` + `Bundler` 用于项目级依赖管理。
- Preferred: `gem`
- See also: [[macos-development-environment-setup]], [[language-runtime-version-management]]

**语言运行时版本管理（language runtime version management）** *(canonical form)*
: 指使用专用版本管理工具在单台开发机上安装、切换和隔离多个语言运行时版本的实践；常见工具包括 rbenv（Ruby）、nvm（Node.js）、pyenv（Python）。
- Preferred: `语言运行时版本管理` or `language runtime version management`
- See also: [[language-runtime-version-management]], [[macos-development-environment-setup]]

**Windows** *(canonical form)*
: Microsoft 的桌面/服务器操作系统；在当前知识库里，它既是 SMB 网络驱动器映射的客户端环境，也是使用 `Inno Setup`、`WMI`、`MFC` 与 `Win32` 完成原生应用安装和设备标识生成的平台，同时还覆盖 `Windows Update`、`Services`、`Task Scheduler` 与 PowerShell 防火墙规则这类工作站管理控制面。
- Preferred: `Windows`
- See also: [[windows]], [[windows-development-related]], [[windows-system-settings]]

**Windows Update** *(canonical form)*
: Windows 内置的系统更新服务与相关任务集合；当前来源通过禁用服务、修改恢复动作、停用计划任务和清理升级助手目录来抑制 `Win10` 自动更新。
- Preferred: `Windows Update`
- See also: [[windows-system-settings]], [[windows]]

**服务（Services）** *(canonical form)*
: Windows 用于查看和管理后台服务启动类型、运行状态和失败恢复动作的控制面；当前来源通过它禁用 `Windows Update` 并把失败恢复改成 `无操作`。
- Preferred: `Services` or `服务`
- See also: [[windows-system-settings]], [[windows]]

**任务计划程序（Task Scheduler）** *(canonical form)*
: Windows 的计划任务管理控制面；当前来源用它停用 `Microsoft\\Windows\\Windows Update` 路径下的更新相关任务，说明系统行为不只由长期运行的服务决定。
- Preferred: `Task Scheduler` or `任务计划程序`
- See also: [[windows-system-settings]], [[windows]]

**PowerShell** *(canonical form)*
: Windows 上常用的命令行与自动化环境；当前来源通过 PowerShell 的 `New-NetFirewallRule` cmdlet 创建入站防火墙规则，为调试端口开放访问。
- Preferred: `PowerShell`
- See also: [[windows-system-settings]], [[windows]]

**New-NetFirewallRule** *(canonical form)*
: PowerShell 用于创建 Windows 防火墙规则的 cmdlet；当前来源用它放行入站 TCP `12345`，作为调试场景下的最小端口开放示例。
- Preferred: `New-NetFirewallRule`
- See also: [[windows-system-settings]], [[windows]]

**Inno Setup** *(canonical form)*
: Windows 安装包制作工具，使用 section-based 脚本和 Pascal 风格代码钩子来描述安装器行为；当前来源中它负责文件打包、安装前钩子、多语言安装器以及服务安装/卸载阶段动作。
- Preferred: `Inno Setup`
- See also: [[inno-setup]], [[windows-development-related]], [[windows]]

**WMI（Windows Management Instrumentation）** *(canonical form)*
: Windows 的系统与硬件信息查询接口；当前来源中它被用来读取 BIOS、CPU、主板和硬盘属性，为设备标识生成提供原始字段。
- Preferred: `WMI` or `Windows Management Instrumentation`
- See also: [[windows-management-instrumentation]], [[hardware-derived-machine-identifier]], [[windows]]

**MFC** *(canonical form)*
: Microsoft Foundation Class Library，Windows 原生 C++ 开发中的一个封装层；当前来源用它承载 COM / WMI 查询代码，而不是通过外部命令取数。
- Preferred: `MFC`
- See also: [[windows-development-related]], [[windows-management-instrumentation]], [[windows]]

**Win32** *(canonical form)*
: Windows 用户态 API 与传统原生开发语境的统称；在当前来源中，它与 `MFC` 区分开来，表现为通过 `CreateProcessW`、管道和 `ReadFile` 调起 `wmic` 并解析输出的实现路径。
- Preferred: `Win32`
- See also: [[windows-development-related]], [[windows]], [[windows-management-instrumentation]]

**机器码（machine identifier）** *(canonical form)*
: 在当前 Windows 来源中，`机器码` 指由多项硬件信息拼接并哈希得到的设备标识，而不是 CPU 指令层面的 machine code；文档中应优先把它解释为 hardware-derived machine identifier。
- Preferred: `机器码` or `machine identifier` / Avoid: 直接翻成 `machine code`
- See also: [[hardware-derived-machine-identifier]], [[windows-development-related]]

---

## Style Conventions

*(Writing rules and tone guidelines specific to this knowledge base's domain. Will populate as style guides and branded content are ingested.)*

| Convention | Rule | Example |
|---|---|---|
| Canonical AI terms | Use `AI辅助开发` for engineering-process collaboration and `AI融合` for product/function embedding. | “通过 AI辅助开发提效，并在项目中推进 AI融合。” |
| AI tool integration terminology | Use `MCP` for the tool/data connection protocol and `Steering 文件` for persistent project context. | “通过 MCP 接数据库，通过 Steering 文件固化项目规范。” |
| Platform terminology | Use `底座` or `平台底座` for shared strategic infrastructure, not generic “公共能力”. | “新系统优先复用平台底座能力。” |
| Delivery scope | Use `完整交付能力` when describing end-to-end ownership across the full lifecycle. | “小团队模式依赖完整交付能力。” |
| Full-stack terminology | Use `全栈研发` when the topic includes delivery-chain awareness, not just front-end/back-end coding breadth. | “这份入门材料强调全栈研发的最小交付闭环。” |
| Engineering thinking terminology | Use `抽象` for stripping details to stable structure, `建模` for expressing states/data flow, and `验证` for uncertainty reduction. | “先抽象对象，再建模状态与数据流，最后列验证方案。” |
| Editor terminology | Use `Vim` for the editor, `vimrc` for its config file, and `模态编辑` for the editing model. | “先解释模态编辑，再介绍 `~/.vimrc` 常用配置。” |
| Shell terminology | Use `Bash` when the content depends on Bash-specific syntax; use `shell scripting` for the broader automation practice. | “这段脚本使用了 Bash 关联数组，属于 Bash 专用写法。” |
| Capacity tuning terminology | Name the exact layer such as `nofile`, `fs.file-max`, or `LimitNOFILE` instead of saying only “把句柄数调大了”. | “先调 `fs.file-max`，再确认服务 unit 的 `LimitNOFILE` 是否同步。” |
| Linux operations terminology | When behavior depends on a specific subsystem, use the exact command or service name such as `journalctl`, `crontab`, or `firewalld`, not a vague “Linux 命令”. | “查看 `systemd` 日志时使用 `journalctl`。” |
| Apple platform terminology | Use `macOS` for the OS name, `Recovery 模式` for the maintenance environment, and exact Apple command names such as `createinstallmedia`, `diskutil`, `launchctl`, `pmset`, and `defaults` when behavior depends on them. | “先用 `diskutil` 查看磁盘，再用 `defaults` 调整 Finder 行为。” |
| macOS GUI terminology | When behavior depends on `系统设置` or app preferences, preserve the exact UI label and distinguish recommended configuration from platform default. | “启用 `轻点来点按`，并注明它是推荐设置，不是所有 Mac 的出厂默认值。” |
| Windows terminology | When behavior depends on a Windows-specific surface, name the exact layer such as `Services`, `Task Scheduler`, `PowerShell`, `New-NetFirewallRule`, `Inno Setup`, `InitializeSetup()`, `WMI`, `MFC`, or `Win32`, explain `机器码` as a hardware-derived identifier rather than compiled machine code, and treat update disabling as a contextual workaround rather than a universal default. | “先在 `Services` 停用 `Windows Update`，再说明 `New-NetFirewallRule` 只是为调试临时开放入站端口。” |
| Testing terminology | Use `属性测试` when describing invariant-driven generated-input testing, and use `回归测试` for suites built from historical bugs. | “把这个历史缺陷沉淀为回归测试，再补一条属性测试覆盖通用规律。” |

---

## Deprecated / Avoid List

Terms that have been replaced, renamed, or should not be used:

| Avoid | Use Instead | Reason |
|---|---|---|
| 只说“监控” | `可观测性` | 当前语境明确包含日志、指标和 Trace，范围大于传统监控。 |
| 把 AI能力 统称为“工具” | `AI辅助开发` or `AI融合` | 需要区分工程过程提效与业务功能落地。 |
| 把 `MCP` 写成泛泛的“插件机制” | `MCP` or `Model Context Protocol` | 需要保留协议层与工具接入边界的含义。 |
| 把长期项目约束写成一次性 Prompt | `Steering 文件` or project context docs | 需要区分持久上下文与单次任务指令。 |
| `playwrite` | `Playwright` | 原始来源中存在拼写不规范，知识库统一采用产品正确名称。 |
| 把底座写成“公共部分” | `底座` or `平台底座` | “底座”更准确表达复用、治理和战略支撑含义。 |
| 把 Bash 专有写法统称为“shell 通用语法” | `Bash` | `[[ ]]`、关联数组、丰富参数展开等能力并非所有 shell 都支持。 |
| 把 `journalctl`、`firewall-cmd` 等接口写成所有 Linux 环境通用命令 | 写明 `systemd`、`firewalld` 或具体发行版前提 | 避免把子系统特定行为误写成普适事实。 |
| `MacOS` | `macOS` | Apple 桌面操作系统名称在当前知识库里统一使用官方大小写。 |
| 把 `Homebrew` 写成 Apple 官方包管理器 | `Homebrew`（社区包管理器） | 避免误写支持边界和工具归属。 |
| 把个人或团队工作站偏好直接写成“macOS 默认” | 写成 `推荐设置`、`偏好配置` 或标明来源上下文 | 避免把用户自定义映射误写成平台默认事实。 |
| 把当前 Windows 来源中的 `机器码` 直译为 `machine code` | `机器码`（设备标识）or `hardware-derived machine identifier` | 避免把硬件派生标识与编译后的机器指令混淆。 |
| 把禁用 `Windows Update` 写成 Windows 通用最佳实践 | 写成 `特定场景下的更新抑制措施` 并注明版本/安全代价 | 避免把战术性 workaround 误写成默认管理基线。 |
| 把所有自动化测试都叫“单元测试” | 按 `属性测试`、`集成测试`、`E2E`、`回归测试` 等更准确名称区分 | 避免掩盖测试层级、成本和职责差异。 |

---

## Regional / Variant Terms

Terms that differ between audiences, teams, or locales:

| Term | Region/Context | Notes |
|---|---|---|
| 信创 Linux | 行业/政企项目语境 | 与国产化软硬件适配和落地相关。 |
| 鸿蒙移动端适配 | 终端生态语境 | 指对鸿蒙端的产品或应用适配工作。 |
| Vim / vim | 开发者工具语境 | 文中提及产品名称时优先写 `Vim`；命令名或文件路径中可写小写 `vim`。 |
| Bash / shell | 开发者工具语境 | 讲具体语法、关联数组、`getopts`、`[[ ]]` 时优先写 `Bash`；讲更宽泛的自动化实践时再写 `shell scripting`。 |
| `Option` / `Alt` | Mac 启动语境 | 文档可写作 `Option (Alt)`，兼顾不同键盘标识。 |
| `ip` / `ss` vs `ifconfig` / `netstat` | Linux 网络运维语境 | 文档优先使用更现代的 `ip`、`ss`，但可注明在旧系统中仍会遇到后者。 |
| `ifconfig` / `netstat` / `route` | macOS / BSD 桌面运维语境 | 当前 macOS 来源大量使用这些接口；不要把 Linux 文档里的 `ip` / `ss` 示例直接移植成“通用 Unix”表述。 |

---

## Related Pages

- [[overview]] — big-picture synthesis
- [[index]] — master catalog
- [[2025-technical-line-summary]] — first ingested source summary
- [[engineering-thinking-framework]] — engineering-thinking source summary
- [[linux]] — Linux platform/tool page
- [[linux-command-line-operations]] — reusable Linux operations concept
- [[linux-common-commands-reference]] — Linux commands source summary
- [[technical-line-leader]] — primary management persona inferred from the source
- [[ai-full-stack-development-onboarding]] — source summary for AI-era full-stack onboarding
- [[model-context-protocol]] — tool/data connection protocol for AI systems
- [[project-steering-context]] — persistent project context for AI collaboration
- [[devops-delivery-pipeline]] — delivery flow from development through runtime feedback
- [[ai-era-full-stack-beginner]] — persona learning AI-assisted full-stack delivery basics
- [[growing-engineer]] — learner persona focused on abstraction, modeling, and validation growth
- [[full-lifecycle-delivery-capability]] — end-to-end delivery concept
- [[ai-enabled-software-delivery]] — AI-enabled engineering concept
- [[engineering-mindset]] — foundational engineering-thinking concept
- [[state-and-data-flow-modeling]] — system modeling concept centered on state and flow
- [[validation-driven-design]] — design-time validation and uncertainty reduction concept
- [[macos-usb-installer-creation]] — macOS installation USB source summary
- [[macos-common-commands-reference]] — macOS command cheat sheet source summary
- [[macos-system-settings]] — macOS workstation settings source summary
- [[macos]] — macOS product page
- [[macos-command-line-operations]] — reusable macOS operations concept
- [[windows-development-related]] — Windows-native development source summary
- [[windows]] — Windows product page
- [[inno-setup]] — Windows installer tool page
- [[windows-management-instrumentation]] — Windows hardware/system metadata query concept
- [[hardware-derived-machine-identifier]] — concept page for hardware-based device identifiers
- [[bootable-os-installer-media]] — concept page for external installation media creation
- [[startup-security-and-external-boot-policy]] — concept page for recovery-time external boot authorization
- [[platform-foundation]] — shared platform and governance concept
- [[observability-and-reliability]] — observability and reliability concept
- [[unix-philosophy-and-pipeline-thinking]] — Unix-style design model centered on composition and stable interfaces
- [[use-methodology]] — resource-first troubleshooting method for observability and performance
- [[software-testing-architecture]] — layered testing model for quality feedback
- [[property-based-testing]] — invariant-driven testing concept
- [[moshi-neural-audio-codec-architecture-analysis]] — source summary for Moshi and codec architecture
- [[moshi]] — speech-native LLM product page
- [[mimi]] — audio tokenizer product page
- [[speech-native-llm]] — core speech-first architecture concept
- [[neural-audio-codec]] — codec infrastructure concept
- [[full-duplex-speech-interaction]] — real-time duplex interaction concept
- [[bash-syntax-and-scripting-reference]] — Bash source summary and scripting reference
- [[bash]] — Bash product/tool page
- [[shell-scripting]] — shell automation concept
- [[vim-usage-and-configuration-reference]] — Vim source summary and command reference
- [[vim]] — Vim product/tool page
- [[os-initialization-workflow]] — OS initialization concept
- [[centos7-os-initialization-workflow]] — CentOS 7 initialization source summary
- [[modal-editing]] — modal editing concept
- [[ubuntu]] — Ubuntu distribution page
- [[netplan-configuration-guide]] — Netplan configuration source summary
- [[ubuntu2204-openssh-upgrade-from-source]] — Ubuntu 22.04 OpenSSH source upgrade runbook
- [[ubuntu-common-issues-and-optimization]] — Ubuntu troubleshooting and optimization source summary
- [[systemd-resolved-dns-management]] — DNS resolver management concept
- [[network-configuration]] — declarative network configuration concept
