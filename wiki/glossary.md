---
title: Glossary
type: glossary
created: 2026-04-07
updated: 2026-04-15
sources: [2025年技术线总结.md, Moshi 与神经音频编码（Neural Audio Codec）技术架构解析.md, vim.md, bash.md, commands.md, CentOS6由于镜像废弃无法使用的解决办法.md, CentOS7离线安装docker问题排查.md, CentOS7配置Samba共享.md, CentOS7升级内核.md, CentOS7升级OpenSSL和OpenSSH.md, CentOS7系统参数调优.md, CentOS操作系统初始化流程.md, 基于docker构建ubuntu20.04开发环境.md, netplan配置指南.md, Ubuntu22.04升级OpenSSH版本到最新.md, Ubuntu常见问题与优化.md]
tags: [terminology, style, glossary, ai, engineering-management, speech-llm, developer-tooling, linux, command-line, vim, bash, shell, centos, ubuntu, yum, repository, elrepo, grub, bootloader, docker, containers, kernel, networking, netplan, yaml, vlan, bonding, bridging, samba, smb, file-sharing, windows, selinux, openssl, openssh, ssh, tls, source-build, sysctl, systemd, tuning, file-descriptors, tcp, initialization, post-install, ntp, chrony, epel, development-environment, apt-mirror, pam, systemd-resolved, dns, swap, nfs, multipath]
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
- See also: [[kernel-upgrade-and-boot-management]], [[centos7-kernel-upgrade-via-elrepo]], [[centos]]

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

---

## Style Conventions

*(Writing rules and tone guidelines specific to this knowledge base's domain. Will populate as style guides and branded content are ingested.)*

| Convention | Rule | Example |
|---|---|---|
| Canonical AI terms | Use `AI辅助开发` for engineering-process collaboration and `AI融合` for product/function embedding. | “通过 AI辅助开发提效，并在项目中推进 AI融合。” |
| Platform terminology | Use `底座` or `平台底座` for shared strategic infrastructure, not generic “公共能力”. | “新系统优先复用平台底座能力。” |
| Delivery scope | Use `完整交付能力` when describing end-to-end ownership across the full lifecycle. | “小团队模式依赖完整交付能力。” |
| Editor terminology | Use `Vim` for the editor, `vimrc` for its config file, and `模态编辑` for the editing model. | “先解释模态编辑，再介绍 `~/.vimrc` 常用配置。” |
| Shell terminology | Use `Bash` when the content depends on Bash-specific syntax; use `shell scripting` for the broader automation practice. | “这段脚本使用了 Bash 关联数组，属于 Bash 专用写法。” |
| Capacity tuning terminology | Name the exact layer such as `nofile`, `fs.file-max`, or `LimitNOFILE` instead of saying only “把句柄数调大了”. | “先调 `fs.file-max`，再确认服务 unit 的 `LimitNOFILE` 是否同步。” |
| Linux operations terminology | When behavior depends on a specific subsystem, use the exact command or service name such as `journalctl`, `crontab`, or `firewalld`, not a vague “Linux 命令”. | “查看 `systemd` 日志时使用 `journalctl`。” |

---

## Deprecated / Avoid List

Terms that have been replaced, renamed, or should not be used:

| Avoid | Use Instead | Reason |
|---|---|---|
| 只说“监控” | `可观测性` | 当前语境明确包含日志、指标和 Trace，范围大于传统监控。 |
| 把 AI能力 统称为“工具” | `AI辅助开发` or `AI融合` | 需要区分工程过程提效与业务功能落地。 |
| 把底座写成“公共部分” | `底座` or `平台底座` | “底座”更准确表达复用、治理和战略支撑含义。 |
| 把 Bash 专有写法统称为“shell 通用语法” | `Bash` | `[[ ]]`、关联数组、丰富参数展开等能力并非所有 shell 都支持。 |
| 把 `journalctl`、`firewall-cmd` 等接口写成所有 Linux 环境通用命令 | 写明 `systemd`、`firewalld` 或具体发行版前提 | 避免把子系统特定行为误写成普适事实。 |

---

## Regional / Variant Terms

Terms that differ between audiences, teams, or locales:

| Term | Region/Context | Notes |
|---|---|---|
| 信创 Linux | 行业/政企项目语境 | 与国产化软硬件适配和落地相关。 |
| 鸿蒙移动端适配 | 终端生态语境 | 指对鸿蒙端的产品或应用适配工作。 |
| Vim / vim | 开发者工具语境 | 文中提及产品名称时优先写 `Vim`；命令名或文件路径中可写小写 `vim`。 |
| Bash / shell | 开发者工具语境 | 讲具体语法、关联数组、`getopts`、`[[ ]]` 时优先写 `Bash`；讲更宽泛的自动化实践时再写 `shell scripting`。 |
| `ip` / `ss` vs `ifconfig` / `netstat` | Linux 网络运维语境 | 文档优先使用更现代的 `ip`、`ss`，但可注明在旧系统中仍会遇到后者。 |

---

## Related Pages

- [[overview]] — big-picture synthesis
- [[index]] — master catalog
- [[2025-technical-line-summary]] — first ingested source summary
- [[linux]] — Linux platform/tool page
- [[linux-command-line-operations]] — reusable Linux operations concept
- [[linux-common-commands-reference]] — Linux commands source summary
- [[technical-line-leader]] — primary management persona inferred from the source
- [[full-lifecycle-delivery-capability]] — end-to-end delivery concept
- [[ai-enabled-software-delivery]] — AI-enabled engineering concept
- [[platform-foundation]] — shared platform and governance concept
- [[observability-and-reliability]] — observability and reliability concept
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
