---
title: OpenSSL
type: product
created: 2026-04-15
updated: 2026-04-15
sources: [CentOS7升级OpenSSL和OpenSSH.md]
tags: [product, openssl, tls, crypto, ssl, linux, centos, source-build, shared-libraries]
---

OpenSSL 是常见的 TLS/加密库与命令行工具；在当前知识库里，它首先以“CentOS 7 上需要通过源码编译安装到自定义前缀，再供 OpenSSH 重新链接”的基础依赖出现。

## Product Snapshot

| 项目 | 内容 |
|---|---|
| 产品类型 | 加密库 / TLS 工具包 / 命令行工具 |
| 当前来源中的示例版本 | `openssl-1.1.1l` |
| 当前来源中的安装位置 | `/usr/local/openssl` |
| 当前来源中的系统接入点 | `/usr/bin/openssl` 符号链接、`/etc/ld.so.conf`、`ldconfig` |
| 典型下游依赖 | OpenSSH `--with-ssl-dir=/usr/local/openssl` |
| 典型使用场景 | TLS 能力补齐、命令行版本检查、为其他源码软件提供更新的加密库 |

## Core Capability Surface in This Source

### Upstream Source Installation on a Legacy Distro

- 当前来源不是通过 `yum update openssl` 管理 OpenSSL，而是直接从上游下载 tarball 并执行 `./config`、`make install`。
- 这说明在 CentOS 7 这样的遗留发行版上，OpenSSL 可能被当成“需要绕过发行版包版本”的组件来处理。

### Shared Library and CLI Exposure Are Both Part of the Cutover

- 仅把新版本编译出来并不够，来源还显式替换了 `/usr/bin/openssl` 并刷新动态链接器缓存。
- 这意味着 OpenSSL 在这类维护里既是命令行工具，也是其他程序运行时要加载的共享库集合。

### Dependency Provider for Higher-Level Services

- 来源的真正业务目标不是单独使用 OpenSSL，而是让 OpenSSH 能链接到更新的加密库。
- 因此 OpenSSL 文档不能只写版本号和安装命令，还要说明哪些下游服务会受它影响。

### Manual Replacement Changes Ownership and Rollback Rules

- 当 OpenSSL 安装到 `/usr/local/openssl` 后，再用符号链接把它接进系统路径，就不再完全受 RPM 管理。
- 这让升级、回滚、库路径冲突和兼容性验证都变成文档必须覆盖的内容。

## Why It Matters

- OpenSSL 往往不是孤立升级对象，而是很多网络服务和客户端工具的共同底层依赖。
- 对技术写作来说，OpenSSL 文档需要同时说明“新版本装在哪里”“系统怎么找到它”“哪些服务要重新编译或重启”。
- 在知识库中，它把 [[linux]] 的动态链接机制、[[centos]] 的遗留发行版现实和 [[openssh]] 的远程访问服务串了起来。

## Documentation Notes

- 应固定具体版本、下载源和校验方式，不要把“latest”当成稳定文档术语。
- 应区分“并行安装在自定义前缀”与“替换系统默认二进制/库路径”这两类风险不同的做法。
- 如果要修改动态链接器配置，最好写明配置文件位置、回滚方式和对其他依赖者的影响。
- 如果目标是服务升级，应同步说明哪些下游组件需要重新编译、重启或重新验证。

## Known Gaps from This Source

- 没有覆盖证书存储、CA bundle、FIPS、provider/engine、`openssl.cnf` 或更现代的包管理替代方案。
- 没有给出与系统自带库共存时的 ABI 风险评估或兼容矩阵。
- 没有提供校验 tarball、回滚符号链接或使用 `ld.so.conf.d` 细分配置文件的更稳妥做法。

## Related Pages

- [[centos7-openssl-and-openssh-upgrade-from-source]]
- [[centos]]
- [[linux]]
- [[openssh]]
- [[source-built-package-replacement]]
- [[linux-command-line-operations]]
- [[glossary]]
- [[overview]]
