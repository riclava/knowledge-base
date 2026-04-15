---
title: CentOS7升级OpenSSL和OpenSSH
type: source
created: 2026-04-15
updated: 2026-04-15
sources: [CentOS7升级OpenSSL和OpenSSH.md]
tags: [source, centos, linux, openssl, openssh, ssh, tls, source-build, package-management, service-management, remote-access]
---

这是一份在 CentOS 7 上先编译 OpenSSL、再从源码重编译 OpenSSH 并切换 `sshd` 服务的操作备忘，重点在依赖顺序、系统替换动作和远程登录不中断，但它同时包含多处明显的安全退让和高风险维护动作。

## Source Metadata

- Raw source: `raw/OS/Linux/CentOS/CentOS7升级OpenSSL和OpenSSH.md`
- 时间标记：未注明
- 文档性质：升级操作笔记 / 维护备忘
- 主要对象：CentOS 7、OpenSSL、OpenSSH、`/usr/local/openssl`、`/etc/ld.so.conf`、`/etc/ssh/sshd_config`
- 示例版本：`openssl-1.1.1l.tar.gz`；OpenSSH 版本未写明，只说明“下载最新版本”
- 适用场景：CentOS 7 自带 OpenSSL/OpenSSH 版本不足，需要通过上游源码手工升级 SSH/TLS 相关组件时

## What the Source Covers

| 阶段 | 操作 | 观察到的意图 |
|---|---|---|
| 准备 OpenSSL 编译环境 | `yum install gcc gcc-c++ autoconf automake zlib zlib-devel pcre-devel -y` | 先准备源码编译所需的工具链与压缩库依赖 |
| 下载并解压 OpenSSL | `wget`、`tar -xzf` | 引入比系统仓库更新的上游 OpenSSL 源码包 |
| 编译安装 OpenSSL | `./config shared --openssldir=/usr/local/openssl --prefix=/usr/local/openssl`、`make install` | 把新 OpenSSL 安装到独立前缀，而不是直接覆盖系统目录 |
| 让系统发现新 OpenSSL | 备份 `/usr/bin/openssl`、创建符号链接、追加 `/etc/ld.so.conf`、执行 `ldconfig` | 让命令路径和动态链接器都能找到新版本 |
| 准备 OpenSSH 编译依赖 | 安装 `wget`、`gcc`、`zlib-devel`、`openssl-devel`、`pam-devel`、`libselinux-devel` | 为 OpenSSH 源码编译补齐依赖 |
| 获取并准备 OpenSSH 源码 | 从 OpenBSD portable 目录下载并解压 | 使用上游 portable 版本替代发行版 RPM |
| 移除旧 OpenSSH RPM | `rpm -e --nodeps \`rpm -qa | grep openssh\`` | 避免文件冲突并为源码安装腾位置，但牺牲包管理安全网 |
| 按目标 OpenSSL 路径重编译 OpenSSH | `./configure ... --with-ssl-dir=/usr/local/openssl ...`、`make install` | 显式把 OpenSSH 绑定到选定的 OpenSSL 前缀 |
| 恢复服务配套文件 | 调整 host key 权限、复制 `sshd.init` 和 `sshd.pam`、赋予脚本可执行权限 | 让源码安装出来的二进制重新接上系统启动与认证链路 |
| 放宽认证策略 | 修改 `PermitRootLogin` 与 `PasswordAuthentication` | 优先保证切换后能重新连入主机 |
| 启用并验证服务 | `chkconfig`、`systemctl restart sshd`、`ssh -V` | 让新服务开机自启并确认客户端版本已经变化 |

## Core Mental Model Implied by the Source

1. OpenSSH 依赖 OpenSSL，因此升级顺序必须先库、后服务。
2. 源码编译安装和系统真正切换是两件事；真正切换还依赖符号链接、动态库路径、服务脚本和 PAM 配置。
3. 远程登录组件的升级被当作一次“在线割接”处理，所以来源明确提醒不要断开当前终端会话。
4. 这份笔记优先追求“升级后还能连上机器”，而不是默认保持最严格的安全策略。

## Notable Operational Patterns

### Install Dependency Libraries Before Rebuilding Dependents

- 来源没有直接先装 OpenSSH，而是先把 OpenSSL 安装到自定义前缀，再在 OpenSSH `configure` 时显式指定 `--with-ssl-dir`。
- 这说明它把 OpenSSL 看作 SSH 栈的下游依赖提供者，而不是一个孤立工具。

### Separate Compilation from System Cutover

- OpenSSL 先被装到 `/usr/local/openssl`，并未在编译阶段直接写进 `/usr`。
- 真正的系统替换动作发生在后续的符号链接、`ldconfig`、RPM 删除和服务重启阶段。

### Treat Remote Access Replacement as a Session-Sensitive Maintenance Window

- 来源特别警告在执行 `rpm -e --nodeps` 之后不要断开会话，说明它默认操作者正在通过 SSH 连接目标主机。
- 这种维护方式把“会话保活”和“服务切换是否成功”变成同一个风险面。

### Use Authentication Relaxation as a Recovery Aid

- 把 `PermitRootLogin` 改成 `yes`，并打开 `PasswordAuthentication`，反映出来源把“先确保能登回去”放在首位。
- 这类设置更像临时回连保险，而不是长期推荐配置。

## Caveats and Verification Risks

- 标题写的是“升级到最新版本”，但实际只给了 `openssl-1.1.1l` 这个示例版本，OpenSSH 版本也没有固定，因此它更像时间敏感的现场笔记，而不是可长期复用的“latest”文档。
- 把 `/usr/bin/openssl` 改成指向 `/usr/local/openssl/bin/openssl`，并把新库路径直接追加到全局 `/etc/ld.so.conf`，会绕开 RPM 文件归属与依赖管理；正式文档应明确回滚方式和影响面。
- `rpm -e --nodeps` 强制删除 OpenSSH RPM 是高风险动作；如果编译、配置或重启失败，主机可能失去远程登录能力。
- `--with-md5-passwords` 与 `--without-hardening` 都是明显的版本敏感或安全退让参数；前者在较新的 OpenSSH 版本中可能已不合适，后者则主动关闭部分强化保护。
- 直接开启 `PermitRootLogin yes` 和 `PasswordAuthentication yes` 有助于回连，但不应被记录成长期安全基线。
- 来源把 `contrib/redhat/sshd.pam` 复制到 `/etc/pam.d/sshd.pam`，而常见 PAM 目标文件通常是 `/etc/pam.d/sshd`；这一步应按目标主机现状额外核实。
- 来源没有给出 tarball 校验、`sshd -t` 配置检查、备用会话/控制台回滚、systemd unit 适配或灰度验证步骤，因此仍更接近个人备忘而不是生产变更手册。

## Documentation Implications

- 应把这份内容写成“CentOS 7 上通过源码编译升级 OpenSSL/OpenSSH 的高风险 runbook”，而不是泛化为通用的“升级到最新版本”指南。
- 应把“编译安装”“系统切换”“服务验证”“失败回滚”拆成独立阶段，而不是压缩成一串连续命令。
- 应明确标注哪些认证设置是临时回连措施，避免被误读为默认安全建议。
- 如果和其他 Linux 升级资料一起使用，应补上“何时优先选择发行版包更新，何时才需要源码构建”的判断条件。

## Related Pages

- [[centos]]
- [[linux]]
- [[openssl]]
- [[openssh]]
- [[source-built-package-replacement]]
- [[linux-command-line-operations]]
- [[glossary]]
- [[overview]]
