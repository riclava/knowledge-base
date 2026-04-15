---
title: OpenSSH
type: product
created: 2026-04-15
updated: 2026-04-15
sources: [CentOS7升级OpenSSL和OpenSSH.md, Ubuntu22.04升级OpenSSH版本到最新.md]
tags: [product, openssh, ssh, remote-access, daemon, linux, centos, ubuntu, openssl, source-build, authentication, systemd]
---

OpenSSH 是常见的 SSH 客户端与服务端套件；在当前知识库里，它以"需要在线替换 `sshd` 服务并处理认证回连风险"的远程访问组件出现，覆盖 CentOS 7（依赖自定义 OpenSSL）和 Ubuntu 22.04（使用系统 OpenSSL）两种源码升级路径。

## Product Snapshot

| 项目 | 内容 |
|---|---|
| 产品类型 | 远程登录与安全传输套件 |
| 当前来源中的版本信息 | CentOS 7 来源未固定版本；Ubuntu 22.04 来源以 openssh-9.5p1 为参考 |
| 关键服务对象 | `ssh`、`sshd`、`/etc/ssh/sshd_config`、host keys、PAM、systemd unit |
| 当前来源中的构建方式 | 从源码编译，通过 `--with-ssl-dir` 指向目标 OpenSSL 前缀 |
| CentOS 7 切换方式 | 强制删除旧 RPM、安装源码版本、复制 init/PAM 文件、重启 `sshd` |
| Ubuntu 22.04 切换方式 | 直接覆盖系统二进制（`--prefix=/usr`），保留现有 systemd unit |
| 最小验证路径 | `systemctl restart ssh[d]` -> `ssh -V` |

## Core Capability Surface in This Source

### Portable Source Build Against a Chosen Crypto Library

- 两个来源都把 OpenSSH 当作一个可重新链接的上层服务，而不是只能使用发行版默认包。
- CentOS 7 来源使用 `--with-ssl-dir=/usr/local/openssl` 指向自建 OpenSSL。
- Ubuntu 22.04 来源使用 `--with-ssl-dir=/usr` 直接使用系统 OpenSSL（通常已足够新）。

### Service Continuity Is Part of the Upgrade Problem

- 两个来源都不是在离线镜像里构建再部署，而是在可能正通过 SSH 登录的机器上直接替换服务。
- 这让 OpenSSH 升级天然带有"如果切换失败就会失联"的运维风险。
- 两个来源都强调保持当前终端会话，并建议使用 `screen` 或 `tmux`。

### Authentication Policy and Availability Interact

- 两个来源都通过调整 `PermitRootLogin` 和 `PasswordAuthentication` 来降低回连失败概率。
- 这些是临时便利设置，应在验证成功后恢复到安全基线。
- 这说明 OpenSSH 文档不仅要讲软件安装，还要讲认证策略如何影响升级窗口中的可达性。

### Host Key and PAM Integration Cannot Be Skipped

- 除了编译和安装，来源还额外处理了 host key 权限和 PAM 集成。
- CentOS 7 来源需要额外处理 init 脚本；Ubuntu 22.04 来源保留现有 systemd unit。
- 这表明 OpenSSH 从源码安装后，能否真正作为系统服务启动，取决于服务配套文件是否完整接上。

### Distribution-Specific Differences

| 方面 | CentOS 7 | Ubuntu 22.04 |
|------|----------|--------------|
| OpenSSL 依赖 | 通常需要先源码构建 OpenSSL | 使用系统 OpenSSL（通常足够新） |
| 安装前缀 | `/usr/local/openssl` + `/usr` | `/usr`（直接覆盖） |
| 包移除 | 可能需要 `rpm -e --nodeps` | 不需要（直接覆盖） |
| 服务名 | `sshd` | `ssh` |
| 服务配置 | 可能需要 init 脚本或 unit 文件 | 保留现有 systemd unit |
| TCP Wrappers | 不支持（新版本已移除） | 不支持 |

## Why It Matters

- OpenSSH 是远程维护 Linux 主机最常见的入口，一旦升级过程出错，影响通常比普通工具升级更直接。
- 对技术写作来说，OpenSSH 文档必须同时覆盖版本来源、认证策略、服务管理、配置校验和回滚路径。
- 在知识库里，它连接了 [[openssl]] 的加密库依赖、[[centos]] 和 [[ubuntu]] 的发行版差异、以及 [[source-built-package-replacement]] 的整体升级模式。

## Documentation Notes

- 应在任何升级文档中强调保留现有会话、准备控制台或备用连接方式，不要只写命令步骤。
- 应在重启前加入 `sshd -t` 一类配置校验，并说明如何在单独端口或备用会话上做验证。
- 应把 `PermitRootLogin yes`、`PasswordAuthentication yes` 等设置标明为临时回连策略，而不是长期基线。
- 应区分"发行版包升级"和"源码替换系统 OpenSSH"两种维护路径，并说明各自适用条件。

## Known Gaps from This Source

- 没有覆盖 `scp` / `sftp`、密钥登录迁移、host key 轮换、加密套件策略、`sshd -t` 校验细节。
- 没有给出更稳妥的并行验证方案，例如备用端口、第二个实例或控制台回滚。
- 没有涉及 SELinux/AppArmor 上下文、审计日志、fail2ban 或 SSH 加固基线。
- 没有讨论何时应优先采用发行版补丁而非源码替换。

## Related Pages

- [[centos7-openssl-and-openssh-upgrade-from-source]] — CentOS 7 source upgrade runbook
- [[ubuntu2204-openssh-upgrade-from-source]] — Ubuntu 22.04 source upgrade runbook
- [[centos]] — CentOS distribution page
- [[ubuntu]] — Ubuntu distribution page
- [[linux]] — Linux platform page
- [[openssl]] — TLS/crypto library page
- [[source-built-package-replacement]] — source-compiled software replacement concept
- [[linux-command-line-operations]] — Linux operations concept
- [[glossary]] — terminology reference
- [[overview]] — knowledge base synthesis
