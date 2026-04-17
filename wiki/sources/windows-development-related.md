---
title: Windows 开发相关
type: source
created: 2026-04-17
updated: 2026-04-17
sources: [Windows开发相关.md]
tags: [windows, native-development, inno-setup, installer, packaging, service-installation, wmi, machine-identifier, mfc, win32, com, source]
---

# Windows 开发相关

Compact Windows-native development note covering Inno Setup packaging, service bootstrap sequencing, and WMI-based hardware fingerprint generation for MFC and Win32 applications.

## Source Metadata

| Attribute | Value |
|---|---|
| Filename | `Windows开发相关.md` |
| Location | `raw/OS/Windows/` |
| Document nature | Snippet collection / engineering memo |
| Primary topics | Inno Setup installer scripting; WMI-based machine identifier generation |
| Target platform | Windows native desktop / service applications |
| Covered implementation styles | Inno Setup script, MFC C++, Win32 C++ |
| Hardware properties used | BIOS UUID, processor ID, motherboard serial/product/manufacturer, BIOS serial, disk serial |

## What the Source Covers

| Area | Concrete content | Observed intent |
|---|---|---|
| Install preflight hook | `InitializeSetup()` + `Exec(...)` + `ewWaitUntilTerminated` | Run a prerequisite or external process before continuing installation |
| Installer packaging | `#define` macros, `[Setup]`, `[Languages]`, `[Files]`, `[Icons]` | Package the main executable plus an auxiliary installer and standardize metadata |
| Post-install execution | `[Run]` entries for helper installer and `-service=install` / `-service=start` | Turn file deployment into a real service bootstrap workflow |
| Uninstall cleanup | `[UninstallRun]` entries for `-operation=uninstall`, `-service=stop`, `-service=uninstall` | Treat removal as an explicit lifecycle rather than passive file deletion |
| Hardware data model | WMI classes such as `Win32_ComputerSystemProduct`, `Win32_Processor`, `Win32_BaseBoard`, `Win32_BIOS`, `Win32_DiskDrive` | Build a multi-field hardware fingerprint instead of trusting a single property |
| MFC implementation path | COM initialization, `IWbemLocator`, `ConnectServer`, `ExecQuery`, `VariantToString` | Query WMI directly through COM APIs inside an MFC application |
| Win32 implementation path | `CreatePipe`, `CreateProcessW`, `ReadFile`, `wmic ... get ...` | Shell out to command-line tooling and parse stdout from a plain Win32 program |

## Core Mental Model Implied by the Source

1. 安装包不是“把文件复制过去”这么简单，而是一个可编排安装前、安装后和卸载阶段动作的生命周期控制器。
2. 当前语境里的“机器码”不是 CPU 指令意义上的 machine code，而是由多项硬件信息拼接并哈希得到的设备标识。
3. 同一个业务目标在 Windows 上可以通过不同控制面完成，例如直接走 COM/WMI，或者通过 Win32 进程 API 调起 `wmic` 命令。
4. 来源倾向于先记录最短可用路径，因此更像“项目内实战备忘”而不是完整的 Windows 平台开发指南。

## Notable Operational Patterns

### Pre-install Hooks Can Block on External Work

- `InitializeSetup()` 示例展示了 Inno Setup 可以在真正进入安装流程前执行外部程序，并等待其结束后再继续。
- 这意味着安装文档应明确区分“安装器启动了”和“安装前检查已完成”这两个阶段。

### Build-time Metadata and Artifact Paths Are Parameterized

- `#define` 宏和 `GetEnv('MOLAN_RELEASE_DIR')` 说明脚本既包含固定产品元数据，也依赖构建环境提供的输出目录。
- 这种写法适合被整理成“脚本模板 + CI/CD 变量”的结构，而不是把所有路径硬编码进正文。

### Service Bootstrap Is Encoded in Installer Phases

- `[Run]` 里先执行辅助安装器，再执行 `-service=install` 和 `-service=start`，说明安装成功的判定不只是文件是否落盘。
- `[UninstallRun]` 反过来补齐卸载动作，强调 Windows 服务型程序需要显式停止和反注册。

### WMI Access Is Explicitly Class-and-Property Driven

- 来源没有把“获取机器码”写成黑盒函数，而是明确列出了 `Win32_*` 类和目标属性。
- 这对技术写作很重要，因为硬件标识的稳定性和可获取性，往往取决于具体查询了哪些属性以及如何拼接。

## Caveats and Verification Risks

- `notepad.exe` 显然只是 `Exec` 的占位示例，不能直接理解为真实业务依赖。
- Inno Setup 示例包含特定产品名、AppId、环境变量名和可执行文件名，整理时应抽象出结构，避免把项目私有命名误写成通用模板。
- 来源没有覆盖代码签名、UAC、静默安装、升级覆盖策略、回滚策略、32/64 位差异或服务账号权限。
- 来源展示了机器码的采集与拼接，但没有讨论字段缺失、硬件更换、虚拟机环境或多磁盘环境下的稳定性策略。
- Win32 版本依赖 `wmic` 命令输出，来源没有讨论不同 Windows 环境中的可用性、编码或本地化输出差异。

## Documentation Implications

- Windows 安装文档应按“安装前检查 -> 文件落盘 -> 安装后动作 -> 卸载动作”分阶段书写，而不是只贴一个 `[Files]` 片段。
- 当文档使用“机器码”一词时，应明确写明它指设备标识 / hardware-derived identifier，而不是编译后的 machine code。
- 对 WMI 类文档，应保留精确类名和属性名，例如 `Win32_BaseBoard.SerialNumber`，不要泛写成“读取主板信息”。
- 若同一目标既有 COM 方案又有命令行方案，应分别写清依赖、失败点和适用上下文。
- 原生开发说明应与 [[windows-system-settings]] 这类 OS 管理说明分开组织，避免把安装器/WMI 路径和 `Windows Update` / 防火墙 / 计划任务操作混成同一层。

## Related Pages

- [[windows]]
- [[windows-system-settings]]
- [[inno-setup]]
- [[windows-management-instrumentation]]
- [[hardware-derived-machine-identifier]]
- [[glossary]]
- [[overview]]
