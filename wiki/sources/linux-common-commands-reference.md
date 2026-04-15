---
title: Linux 常用命令
type: source
created: 2026-04-15
updated: 2026-04-15
sources: [commands.md]
tags: [linux, command-line, operations, system-administration, cheat-sheet, source]
---

这是一份面向 Linux 命令行日常运维与排障的常用命令速查，覆盖文件、文本、磁盘、进程、网络、权限、压缩、定时任务和日志操作。

## Source Metadata

- Raw source: `raw/OS/Linux/Common/commands.md`
- 文档性质：Linux 命令速查 / 运维操作笔记
- 主要对象：文件操作、文本处理、磁盘与存储、进程管理、网络相关、系统信息、用户与权限、压缩与解压、定时任务、日志查看
- 适用场景：服务器巡检、基础排障、环境维护、开发辅助运维、终端值守操作

## What the Source Covers

| 模块 | 覆盖内容 | 价值 |
|---|---|---|
| 文件操作 | `cp -p/-rp`、`rsync -a/-avxP/-avzP`、inode 删除、`find` 多条件查找 | 处理复制、同步、异常文件名清理和批量定位问题文件 |
| 文本处理 | `grep`、`sed`、`awk`、`sort`、`uniq`、`wc`、`cut`、`paste`、`diff` | 把命令输出转化为可筛选、可替换、可统计的信息流 |
| 磁盘与存储 | `df`、`du`、`dd`、direct I/O 测试 | 观察空间占用并做基础读写性能验证 |
| 进程管理 | `ps`、`top`、`htop`、`pstree`、僵尸进程处理、`nohup`、`jobs`、`fg/bg` | 观察运行态、定位资源占用和管理前后台任务 |
| 网络相关 | `ss`、`ip route`、`ip addr`、`dig`、`ping`、`nc`、`mtr`、`firewall-cmd` | 处理端口、路由、DNS、连通性与防火墙配置 |
| 系统与权限 | `uname`、`lscpu`、`free`、`useradd/usermod/userdel`、`chmod/chown` | 巡检系统环境并控制用户、组和权限边界 |
| 压缩、定时任务与日志 | `tar`、`zip/unzip`、`crontab`、`tail -f`、`journalctl`、`logrotate` | 处理归档、周期性任务和问题追踪日志 |

## Core Mental Model Implied by the Source

这份来源虽然是命令清单，但隐含了一套非常典型的 Linux 操作顺序：

1. 先观察状态，再修改系统，例如先用 `df`、`du`、`ps`、`ss`、`journalctl`、`ls -i` 明确对象。
2. 用文本工具把输出压缩成可判断的信息，例如 `grep`、`awk`、`sort`、`wc`。
3. 再执行带副作用的变更，如 `rsync`、`sed -i`、`chmod`、`firewall-cmd`、`crontab -e`。
4. 最后回到验证步骤，确认端口、权限、日志和任务调度是否真的生效。

## Notable Operational Patterns

### Prefer Tool Families Over a Single "万能命令"

- 来源不是围绕一个超大工具展开，而是按资源对象组织多个小工具族：文件、文本、磁盘、进程、网络、权限、定时任务、日志。
- 这说明 Linux 命令行的核心价值在于组合，而不是记住某个单点“全能命令”。

### Inspection Before Mutation

- 删除乱码文件前先查 inode，做 `dd` 压测前先 `df -h` 确认磁盘，处理僵尸进程时先通过父进程定位问题。
- 这种顺序体现了运维文档中最重要的安全原则之一：先确认对象，再执行修改。

### Distinguish General Linux Commands from Subsystem-Specific Interfaces

- 来源同时出现 `netstat` / `ss`、`route -n` / `ip route`、`ifconfig` / `ip addr`、`traceroute` / `mtr` 这类“传统命令 / 更现代替代”并存的情况。
- `journalctl` 依赖 `systemd`，`firewall-cmd` 依赖 `firewalld`，它们不应被泛写成所有 Linux 环境都完全一致的接口。

### Text Processing Is the Universal Glue

- `grep`、`sed`、`awk` 以及 `sort`、`uniq`、`cut`、`paste` 等工具出现在来源中心位置，说明文本流处理是命令行操作的共同语言。
- 这与 [[bash]] 和 [[shell-scripting]] 的角色天然互补：shell 负责编排，这些工具负责真正的数据筛选和改写。

### Metadata Awareness Matters

- `cp -p`、`rsync -a`、`chmod`、`chown`、SUID/SGID/Sticky bit 说明“文件内容”之外，权限、所有权和时间戳也是操作对象。
- inode 删除示例也表明，命令行运维经常需要直面文件系统元数据，而不是只看文件名。

### Some Commands Need Explicit Risk Framing

- `dd`、`kill -9`、`userdel -r`、`crontab -r`、防火墙端口变更都带明显副作用。
- 这类命令在正式文档中应补上前置检查、权限要求、影响范围和回滚建议，而不能只给语法。

## Documentation Implications

- 文档应按“要回答什么操作问题”来组织命令，而不只是按命令字母排序。
- 对 `journalctl`、`firewall-cmd`、`sed -i`、`rsync` 一类命令，应标出系统前提、权限要求和平台差异，避免把 GNU/Linux 行为误写成通用 Unix 事实。
- 对带副作用的例子，应明确写出验证步骤，例如改防火墙后用 `ss` / `nc` 验证，改定时任务后先 `crontab -l` 回看。
- 如果把来源改写成教程，适合按“文件与文本 -> 资源巡检 -> 进程与网络 -> 权限与系统策略 -> 定时任务与日志”递进。

## Known Gaps from This Source

- 没有覆盖 `systemctl`、包管理器、SSH、挂载/文件系统修复或容器相关操作。
- 没有讨论 `sudo`、权限提升流程、最小权限原则或命令回滚策略。
- 没有说明发行版差异、BSD/macOS 差异，或 GNU 工具和 BusyBox 环境的兼容边界。
- 没有把这些命令进一步封装为脚本、别名或巡检流程模板。

## Related Pages

- [[linux]]
- [[linux-command-line-operations]]
- [[bash]]
- [[shell-scripting]]
- [[glossary]]
- [[overview]]
