---
title: Samba
type: product
created: 2026-04-15
updated: 2026-04-17
sources: [CentOS7配置Samba共享.md]
tags: [product, samba, smb, centos, linux, windows, file-sharing, authentication, permissions]
---

Samba 是 Linux 上实现 SMB/CIFS 文件共享的服务套件；在当前知识库里，它以“把 CentOS 7 目录映射给 Windows 客户端”的跨系统共享方案出现。

## Product Snapshot

| 项目 | 内容 |
|---|---|
| 产品类型 | 文件共享服务 / SMB 服务器 |
| 当前来源中的安装方式 | `yum -y install samba` |
| 核心配置文件 | `/etc/samba/smb.conf` |
| 当前来源中的认证模型 | `security = user`、`passdb backend = tdbsam`、`smbpasswd -a` |
| 当前来源中的共享示例 | `[data] -> /data` |
| Windows 访问形式 | `\\\\IP\\data` 网络驱动器映射 |
| 当前来源中的主要风险 | 通过关闭 `firewalld`、`iptables` 和 `SELinux` 来换取最小可用性 |

## Core Capability Surface in This Source

### Share Definitions Are Plain Text and Explicit

- 当前来源通过 `/etc/samba/smb.conf` 里的 `[global]` 和 `[data]` 段来声明服务行为。
- 这让 Samba 很适合被写成操作文档：工作组、服务器名、共享路径、允许用户和权限掩码都能以纯文本配置表达。

### Authentication Has a Samba-Specific Layer

- 来源同时使用 `useradd` 和 `smbpasswd -a`，说明 Samba 登录并不只依赖 Linux 本地账户存在。
- `security = user`、`valid users` 和 `write list` 共同定义了谁能访问共享、谁具备写入权限。

### File-System Ownership Still Governs the Real Data Path

- 当前来源用 `chown -R ricl.ricl /data` 把共享目录归属到示例账号。
- 这说明 Samba 更像访问代理层，底层目录的 Linux 权限依然是成败关键。

### Windows Interoperability Is the User-Facing Outcome

- 用户最终不是记住 Linux 路径，而是在 [[windows]] 中通过 `\\\\IP\\data` 映射网络驱动器。
- 因此技术写作不能只停留在服务端命令，还要解释客户端如何连接、凭据如何对应以及失败时看哪里。

## Why It Matters

- Samba 是 Linux 与 Windows 之间最常见的共享文件桥梁之一，适合作为“跨系统权限和服务配置如何配合”的教学案例。
- 对知识库来说，它把 [[linux]] 的目录与用户权限、[[centos]] 的服务管理方式，以及 [[smb-file-sharing]] 这种更抽象的共享模式连到了一起。
- 对技术写作来说，Samba 文档天然需要跨越服务端配置和客户端使用两个视角。

## Documentation Notes

- 应明确区分“Linux 用户存在”“Samba 密码已设置”“Windows 客户端成功认证”这三个不同检查点。
- 不应把关闭 `firewalld`、`iptables` 或 `SELinux` 当成默认步骤；正式文档应优先提供最小暴露和最小放行方案。
- 应给出修改配置后的验证方法，例如配置检查、服务状态、端口监听和客户端访问测试。
- 当示例使用 `create mask` 和 `directory mask` 时，应解释这些字段会怎样影响 Windows 侧新建文件和目录的权限。

## Known Gaps from This Source

- 没有覆盖域集成、Active Directory、ACL、`smbclient`、打印共享、名称广播或多共享目录设计。
- 没有解释 `nmb`、防火墙端口、SELinux 上下文或更细致的权限继承策略。
- 没有形成 Samba 与 NFS、SSHFS 等替代方案的对比知识。

## Related Pages

- [[centos7-samba-share-setup]]
- [[smb-file-sharing]]
- [[windows]]
- [[centos]]
- [[linux]]
- [[linux-command-line-operations]]
- [[glossary]]
- [[overview]]
