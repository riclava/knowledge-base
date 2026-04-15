---
title: Kernel upgrade and boot management
type: concept
created: 2026-04-15
updated: 2026-04-15
sources: [CentOS7离线安装docker问题排查.md, CentOS7升级内核.md, Ubuntu切换指定版本内核.md]
tags: [concept, kernel, linux, centos, ubuntu, grub, bootloader, elrepo, apt, compatibility, operations]
---

Kernel upgrade and boot management 是为了满足兼容性、驱动或运行时需求而有控制地升级或切换 Linux 内核版本、选择默认启动项并在重启后验证结果的实践；它强调"装上新内核"和"实际运行在新内核上"是两个不同阶段。

## Definition

在当前知识库里，这个概念不是泛指所有系统更新，而是特指一条与宿主机基线直接相关的运维链路：

- 先识别当前运行内核、发行版版本和触发升级的兼容性压力
- 再选择内核来源与分支，例如继续使用发行版官方维护分支，或引入 ELRepo（CentOS）或 PPA（Ubuntu）之类的替代仓库
- 安装新内核包，同时保留旧内核作为回滚路径
- 显式管理 GRUB 默认启动项，而不是假设系统会自动切换
- 重启并用针对场景的验证命令确认问题是否真的解决

## Distribution-Specific Patterns

| 操作面 | CentOS 7 | Ubuntu |
|---|---|---|
| 包管理器 | YUM | APT |
| 内核搜索 | `yum list available kernel*` | `apt search linux-image-*` |
| 内核包命名 | `kernel`, `kernel-tools`, `kernel-headers` | `linux-image-*`, `linux-headers-*`, `linux-modules-*` |
| 第三方内核 | ELRepo (`kernel-lt`, `kernel-ml`) | PPA 或手动下载 |
| GRUB 配置 | `/etc/default/grub` | `/etc/default/grub` |
| GRUB 更新 | `grub2-mkconfig -o /boot/grub2/grub.cfg` | `update-grub` |
| 默认项设置 | 数字索引或 `grub2-set-default` | 完整菜单路径字符串 |
| 默认项格式 | `0`, `1`, ... | `"Advanced options for Ubuntu>Ubuntu, with Linux X.Y.Z"` |

## Operational Surfaces

| 操作面 | 典型命令 / 对象 | 典型要回答的问题 |
|---|---|---|
| 运行内核基线 | `uname -r`、`cat /etc/os-release` | 当前到底跑在哪个内核和哪个发行版上 |
| 内核来源与信任 | `yum`/`apt`、ELRepo/PPA、GPG key、repo 文件 | 新内核从哪里来，系统是否信任这个来源 |
| 内核分支选择 | `kernel-lt`、`kernel-ml`、发行版官方内核 | 应该追求长期维护、更新主线，还是只补齐发行版维护版本 |
| 启动项可见性 | `/boot/grub*/grub.cfg`、`menuentry` 枚举 | 系统现在到底有哪些可启动内核条目 |
| 默认启动项控制 | `grub2-set-default`、`/etc/default/grub` | 下次重启会默认进入哪个内核 |
| 配置落盘 | `grub2-mkconfig`、`update-grub` | 当前默认项和实际 GRUB 配置是否一致 |
| 激活后验证 | `reboot`、`uname -r`、`ip netns list-id`、最小 Docker 端口测试 | 新内核是否真的生效，是否解决了原始兼容性问题 |
| 清理与回滚 | `rpm -qa \| grep kernel`、`dpkg -l \| grep linux-image`、可选删除旧内核 | 哪些版本仍保留，失败时能否切回旧内核 |

## Why It Matters

- 它把"升级内核"从零散命令整理成了一个更安全的变更模型。
- 在当前知识库中，Docker 网络异常案例说明宿主机内核基线会直接决定运行时行为；内核升级页则补上了如何执行这一变更的具体路径。
- 对技术文档来说，它能避免把"包装进系统"误写成"已经生效"，也能提醒作者保留回滚视角。

## Common Misconceptions

| 误解 | 更准确的理解 |
|---|---|
| 安装了新内核包，系统就已经切到新内核 | 当前运行内核只有在成功重启并进入对应启动项后才会改变 |
| 解决兼容性问题只有一种升级路线 | CentOS 支持官方维护内核或 ELRepo 替代内核；Ubuntu 支持官方仓库内核或 PPA/手动安装 |
| 旧内核应该尽快删掉以免混乱 | 旧内核首先是回滚保险，清理应放在验证之后 |
| `grub2-set-default 0` 永远安全且稳定 | 数字索引依赖当前 `menuentry` 顺序，升级后应再次核对 |
| 内核包装好了，相关工具包也一定自动对齐 | `kernel-tools*`、headers 和调试工具可能仍需单独检查或补齐 |
| GRUB 默认项语法在所有发行版上相同 | CentOS 常用数字索引，Ubuntu 需要完整的子菜单路径字符串 |

## Documentation Implications

- 文档应把"为什么升级"与"选择哪条升级路径"写清楚，而不是只给安装命令。
- 应区分发行版官方维护内核和第三方替代内核的支持边界、风险和适用场景。
- 应把 GRUB 默认项检查、配置重建和重启后验证写成标准步骤。
- 如果升级是为了修复 Docker、驱动或网络命名空间问题，验证命令应直接对准原始故障面，而不是只写 `uname -r`。
- 应明确哪些步骤属于可选优化，例如镜像替换、旧内核清理或额外工具包安装。
- 应注明不同发行版的 GRUB 默认项语法差异，避免读者照搬命令后失败。

## Related Pages

- [[centos7-kernel-upgrade-via-elrepo]] — CentOS 7 kernel upgrade via ELRepo
- [[ubuntu-kernel-version-switching]] — Ubuntu kernel version switching runbook
- [[centos7-offline-docker-install-troubleshooting]] — Docker troubleshooting that led to kernel upgrade
- [[centos]] — CentOS distribution page
- [[ubuntu]] — Ubuntu distribution page
- [[linux]] — Linux platform page
- [[docker]] — Docker runtime page
- [[container-network-namespace-support]] — kernel-dependent container networking concept
- [[linux-command-line-operations]] — Linux command-line operations concept
- [[glossary]] — terminology reference
- [[overview]] — knowledge base overview
