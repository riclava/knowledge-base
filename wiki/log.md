# Wiki Log

Append-only chronological record of all activity: ingests, queries, and lint passes.

To view recent activity: `grep "^## \[" log.md | tail -10`

---

## [2026-04-07] init | Wiki created

Wiki initialized for a technical writer's personal knowledge base.

Structure created:
- `raw/` — source documents folder
- `wiki/` — LLM-maintained knowledge base
- `wiki/sources/` — per-source summary pages
- `CLAUDE.md` — schema and operating instructions

Core pages created:
- `wiki/index.md`
- `wiki/log.md`
- `wiki/overview.md`
- `wiki/glossary.md`

Next step: Drop your first source into `raw/` and say **"ingest [filename]"**.

## [2026-04-14] ingest | 2025年技术线总结

Pages created:
- `wiki/sources/2025-technical-line-summary.md`
- `wiki/personas/technical-line-leader.md`
- `wiki/concepts/full-lifecycle-delivery-capability.md`
- `wiki/concepts/ai-enabled-software-delivery.md`
- `wiki/concepts/platform-foundation.md`
- `wiki/concepts/observability-and-reliability.md`

Pages updated:
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Filed the first source summary for a technical-line annual retrospective and planning report.
- Established core concepts for AI-enabled delivery, platform foundation, observability/reliability, and full-lifecycle delivery capability.
- Added a technical-line leader persona and canonical terminology for AI辅助开发、AI融合、底座、可观测性、LLMOps 等表述。

## [2026-04-14] ingest | Moshi 与神经音频编码技术架构解析

Pages created:
- `wiki/sources/moshi-neural-audio-codec-architecture-analysis.md`
- `wiki/products/moshi.md`
- `wiki/products/mimi.md`
- `wiki/concepts/speech-native-llm.md`
- `wiki/concepts/neural-audio-codec.md`
- `wiki/concepts/full-duplex-speech-interaction.md`

Pages updated:
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Added a source summary for Moshi, Mimi, and the Neural Audio Codec landscape.
- Established core concepts for speech-native LLMs, neural audio codecs, and full-duplex speech interaction.
- Added product pages for Moshi and Mimi, plus canonical terminology for speech-native LLM、全双工对话、Neural Audio Codec、语音 LLM tokenizer、RVQ 等表述。

## [2026-04-15] ingest | Vim 常用配置与操作参考

Pages created:
- `wiki/sources/vim-usage-and-configuration-reference.md`
- `wiki/products/vim.md`
- `wiki/concepts/modal-editing.md`

Pages updated:
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Added the first Linux/developer-tooling source summary, centered on Vim configuration, editing commands, batch text operations, and plugin management.
- Established `Vim` as a product/tool page and `模态编辑` as a reusable editing-model concept page.
- Added canonical terminology for Vim、vimrc、模态编辑、文本对象、buffer 和 vim-plug, and expanded the overview to include terminal workflow knowledge.

## [2026-04-15] ingest | Bash 语法与脚本技巧

Pages created:
- `wiki/sources/bash-syntax-and-scripting-reference.md`
- `wiki/products/bash.md`
- `wiki/concepts/shell-scripting.md`

Pages updated:
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Added a Bash source summary covering variables, parameter expansion, arrays, control flow, option parsing, I/O, error handling, and common scripting tricks.
- Established `Bash` as a product/tool page and `shell scripting` as a reusable automation concept page.
- Added canonical terminology for Bash、shell scripting、参数展开、Here Document、getopts 和 `set -euo pipefail`, and expanded the overview from editor-centric Linux tooling toward broader terminal automation knowledge.

## [2026-04-15] ingest | Linux 常用命令

Pages created:
- `wiki/sources/linux-common-commands-reference.md`
- `wiki/products/linux.md`
- `wiki/concepts/linux-command-line-operations.md`

Pages updated:
- `wiki/products/bash.md`
- `wiki/concepts/shell-scripting.md`
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Added a Linux command-line source summary spanning file operations, text processing, storage, processes, networking, permissions, archives, cron, and logs.
- Established `Linux` as a platform/tool page and `Linux command-line operations` as a reusable operations concept page.
- Added canonical terminology for Linux、`rsync`、`inode`、`grep/sed/awk`、`systemd`、`journalctl`、`crontab`、`firewalld` 和 `direct I/O`, and clarified how Bash scripts orchestrate common Linux commands.

## [2026-04-15] ingest | CentOS6由于镜像废弃无法使用的解决办法

Pages created:
- `wiki/sources/centos6-archive-repository-workaround.md`
- `wiki/products/centos.md`
- `wiki/concepts/legacy-repository-repointing.md`

Pages updated:
- `wiki/products/linux.md`
- `wiki/concepts/linux-command-line-operations.md`
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Added a focused source summary for recovering CentOS 6 `yum` access after standard mirrors were retired and archive/vault repos became necessary.
- Established `CentOS` as a product page and `legacy repository repointing` as a reusable concept for restoring package access on legacy distributions.
- Added canonical terminology for CentOS、`YUM`、repo 文件、`mirrorlist/baseurl`、vault 仓库 和 `fastestmirror`, and explicitly flagged the source's CentOS 6 / CentOS 7 GPG key mismatch as a verification risk.

## [2026-04-15] ingest | CentOS7离线安装docker问题排查

Pages created:
- `wiki/sources/centos7-offline-docker-install-troubleshooting.md`
- `wiki/products/docker.md`
- `wiki/concepts/container-network-namespace-support.md`

Pages updated:
- `wiki/products/centos.md`
- `wiki/products/linux.md`
- `wiki/concepts/linux-command-line-operations.md`
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Added a CentOS 7 Docker troubleshooting source summary centered on failed port publishing after offline installation and the discovery that host-kernel age, not reinstall steps, was the decisive variable.
- Established `Docker` as a product page and `container network namespace support` as a reusable concept for diagnosing bridge/veth and namespace-ID-related container networking failures.
- Added canonical terminology for Docker、离线安装、`network namespace`、`docker0 / veth`、`link-netnsid` 和 `iptables`, and expanded the Linux/CentOS synthesis from package-source maintenance into container host compatibility troubleshooting.

## [2026-04-15] ingest | CentOS7配置Samba共享

Pages created:
- `wiki/sources/centos7-samba-share-setup.md`
- `wiki/products/samba.md`
- `wiki/concepts/smb-file-sharing.md`

Pages updated:
- `wiki/products/centos.md`
- `wiki/products/linux.md`
- `wiki/concepts/linux-command-line-operations.md`
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Added a CentOS 7 Samba source summary covering the minimal path from package install to Windows network-drive mapping.
- Established `Samba` as a product page and `SMB file sharing` as a reusable concept for cross-platform directory sharing.
- Added canonical terminology for Samba、`SMB/CIFS`、`smb.conf`、`smbpasswd` 和 `SELinux`, and explicitly flagged blanket firewall/SELinux shutdown as a documentation risk rather than a default best practice.

## [2026-04-15] ingest | CentOS7升级内核

Pages created:
- `wiki/sources/centos7-kernel-upgrade-via-elrepo.md`
- `wiki/concepts/kernel-upgrade-and-boot-management.md`

Pages updated:
- `wiki/sources/centos7-offline-docker-install-troubleshooting.md`
- `wiki/products/centos.md`
- `wiki/products/linux.md`
- `wiki/products/docker.md`
- `wiki/concepts/container-network-namespace-support.md`
- `wiki/concepts/linux-command-line-operations.md`
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Added a CentOS 7 source summary for upgrading kernels through ELRepo, including `kernel-lt` discovery, GRUB default selection, reboot validation, and optional cleanup steps.
- Established `kernel upgrade and boot management` as a reusable concept linking package installation, boot-entry activation, rollback posture, and post-reboot verification.
- Added canonical terminology for `ELRepo`、`GRUB2`、`kernel-lt / kernel-ml`, and clarified across Docker/CentOS pages that host-kernel remediation may follow either a newer stock CentOS 7 kernel path or an ELRepo alternative-kernel path.

## [2026-04-15] ingest | CentOS7升级OpenSSL和OpenSSH

Pages created:
- `wiki/sources/centos7-openssl-and-openssh-upgrade-from-source.md`
- `wiki/products/openssl.md`
- `wiki/products/openssh.md`
- `wiki/concepts/source-built-package-replacement.md`

Pages updated:
- `wiki/products/centos.md`
- `wiki/products/linux.md`
- `wiki/concepts/linux-command-line-operations.md`
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Added a CentOS 7 source summary for upgrading OpenSSL and OpenSSH from upstream source, including dependency ordering, linker-path changes, service cutover, and remote-access continuity risks.
- Established `OpenSSL` and `OpenSSH` as product pages plus `source-built package replacement` as a reusable Linux operations concept for source-compiled software taking over system paths and services.
- Added canonical terminology for `源码编译安装`、`OpenSSL`、`OpenSSH`、`ldconfig`、`sshd_config` 和 `rpm -e --nodeps`, and expanded the Linux/CentOS synthesis to cover high-risk SSH/TLS stack upgrades on legacy hosts.

## [2026-04-15] ingest | CentOS7 系统参数调优

Pages created:
- `wiki/sources/centos7-system-parameter-tuning.md`
- `wiki/concepts/file-descriptor-and-tcp-backlog-tuning.md`

Pages updated:
- `wiki/products/centos.md`
- `wiki/products/linux.md`
- `wiki/concepts/linux-command-line-operations.md`
- `wiki/sources/centos7-samba-share-setup.md`
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Added a CentOS 7 source summary for tuning `nofile`, `fs.file-max`, `systemd` service limits, and TCP backlog-related kernel parameters.
- Established `file descriptor and TCP backlog tuning` as a reusable Linux operations concept that separates session-level, kernel-level, and service-level resource ceilings.
- Added canonical terminology for `limits.conf`、`nofile`、`sysctl`、`fs.file-max`、`LimitNOFILE` 和 `somaxconn / tcp_max_syn_backlog`, and clarified in the Samba notes that `limits.conf` is a generic capacity-tuning surface rather than Samba-specific configuration.

## [2026-04-15] ingest | CentOS 操作系统初始化流程

Pages created:
- `wiki/sources/centos7-os-initialization-workflow.md`
- `wiki/concepts/os-initialization-workflow.md`

Pages updated:
- `wiki/products/centos.md`
- `wiki/products/linux.md`
- `wiki/concepts/linux-command-line-operations.md`
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Added a CentOS 7 source summary for OS initialization workflow covering minimal install, account configuration, network/DNS setup, SSH port adjustment, internal mirror configuration, NTP/chrony time sync, SELinux/firewalld simplification, and base tool installation.
- Established `OS initialization workflow` as a reusable concept for structuring the path from bare metal to usable baseline.
- Added canonical terminology for `OS 初始化`、`minimal 安装`、`NTP / chrony` 和 `EPEL`, and explicitly flagged the source's SELinux/firewalld shutdown as a documentation risk (quick-start simplification, not production default).
- Extracted the generic initialization pattern (install → account → network → remote access → package sources → time sync → security policy → base tools) while noting site-specific values (internal mirrors, DNS, VPN ports) as examples rather than universal defaults.

## [2026-04-15] ingest | 基于docker构建ubuntu20.04开发环境

Pages created:
- `wiki/sources/ubuntu2004-docker-dev-environment-setup.md`
- `wiki/concepts/containerized-development-environment.md`

Pages updated:
- `wiki/products/docker.md`
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Added a source summary for creating Ubuntu 20.04 development environments using Docker, covering container keep-alive with `sleep infinity`, APT mirror configuration, and basic tool installation.
- Established `containerized development environment` as a reusable concept for isolated, reproducible development workspaces using containers.
- Expanded the Docker product page to cover both troubleshooting/compatibility scenarios and development environment use cases.
- Added canonical terminology for `容器化开发环境`、`sleep infinity` 和 `APT 镜像源`, and noted USTC mirror as a China-specific site value rather than universal default.

## [2026-04-15] ingest | Netplan 配置指南

Pages created:
- `wiki/sources/netplan-configuration-guide.md`
- `wiki/products/ubuntu.md`
- `wiki/concepts/network-configuration.md`

Pages updated:
- `wiki/products/linux.md`
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Added a comprehensive Netplan source summary covering YAML syntax, configuration hierarchy, DHCP/static IP, multi-NIC, VLAN, bonding, bridging, WiFi, IPv6, MTU, static routes, advanced matching patterns, operational commands, migration from ifupdown, and troubleshooting.
- Established `Ubuntu` as a product page covering APT package management, Netplan network configuration, and containerized development use cases.
- Established `network configuration` as a unified concept page for declarative network interface management, covering configuration layers, distribution-specific approaches, and safe change workflows.
- Added canonical terminology for `Ubuntu`、`Netplan`、`systemd-networkd`、`NetworkManager`、`VLAN`、`Bond`、`Bridge`、`netplan try` 和 `ifupdown`, and expanded the overview to include Ubuntu-specific network configuration knowledge.


## [2026-04-15] ingest | Ubuntu22.04升级OpenSSH版本到最新

Pages created:
- `wiki/sources/ubuntu2204-openssh-upgrade-from-source.md`

Pages updated:
- `wiki/products/openssh.md` — added Ubuntu 22.04 upgrade path, distribution comparison table
- `wiki/products/ubuntu.md` — added OpenSSH source upgrade section
- `wiki/concepts/source-built-package-replacement.md` — added Ubuntu 22.04 patterns, distribution comparison
- `wiki/glossary.md` — added `--without-hardening`, `TCP Wrappers`, `libpam0g-dev` terms
- `wiki/index.md` — added new source, updated product/concept summaries
- `wiki/overview.md` — updated source count, added Ubuntu OpenSSH theme
- `wiki/log.md`

Key additions:
- Added an Ubuntu 22.04 source summary for upgrading OpenSSH from upstream portable source, using openssh-9.5p1 as reference version.
- Documented the simpler Ubuntu upgrade path: uses system OpenSSL (no need to build from source), in-place binary replacement with `--prefix=/usr`, preserves existing systemd unit.
- Expanded OpenSSH product page to cover both CentOS 7 and Ubuntu 22.04 upgrade patterns as independent runbooks with distribution-specific comparison table.
- Expanded source-built package replacement concept to contrast CentOS 7 (separate prefix, RPM removal) vs Ubuntu 22.04 (direct overwrite, no package removal) approaches.
- Flagged `PermitRootLogin yes` and `PasswordAuthentication yes` as temporary convenience settings for recovery access, not production baseline.
- Added canonical terminology for `--without-hardening`, `TCP Wrappers` (deprecated), and `libpam0g-dev`.

## [2026-04-15] ingest | Ubuntu 常见问题与优化

Pages created:
- `wiki/sources/ubuntu-common-issues-and-optimization.md`
- `wiki/concepts/systemd-resolved-dns-management.md`

Pages updated:
- `wiki/products/ubuntu.md` — added system optimization, swap management, NFS mount, DNS troubleshooting, VMware multipath sections
- `wiki/glossary.md` — added `systemd-resolved`, `DNSStubListener`, `multipath`, `swap`, `_netdev` terms
- `wiki/index.md` — added new source and concept entries
- `wiki/overview.md` — updated source count, added Ubuntu optimization themes
- `wiki/log.md`

Key additions:
- Added an Ubuntu source summary covering system optimization areas (sysctl, fstab, swap, NFS), APT package management (download-only, mirror switching, maintenance), and common troubleshooting scenarios (DNS port 53 conflict with systemd-resolved, VMware multipath errors).
- Established `systemd-resolved DNS management` as a reusable concept for understanding and troubleshooting the default DNS resolver service on modern Ubuntu.
- Expanded Ubuntu product page to cover system optimization, swap management, NFS auto-mount, DNS port conflicts, and VMware compatibility.
- Added canonical terminology for `systemd-resolved`、`DNSStubListener`、`multipath`、`swap` 和 `_netdev`, and noted that legacy Ubuntu 16.04 WiFi configuration via wpa_supplicant is deprecated in favor of Netplan.
- Flagged that disabling swap should be done with awareness of OOM risks, and that multipath blacklisting is specific to VMware VMs rather than physical servers.

## [2026-04-15] ingest | Ubuntu切换指定版本内核

Pages created:
- `wiki/sources/ubuntu-kernel-version-switching.md`

Pages updated:
- `wiki/products/ubuntu.md` — added kernel management section with APT install and GRUB configuration
- `wiki/concepts/kernel-upgrade-and-boot-management.md` — expanded to cover Ubuntu patterns alongside CentOS, added distribution comparison table
- `wiki/glossary.md` — added `update-grub`, `linux-image / linux-headers / linux-modules` terms, updated GRUB2 entry
- `wiki/index.md` — added new source, updated Ubuntu and kernel concept summaries
- `wiki/overview.md` — updated source count, added Ubuntu kernel switching theme
- `wiki/log.md`

Key additions:
- Added an Ubuntu source summary for switching to a specific kernel version via APT, covering kernel package installation, GRUB default entry configuration with submenu path syntax, and reboot verification.
- Expanded the kernel upgrade and boot management concept to cover both CentOS 7 (ELRepo, numeric GRUB indices) and Ubuntu (APT, full menu path strings) patterns with a distribution comparison table.
- Expanded the Ubuntu product page to include kernel management as a first-class capability alongside network configuration and system optimization.
- Added canonical terminology for `update-grub` and Ubuntu kernel package naming conventions (`linux-image-*`, `linux-headers-*`, `linux-modules-*`).
- Noted the key difference in GRUB default entry syntax: CentOS uses numeric indices or `grub2-set-default`, while Ubuntu requires the full submenu path string like `"Advanced options for Ubuntu>Ubuntu, with Linux X.Y.Z"`.

## [2026-04-15] ingest | 构建技术研发思维

Pages created:
- `wiki/sources/engineering-thinking-framework.md`
- `wiki/concepts/engineering-mindset.md`
- `wiki/concepts/state-and-data-flow-modeling.md`
- `wiki/concepts/validation-driven-design.md`
- `wiki/personas/growing-engineer.md`

Pages updated:
- `wiki/concepts/full-lifecycle-delivery-capability.md`
- `wiki/concepts/ai-enabled-software-delivery.md`
- `wiki/personas/technical-line-leader.md`
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Added a source summary for an engineering-thinking presentation that frames software R&D as turning real-world problems into computable systems through abstraction, modeling, decomposition, and validation.
- Established `研发思维` as a foundational concept, plus reusable pages for `状态与数据流建模` and `验证驱动设计`.
- Added a `成长型工程师` persona to capture the audience moving from feature implementation toward system-level reasoning and explicit engineering judgment.
- Expanded the delivery and AI synthesis pages to clarify that AI collaboration does not replace human abstraction, model selection, trade-off analysis, or validation responsibility.
- Added canonical terminology for `研发思维`、`抽象`、`建模`、`分治思维`、`状态机思维`、`数据流思维`、`权衡` 和 `验证思维`.

## [2026-04-15] ingest | 快速基于 AI 入门全栈研发

Pages created:
- `wiki/sources/ai-full-stack-development-onboarding.md`
- `wiki/concepts/model-context-protocol.md`
- `wiki/concepts/project-steering-context.md`
- `wiki/concepts/devops-delivery-pipeline.md`
- `wiki/personas/ai-era-full-stack-beginner.md`

Pages updated:
- `wiki/concepts/ai-enabled-software-delivery.md`
- `wiki/concepts/full-lifecycle-delivery-capability.md`
- `wiki/concepts/observability-and-reliability.md`
- `wiki/personas/growing-engineer.md`
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Added a source summary for an AI-era full-stack onboarding presentation that connects local tooling, AI collaboration, DevOps flow, deployment, and testing into one beginner-friendly delivery map.
- Established `Model Context Protocol（MCP）`、`项目 Steering 上下文` and `DevOps 交付流水线` as first-class concepts rather than leaving them embedded in a single slide deck.
- Added an `AI时代全栈研发入门者` persona to represent learners who need a golden path across environment setup, Git/PR, CI/CD, deployment, monitoring, and AI-assisted coding.
- Expanded the AI, delivery, and observability synthesis pages to show that AI-era onboarding still depends on human review, explicit workflow guardrails, and runtime feedback loops.
- Added canonical terminology for `AI IDE`、`CLI Agent`、`MCP`、`Steering 文件`、`DevOps`、`CI/CD 流水线`、`全栈研发`、`Docker Compose` and `Playwright`, and normalized the source typo `playwrite`.

## [2026-04-16] ingest | Linux基础与测试专题

Pages created:
- `wiki/sources/linux-foundations-and-testing-special-topic.md`
- `wiki/concepts/unix-philosophy-and-pipeline-thinking.md`
- `wiki/concepts/use-methodology.md`
- `wiki/concepts/software-testing-architecture.md`
- `wiki/concepts/property-based-testing.md`

Pages updated:
- `wiki/products/linux.md` — added Unix-style design principles, permissions/signals, and resource observability positioning
- `wiki/concepts/linux-command-line-operations.md` — expanded from command usage into mental models, standard streams, and observability mapping
- `wiki/concepts/shell-scripting.md` — added idempotence and fail-fast scripting principles
- `wiki/concepts/observability-and-reliability.md` — grounded observability in Linux host signals, USE, and the `node_exporter -> Prometheus -> Grafana` stack
- `wiki/concepts/devops-delivery-pipeline.md` — added commit/nightly/release testing gates and environment alignment
- `wiki/concepts/full-lifecycle-delivery-capability.md` — expanded delivery scope to include test architecture and runtime observability
- `wiki/glossary.md` — added Unix/testing/observability terminology and style guidance
- `wiki/index.md` — added new source and concept entries, refreshed updated summaries
- `wiki/overview.md` — updated source/page counts and big-picture synthesis
- `wiki/log.md`

Key additions:
- Added a source summary for a training deck that treats Linux as both an operating surface and a reusable engineering philosophy built around small tools, text interfaces, uniform abstractions, and pipeline composition.
- Established `Unix 哲学与管道思维`、`USE 方法`、`软件测试架构` and `属性测试` as standalone reusable concept pages instead of leaving them embedded in a single presentation.
- Expanded Linux-related pages to cover file descriptors, least privilege, signals, resource bottlenecks, and the mapping from `/proc` / `/sys` to monitoring systems.
- Expanded delivery and observability synthesis pages to show how fast feedback, layered testing, regression accumulation, and host-level metrics together form the practical quality loop.
- Added canonical terminology for `Unix 哲学`、`管道思维`、`文件描述符`、`最小权限原则`、`幂等性`、`优雅停机`、`USE 方法`、`测试金字塔`、`属性测试`、`Contract Test`、`回归测试` and `固定用例（Golden Cases）`.

## [2026-04-16] ingest | macOS 安装 U 盘制作

Pages created:
- `wiki/sources/macos-usb-installer-creation.md`
- `wiki/products/macos.md`
- `wiki/concepts/bootable-os-installer-media.md`
- `wiki/concepts/startup-security-and-external-boot-policy.md`

Pages updated:
- `wiki/glossary.md` — added macOS installation, Recovery, startup security, and firmware-reset terminology
- `wiki/index.md` — added new source, product, and concept entries
- `wiki/overview.md` — updated source/page counts and added macOS recovery/install coverage
- `wiki/log.md`

Key additions:
- Added a source summary for a compact macOS runbook covering installer download, USB formatting, `createinstallmedia`, external boot selection, and downgrade preparation from macOS 14 to 13.
- Added a `macOS` product page focused on bootable installer creation, Recovery-mode startup controls, and current coverage boundaries.
- Established `Bootable OS installer media` and `Startup security and external boot policy` as reusable concepts instead of leaving them embedded in one Mac-specific note.
- Added canonical terminology for `macOS`、`可启动安装介质`、`createinstallmedia`、`Recovery 模式`、`启动安全性实用工具`、`NVRAM` and `SMC`.
- Flagged that the source separates installer-media creation from external-boot authorization, but does not yet distinguish Apple silicon vs Intel startup flows or document backup / restore steps.

## [2026-04-17] ingest | macOS 常用命令

Pages created:
- `wiki/sources/macos-common-commands-reference.md`
- `wiki/concepts/macos-command-line-operations.md`

Pages updated:
- `wiki/products/macos.md` — expanded from installer/recovery coverage into broader command-line administration, developer tooling, and desktop automation
- `wiki/sources/macos-usb-installer-creation.md` — added backlink to the broader macOS command-line operations concept
- `wiki/glossary.md` — added Apple/macOS CLI terminology and style guidance for exact command naming
- `wiki/index.md` — added the new source and concept, refreshed macOS product summary, and updated core-file dates
- `wiki/overview.md` — updated source/page counts and broadened the macOS synthesis from installer-only coverage to daily operations coverage
- `wiki/log.md`

Key additions:
- Added a source summary for a broad macOS command cheat sheet covering system inspection, disk and network diagnostics, process control, `launchctl`, power management, Homebrew, Xcode CLI tools, and desktop automation commands.
- Established `macOS command-line operations` as a reusable concept instead of leaving Apple-specific interfaces like `diskutil`, `launchctl`, `pmset`, and `defaults` embedded in one cheat sheet.
- Expanded the `macOS` product page to treat the platform as both a recovery/install surface and a day-to-day developer workstation / local administration surface.
- Added canonical terminology for `Homebrew`、`launchctl`、`pmset`、`diskutil`、`system_profiler`、`defaults` and `Xcode Command Line Tools`.
- Flagged that the source mixes read-only inspection with high-risk commands, and corrected the misleading implication that `rm -rf ~/.Trash/*` is a “safe” trash-emptying action.

## [2026-04-17] ingest | macOS 开发环境配置

Pages created:
- `wiki/sources/macos-development-environment-setup.md`
- `wiki/concepts/language-runtime-version-management.md`

Pages updated:
- `wiki/products/macos.md` — added Development Environment Setup section covering rbenv and CocoaPods
- `wiki/glossary.md` — added `rbenv`, `CocoaPods`, `gem`, `语言运行时版本管理` terms
- `wiki/index.md` — added new source and concept entries, updated macOS product summary
- `wiki/overview.md` — updated source count, added macOS development environment coverage and related pages
- `wiki/log.md`

Key additions:
- Added a source summary for a compact macOS development environment setup note covering Ruby version management with rbenv and CocoaPods installation for iOS/macOS development.
- Established `language runtime version management` as a reusable concept for the pattern of using version managers (rbenv, nvm, pyenv) to isolate language runtimes on development machines.
- Expanded the macOS product page to cover development environment setup as a third operational lens alongside installer/recovery and daily command-line administration.
- Added canonical terminology for `rbenv`、`CocoaPods`、`gem` 和 `语言运行时版本管理`, and documented the common version manager workflow pattern (install → init → shell integration → install version → set default).
- Noted that the source uses `sudo gem install cocoapods` for system-wide installation, and that the shell integration assumes `zsh` as the default macOS shell.

## [2026-04-17] ingest | macOS 系统设置

Pages created:
- `wiki/sources/macos-system-settings.md`

Pages updated:
- `wiki/products/macos.md` — added interactive desktop preferences and browser ergonomics as a first-class workstation layer
- `wiki/concepts/macos-command-line-operations.md` — clarified the boundary between CLI control surfaces, GUI `系统设置`, and app-level preferences
- `wiki/sources/macos-common-commands-reference.md` — linked command-line coverage to the new GUI/system-settings source instead of overextending CLI scope
- `wiki/glossary.md` — added macOS system-settings, input, trackpad, Spotlight, and browser-preloading terminology plus a style rule for GUI labels vs defaults
- `wiki/index.md` — added the new source entry and refreshed macOS summary lines
- `wiki/overview.md` — updated source/page counts and broadened macOS synthesis to include everyday workstation settings
- `wiki/log.md`

Key additions:
- Added a source summary for a compact macOS workstation-settings note covering shortcut preferences, English-first input configuration, trackpad behavior, and a Chrome preload workaround for version-specific typing lag.
- Expanded the `macOS` product page beyond recovery, CLI administration, and development setup to include interactive desktop preferences and browser ergonomics.
- Clarified that many day-to-day Mac experience settings live in GUI `系统设置` or app preferences and should not be flattened into command-only documentation.
- Added canonical terminology for `系统设置（System Settings）`、`Spotlight`、`输入法`、`辅助点按（Secondary click）`、`轻点来点按（Tap to click）` and `网页预加载`.
- Flagged that the source records preferred/user-configured shortcut behavior and a version-sensitive Chrome workaround, so the wiki should preserve them as contextual settings rather than universal macOS defaults.

## [2026-04-17] ingest | Windows 开发相关

Pages created:
- `wiki/sources/windows-development-related.md`
- `wiki/products/windows.md`
- `wiki/products/inno-setup.md`
- `wiki/concepts/windows-management-instrumentation.md`
- `wiki/concepts/hardware-derived-machine-identifier.md`

Pages updated:
- `wiki/products/samba.md` — linked the Samba client-side outcome to the new `Windows` product page
- `wiki/concepts/smb-file-sharing.md` — added a backlink from SMB client behavior to `Windows`
- `wiki/glossary.md` — added Windows-native development terminology and a style rule clarifying `机器码`
- `wiki/index.md` — added new source, product, and concept entries
- `wiki/overview.md` — updated source/page counts and broadened the synthesis to include Windows-native packaging and hardware identification
- `wiki/log.md`

Key additions:
- Added a source summary for a compact Windows-native development note covering Inno Setup installer scripting, install/uninstall service actions, and WMI-based machine identifier generation.
- Added first-class `Windows` and `Inno Setup` product pages instead of leaving Windows knowledge scattered only inside one raw memo or Samba client references.
- Established `Windows Management Instrumentation` and `Hardware-derived machine identifier` as reusable concepts rather than leaving them embedded in code snippets.
- Added canonical terminology for `Windows`、`Inno Setup`、`WMI`、`MFC`、`Win32` and `机器码`, and normalized `机器码` to mean a hardware-derived device identifier rather than compiled machine code.
- Clarified that the source records a minimal implementation path, but does not yet cover Windows packaging topics such as signing, UAC, MSI/WiX, registry operations, service management tooling, or crash diagnostics.
