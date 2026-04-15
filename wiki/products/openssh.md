---
title: OpenSSH
type: product
created: 2026-04-15
updated: 2026-04-15
sources: [CentOS7升级OpenSSL和OpenSSH.md]
tags: [product, openssh, ssh, remote-access, daemon, linux, centos, openssl, source-build, authentication]
---

OpenSSH 是常见的 SSH 客户端与服务端套件；在当前知识库里，它首先以“CentOS 7 上依赖自定义 OpenSSL、需要在线替换 `sshd` 服务并处理认证回连风险”的远程访问组件出现。

## Product Snapshot

| 项目 | 内容 |
|---|---|
| 产品类型 | 远程登录与安全传输套件 |
| 当前来源中的版本信息 | 未固定版本，只说明从 OpenBSD portable 目录下载“最新版本” |
| 关键服务对象 | `ssh`、`sshd`、`/etc/ssh/sshd_config`、host keys、PAM、启动脚本 |
| 当前来源中的构建方式 | 从源码编译，并通过 `--with-ssl-dir` 指向目标 OpenSSL 前缀 |
| 当前来源中的切换方式 | 强制删除旧 RPM、安装源码版本、复制 init/PAM 文件、重启 `sshd` |
| 最小验证路径 | `systemctl restart sshd` -> `ssh -V` |

## Core Capability Surface in This Source

### Portable Source Build Against a Chosen Crypto Library

- 当前来源把 OpenSSH 当作一个可重新链接的上层服务，而不是只能使用发行版默认包。
- `--with-ssl-dir=/usr/local/openssl` 说明它的构建结果受底层 OpenSSL 前缀直接影响。

### Service Continuity Is Part of the Upgrade Problem

- 来源不是在离线镜像里构建再部署，而是在可能正通过 SSH 登录的机器上直接删除旧包并替换服务。
- 这让 OpenSSH 升级天然带有“如果切换失败就会失联”的运维风险。

### Authentication Policy and Availability Interact

- 来源通过调整 `PermitRootLogin` 和 `PasswordAuthentication` 来降低回连失败概率。
- 这说明 OpenSSH 文档不仅要讲软件安装，还要讲认证策略如何影响升级窗口中的可达性。

### Host Key and PAM Integration Cannot Be Skipped

- 除了编译和安装，来源还额外处理了 host key 权限、init 脚本和 PAM 文件。
- 这表明 OpenSSH 从源码安装后，能否真正作为系统服务启动，取决于服务配套文件是否完整接上。

## Why It Matters

- OpenSSH 是远程维护 Linux 主机最常见的入口，一旦升级过程出错，影响通常比普通工具升级更直接。
- 对技术写作来说，OpenSSH 文档必须同时覆盖版本来源、认证策略、服务管理、配置校验和回滚路径。
- 在知识库里，它连接了 [[openssl]] 的加密库依赖、[[centos]] 的遗留系统升级现实和 [[source-built-package-replacement]] 的整体升级模式。

## Documentation Notes

- 应在任何升级文档中强调保留现有会话、准备控制台或备用连接方式，不要只写命令步骤。
- 应在重启前加入 `sshd -t` 一类配置校验，并说明如何在单独端口或备用会话上做验证。
- 应把 `PermitRootLogin yes`、`PasswordAuthentication yes` 等设置标明为临时回连策略，而不是长期基线。
- 应区分“发行版包升级”和“源码替换系统 OpenSSH”两种维护路径，并说明各自适用条件。

## Known Gaps from This Source

- 没有覆盖 `scp` / `sftp`、密钥登录迁移、host key 轮换、加密套件策略、`sshd -t` 校验或 systemd unit 细节。
- 没有给出更稳妥的并行验证方案，例如备用端口、第二个实例或控制台回滚。
- 没有涉及 SELinux 上下文、审计日志、fail2ban 或 SSH 加固基线。

## Related Pages

- [[centos7-openssl-and-openssh-upgrade-from-source]]
- [[centos]]
- [[linux]]
- [[openssl]]
- [[source-built-package-replacement]]
- [[linux-command-line-operations]]
- [[glossary]]
- [[overview]]
