---
title: macOS 常用命令
type: source
created: 2026-04-17
updated: 2026-04-17
sources: [macOS常用命令.md, macOS系统设置.md]
tags: [source, macos, apple, command-line, operations, system-administration, cheat-sheet, networking, storage, processes, launchctl, power-management, developer-tooling, homebrew, xcode, defaults, system-settings]
---

# macOS 常用命令

这是一份面向 macOS 日常巡检、排障、开发环境准备与桌面偏好调整的常用命令速查，覆盖系统信息、磁盘、网络、进程、服务、电源、用户、Homebrew、Xcode 工具和桌面自动化。

## Source Metadata

| Field | Value |
|---|---|
| Original file | `raw/OS/MacOS/macOS常用命令.md` |
| Language | Chinese |
| Scope | macOS command-line operations, developer tooling, and desktop utility commands |
| Primary surfaces | system info, storage, networking, processes, `launchctl`, `pmset`, user management, Homebrew, Xcode CLI tools, `defaults`, clipboard/screenshot/open commands |
| Document style | operator-oriented cheat sheet |
| Typical reader | macOS user or developer doing local inspection, troubleshooting, setup, or quick automation |

## What the Source Covers

| 模块 | 覆盖内容 | 价值 |
|---|---|---|
| 系统信息 | `sw_vers`、`system_profiler`、`sysctl`、`pmset -g batt`、`uptime` | 快速确认 macOS 版本、硬件规格、电池状态和运行时长 |
| 磁盘与存储 | `df`、`du`、`diskutil`、`rm -rf ~/.Trash/*` | 查看空间占用、挂载介质和磁盘信息，并执行本地清理 |
| 网络诊断 | `lsof`、`netstat`、`route`、`ifconfig`、`airport`、`scutil --dns`、`ping`、`traceroute` | 定位端口监听、路由、Wi-Fi、DNS 和连通性问题 |
| 防火墙与服务 | `socketfilterfw`、`launchctl` | 查看或修改系统防火墙状态，管理 `launchd` 下的服务 |
| 进程管理 | `ps`、`top`、`pstree`、`kill`、`killall`、`pkill`、`nohup`、`jobs`、`fg` | 观察运行态并处理前后台任务和异常进程 |
| 电源与用户 | `pmset`、`caffeinate`、`shutdown`、`dscl`、`groups`、`su -` | 管理休眠、关机、用户信息和身份切换 |
| 开发工具 | `brew`、`xcode-select`、`xcodebuild -license accept`、环境变量操作 | 搭建开发工具链并维护命令行开发环境 |
| 桌面实用命令 | `pbcopy`、`pbpaste`、`screencapture`、`say`、`open`、`defaults write` | 把 GUI 相关操作纳入脚本和终端工作流 |

## Core Mental Model Implied by the Source

这份来源虽然以命令清单呈现，但隐含了一个很明确的 macOS 操作模型：

1. 先检查系统或对象状态，例如版本、硬件、磁盘、端口、网络接口、服务列表、电源设置或当前用户。
2. 再定位要变更的控制面，例如 `diskutil`、`launchctl`、`pmset`、`brew` 或 `defaults`。
3. 执行带副作用的命令，例如杀进程、改路由、开关防火墙、调整 Finder/Dock 偏好或安装开发工具。
4. 最后重载受影响的进程或再次验证，例如 `killall Finder`、`killall Dock`、重新查看 `launchctl list`、`pmset -g` 或网络状态。

## Notable Operational Patterns

### Apple-Specific Control Surfaces Sit Beside BSD/Unix Tools

- 来源把 `df`、`du`、`ps`、`find`、`netstat` 这类 Unix/BSD 工具和 `diskutil`、`launchctl`、`pmset`、`scutil`、`defaults` 等 Apple 专用接口放在同一张速查表里。
- 这说明 macOS 文档不能只写“Mac 也能用 Unix 命令”，还要明确哪些操作只能通过 Apple 自己的控制面完成。

### Desktop Behavior Is Scriptable

- `pbcopy` / `pbpaste`、`screencapture`、`say`、`open` 和 `defaults write` 表明，macOS 的命令行不仅用于系统巡检，还能直接驱动桌面交互和 UI 偏好。
- 和很多 Linux 服务器文档相比，这类来源更强调“命令行如何与本地桌面体验联动”，而不只是后台服务管理。

### Developer Environment Setup Is Part of Platform Operations

- `brew`、`xcode-select`、`xcodebuild -license accept` 和 `PATH` 配置说明，这份来源把开发工具链安装视为 macOS 运维表面的一部分。
- 这也让 [[macos]] 不只是消费型桌面系统，而是开发者工作站平台。

### Risk Framing Is Useful but Uneven

- 来源中既有只读巡检命令，也有明显高风险动作，例如 `rm -rf ~/.Trash/*`、`kill -9`、`shutdown`、`defaults write` 和防火墙开关。
- 其中“安全清空回收站”这一表述并不准确，因为 `rm -rf ~/.Trash/*` 本质上是不可恢复的强制删除，绕过了 Finder 的保护性操作路径。

### Private or Version-Sensitive Interfaces Need Extra Context

- `airport` 命令来自私有框架路径，`launchctl` 示例也没有解释 `LaunchAgents` / `LaunchDaemons`、用户域 / 系统域或现代 `bootstrap` / `bootout` 语义。
- 这些命令在正式文档里需要比速查表更多的版本边界和上下文说明。

## Documentation Implications

- 文档应明确区分“只读检查命令”和“会立即修改系统状态的命令”。
- 对 `defaults write ...; killall Finder` 这类示例，应把“写偏好”与“重启受影响进程使其生效”拆开解释。
- 对 `launchctl`、`airport`、`socketfilterfw` 这类 Apple 专用或私有接口，应写明适用范围，不要泛化成通用 Unix 事实。
- 对 `brew` 和 Xcode 工具链命令，应补充安装前提、权限要求和失败后的验证方式。
- 对环境变量持久化示例，应写明当前 shell 语境，因为来源把 `~/.zshrc` 和 `~/.bash_profile` 并列提及，但没有区分默认场景。
- 对输入法、触控板和浏览器内开关这类 GUI / app 层设置，应直接链接到 [[macos-system-settings]]，而不是强行塞进命令速查表。

## Known Gaps from This Source

- 没有区分 Apple silicon、Intel、T2 等不同硬件路径上的命令差异。
- 没有解释 `launchd` 的 domain model、plist 结构和更现代的 `launchctl` 子命令语义。
- 没有系统介绍 Homebrew 的 formula / cask / tap、版本固定或镜像策略。
- 没有覆盖 APFS、Disk Utility 图形化流程、SIP、TCC、Full Disk Access 或代码签名相关约束。
- 没有为高风险命令提供回滚、确认或最小权限建议。

## Related Pages

- [[macos]]
- [[macos-command-line-operations]]
- [[macos-system-settings]]
- [[macos-usb-installer-creation]]
- [[glossary]]
- [[shell-scripting]]
- [[overview]]
