---
title: macOS command-line operations
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: [macOS常用命令.md, macOS系统设置.md]
tags: [concept, macos, apple, command-line, operations, system-administration, unix, bsd, launchctl, pmset, diskutil, networking, developer-tooling, homebrew, xcode, defaults, system-settings]
---

macOS command-line operations 是通过 BSD/Unix 通用工具与 Apple 专用控制接口组合来检查、修改和自动化 macOS 系统状态的实践，覆盖系统信息、磁盘、网络、进程、服务、电源、用户、开发工具和一部分可脚本化的桌面偏好设置，但并不取代 GUI `系统设置` 或应用自身的偏好面板。

## Definition

在当前知识库里，macOS command-line operations 不是“把 Linux 命令搬到 Mac 上”的简单子集，而是一套混合操作表面：

- 一部分来自 Unix/BSD 风格命令，如 `df`、`du`、`ps`、`find`、`netstat`、`ifconfig`
- 一部分来自 Apple 提供的系统控制接口，如 `system_profiler`、`diskutil`、`scutil`、`launchctl`、`pmset`、`defaults`
- 还有一部分直接连接本地桌面体验，如 `pbcopy`、`screencapture`、`say` 和 `open`

这让 macOS 的命令行既能做传统系统巡检，也能管理开发者工作站和桌面交互细节。

## Operational Surfaces in This Source

| 操作面 | 常见命令 | 典型要回答的问题 |
|---|---|---|
| 系统与硬件信息 | `sw_vers`、`system_profiler`、`sysctl`、`uptime`、`pmset -g batt` | 我当前跑的是什么版本、什么硬件、电池和运行时长如何 |
| 磁盘与卷管理 | `df -h`、`du -sh`、`diskutil list`、`diskutil info`、`diskutil eject` | 空间在哪儿、卷怎么挂载、外接磁盘是什么、能否安全弹出 |
| 网络与 DNS 诊断 | `lsof -i`、`netstat -an/-nr/-s`、`route`、`ifconfig`、`airport -I/-s`、`scutil --dns`、`ping`、`traceroute` | 端口谁在占用、路由是否正确、Wi-Fi 状态如何、DNS 配置是否异常 |
| 防火墙与服务管理 | `socketfilterfw`、`launchctl list/load/unload/start/stop` | 防火墙是否开启、服务是否已注册、是否启动或停止 |
| 进程与任务控制 | `ps aux`、`pstree`、`top`、`kill`、`killall`、`pkill`、`nohup`、`jobs`、`fg` | 哪个进程在跑、谁占资源、如何终止或切换前后台任务 |
| 电源与会话管理 | `pmset -g`、`caffeinate`、`shutdown` | 机器为何睡眠、如何临时阻止睡眠、如何重启或定时关机 |
| 用户与身份信息 | `whoami`、`dscl`、`groups`、`su -` | 当前是谁、系统有哪些用户、用户属性和组关系是什么 |
| 开发工具链 | `brew`、`xcode-select`、`xcodebuild -license accept`、`env`、`export`、`source ~/.zshrc` | 软件怎么装、命令行开发工具是否就绪、环境变量是否生效 |
| 桌面与偏好自动化 | `pbcopy`、`pbpaste`、`screencapture`、`say`、`open`、`defaults write`、`killall Finder/Dock/SystemUIServer` | 如何脚本化剪贴板、截图、朗读、打开资源和 Finder/Dock 偏好 |

## Distinctive Characteristics

### Apple-Specific Interfaces Are First-Class

- 和 [[linux-command-line-operations]] 相比，macOS 更明显地把系统管理能力分散在 Apple 自己的命令接口中。
- `diskutil` 管卷，`launchctl` 管 `launchd`，`pmset` 管电源，`defaults` 管偏好，`scutil` 管系统配置状态。
- 这意味着文档必须写出精确接口名，而不能只说“用终端改一下系统设置”。

### GUI State and CLI State Interlock

- `open`、`pbcopy`、`screencapture`、`say` 和 `defaults` 说明，命令行可以直接触达桌面用户体验。
- 很多变更不是写完命令就结束，而是还需要重启 Finder、Dock 或 `SystemUIServer` 让界面层收敛到新状态。

### Developer Workstation Concerns Are Native to the Platform

- `brew` 和 Xcode Command Line Tools 让“开发机初始化”成为 macOS 操作表面的一部分，而不是独立于系统管理之外的主题。
- 这类内容往往和 shell 配置、PATH、证书或许可确认一起出现，适合作为工作站文档的一部分组织。

### CLI Surface Has a Real GUI Boundary

- 新增的 [[macos-system-settings]] 来源表明，很多高频工作站设置仍落在图形化 `系统设置` 或应用内偏好里，例如输入法、触控板和 Chrome 的网页预加载。
- 这意味着 `defaults` 能覆盖的只是其中一部分偏好，而不是所有“系统设置”都能优雅地下沉到终端命令。
- 对知识库写作来说，最重要的是把控制面分清楚：命令行能做什么、GUI 必须做什么、应用级设置又属于哪一层。

### Portable Unix Knowledge Helps, but Is Not Sufficient

- `ps`、`find`、`tar`、`df` 这类命令与 Unix 经验相通，但网络、服务、电源和偏好配置往往需要 Apple 专用接口补全。
- 因此“会 Linux 命令”能迁移一部分能力，但不等于已经掌握 macOS 运维。

## Implied Operating Loop

1. 先查看版本、硬件、磁盘、端口、服务、电池或当前偏好，确认问题落在哪个控制面。
2. 再用 Apple 或 BSD 命令缩小目标对象，例如具体磁盘、端口占用进程、LaunchAgent、DNS 配置或偏好 domain。
3. 对目标执行变更，例如 `diskutil eject`、`launchctl start`、`brew install`、`defaults write` 或 `shutdown`。
4. 最后重新查看状态，必要时重启受影响进程或重新打开 GUI 组件验证结果。

## Why It Matters

- 它把 macOS 从“主要靠 GUI 点击”的印象转化为一套可脚本化、可验证、可复查的系统操作表面。
- 对技术写作来说，它帮助把零散的“Mac 小技巧”组织成系统信息、磁盘、网络、服务、电源、开发工具和桌面自动化几类稳定主题。
- 它也让 [[macos]] 与 [[shell-scripting]] 之间形成更直接的联系，因为很多本地自动化最后都落到这些命令上。

## Common Misconceptions

| 误解 | 更准确的理解 |
|---|---|
| macOS 命令行基本等于 Linux 命令行 | 它共享一部分 Unix/BSD 工具，但系统配置常依赖 `diskutil`、`launchctl`、`pmset`、`defaults` 等 Apple 专用接口 |
| `defaults write` 就等于改了所有系统设置 | 它主要修改 user defaults / app preferences，很多变更还需要重启目标进程才能体现 |
| 只要会终端命令就能覆盖所有 Mac 设置问题 | 输入法、触控板和部分浏览器行为仍可能必须通过 GUI `系统设置` 或应用偏好解决 |
| `brew` 是 Apple 官方包管理器 | `Homebrew` 是社区维护的 macOS 包管理工具，不代表 Apple 官方支持边界 |
| `kill -9` 是标准结束进程方式 | 它是强制手段，适合兜底，不应代替优先尝试的正常退出路径 |
| `launchctl` 只要会 `load` / `unload` 就够了 | 真正稳定的文档还需要解释服务域、plist 放置位置和不同 macOS 版本的语义差异 |

## Documentation Implications

- 当内容依赖 Apple 专用控制面时，应保留精确命令名，如 `launchctl`、`pmset`、`diskutil`、`defaults`。
- 当命令作用于桌面偏好或 GUI 状态时，应补上“需要重启哪个进程才能生效”的说明。
- 当来源实际上是在讲 `系统设置`、输入法、触控板或浏览器偏好时，不要勉强改写成纯命令行教程，应直接标注 GUI / app 控制面。
- 当来源给出 `airport` 这类私有路径命令时，应说明这是 Apple 私有接口而不是稳定的跨版本公共 API。
- 当命令带明显副作用时，应补上风险提示，例如 `rm -rf ~/.Trash/*`、`shutdown`、`kill -9`、防火墙切换和路由修改。
- 当开发工具链命令出现时，应把它们和 shell 配置、PATH、生效验证一起写，而不是只列安装语法。

## Related Pages

- [[macos]]
- [[macos-common-commands-reference]]
- [[macos-system-settings]]
- [[macos-usb-installer-creation]]
- [[linux-command-line-operations]]
- [[shell-scripting]]
- [[bootable-os-installer-media]]
- [[startup-security-and-external-boot-policy]]
- [[glossary]]
- [[overview]]
