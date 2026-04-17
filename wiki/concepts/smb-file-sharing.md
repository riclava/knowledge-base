---
title: SMB file sharing
type: concept
created: 2026-04-15
updated: 2026-04-17
sources: [CentOS7配置Samba共享.md]
tags: [concept, smb, samba, linux, windows, file-sharing, authentication, permissions]
---

SMB file sharing 指通过 SMB/CIFS 协议把一台主机上的目录暴露给远端客户端访问；在当前知识库里，它具体体现为用 Samba 把 CentOS 7 上的 `/data` 映射成 [[windows]] 网络驱动器。

## Definition

在当前来源中，SMB file sharing 不是单一命令，而是一条跨多层边界的访问链路：

- Linux 主机上必须存在真实目录和正确的文件所有权
- Samba 必须声明共享段、认证模式和允许访问的账号
- [[windows]] 客户端必须通过正确的 `\\\\server\\share` 路径和凭据接入
- 宿主机网络与安全控制必须允许这条访问路径真正打通

## Operational Layers in This Source

| 层次 | 当前来源中的对象 | 要回答的问题 |
|---|---|---|
| 文件系统层 | `/data`、`chown -R ricl.ricl /data` | 共享背后的真实目录是谁拥有、能否写入 |
| Samba 配置层 | `/etc/samba/smb.conf`、`[data]`、`valid users`、`write list` | 哪个路径被共享、谁能访问、谁能写 |
| 认证层 | `security = user`、`tdbsam`、`smbpasswd -a ricl` | 登录凭据存在哪里、Linux 用户是否已绑定 Samba 密码 |
| 服务层 | `systemctl enable smb --now` | 共享服务是否已启动并可持续运行 |
| 客户端接入层 | Windows 映射 `\\\\IP\\data` | 用户如何在 Windows 中消费这个共享 |
| 安全边界层 | `firewalld`、`iptables`、`SELinux` | 共享为什么不通，是服务问题还是主机策略在拦截 |

## Why It Matters

- 它把“共享目录”拆成了更可解释的多个技术层面，有助于避免把所有问题都归因为某一条命令没执行。
- 它是理解跨系统文件访问的典型案例，因为 Linux 权限模型、服务端认证和 Windows 客户端路径习惯会在同一流程里相遇。
- 对技术文档来说，这个概念能帮助作者组织成“服务端配置 -> 权限准备 -> 客户端访问 -> 验证与排障”的稳定结构。

## Common Misconceptions

| 误解 | 更准确的理解 |
|---|---|
| 安装 `samba` 包就等于共享已经能用 | 安装只是开始，还要定义共享、建立 Samba 密码、准备目录权限并启动服务 |
| 有 Linux 用户就能直接登录 SMB 共享 | 当前来源明确显示还要执行 `smbpasswd -a`，说明 Samba 凭据层是独立存在的 |
| 共享不通时直接关掉防火墙和 SELinux 就是标准做法 | 这最多是快速排除法；正式文档应优先解释如何按最小权限开放和验证 |
| Windows 映射网络驱动器只是客户端动作 | 它依赖服务端共享名、认证、目录权限和主机安全边界全部配合成功 |

## Documentation Implications

- 文档应按“目录准备、共享定义、账号配置、服务启动、客户端接入、验证”分阶段书写。
- 应分别给出 Linux 端验证和 Windows 端验证，而不是只展示客户端截图或只展示服务端命令。
- 应把“为了快速验证而暂时简化安全控制”和“正式环境建议做法”明确拆开写。
- 应在术语上区分 `Samba`（实现/服务）与 `SMB/CIFS`（协议/共享方式）。

## Related Pages

- [[centos7-samba-share-setup]]
- [[samba]]
- [[windows]]
- [[centos]]
- [[linux]]
- [[linux-command-line-operations]]
- [[glossary]]
- [[overview]]
