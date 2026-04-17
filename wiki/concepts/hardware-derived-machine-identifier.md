---
title: Hardware-derived machine identifier
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: [Windows开发相关.md]
tags: [concept, windows, machine-identifier, hardware-fingerprint, wmi, licensing, native-development]
---

Hardware-derived machine identifier refers to the practice of collecting multiple device properties and hashing them into a derived identifier, called `机器码` in the current Windows source.

## Definition

在当前知识库里，`机器码` 指的不是编译后的机器指令，而是一种基于硬件信息拼接后生成的设备标识。当前来源通过 BIOS、CPU、主板和硬盘等字段组合出一个较稳定的本机身份，再对拼接结果做哈希。

这意味着它本质上是一个多阶段流程：

1. 选择输入字段
2. 从系统接口读取字段
3. 把字段转成可拼接的字符串
4. 约定拼接顺序和分隔符
5. 对拼接结果做哈希，得到业务侧使用的标识

## Pipeline in the Current Source

| Stage | Source behavior | Why it matters |
|---|---|---|
| Field selection | 选择 UUID、ProcessorId、主板序列号/型号/厂商、BIOS 序列号、硬盘序列号 | 决定标识的信息来源和稳定性边界 |
| Data collection | 通过 `WMI` 直接查询或通过 `wmic` 命令查询 | 决定依赖的系统接口和失败点 |
| Normalization | 把 `VARIANT` 或命令行文本转成字符串 | 决定不同字段如何进入统一处理链 |
| Concatenation | MFC 示例用换行，Win32 示例用 `@` 分隔 | 决定哈希前的原始输入是否可复现 |
| Hashing | 示例提到“计算哈希”或 `MD5` | 把多字段原文转成更便于存储和比较的值 |

## Source-specific Inputs

| Input | Origin |
|---|---|
| BIOS UUID | `Win32_ComputerSystemProduct.UUID` |
| Processor ID | `Win32_Processor.ProcessorId` |
| Baseboard serial number | `Win32_BaseBoard.SerialNumber` |
| Baseboard product | `Win32_BaseBoard.Product` |
| Baseboard manufacturer | `Win32_BaseBoard.Manufacturer` |
| BIOS serial number | `Win32_BIOS.SerialNumber` |
| Disk serial number | `Win32_DiskDrive.SerialNumber` |

## Why It Matters

- Many Windows-native products need a locally derivable device identifier for licensing, activation, or host-level differentiation.
- The current source makes clear that this identifier is assembled, not discovered whole.
- For technical writing, this concept helps separate the reusable pattern from any one implementation detail such as MFC, Win32, or a specific hash function.

## Common Misconceptions

| Misconception | More accurate understanding |
|---|---|
| `机器码` 就是 machine code | 在当前语境里它是设备标识，更接近 hardware-derived machine identifier |
| 只查一个字段就足够 | 当前来源选择多字段拼接，说明作者在追求更高区分度 |
| 哈希步骤会自动解决所有稳定性问题 | 哈希只改变表示形式，不会修复输入字段缺失或硬件变更带来的漂移 |
| 获取硬件标识是一个单 API 调用 | 当前来源显示它实际是“采集 + 转换 + 拼接 + 哈希”的流水线 |

## Documentation Implications

- 文档应把“输入字段清单”和“哈希规则”写清楚，否则读者无法复现实验结果。
- 应明确说明 `机器码` 是业务术语，而不是 CPU 指令层面的 machine code。
- 应补充输入字段缺失、多磁盘、虚拟机克隆和硬件更换后的处理策略，而不要只给快乐路径代码。
- 若不同实现使用不同分隔符、编码或哈希算法，应写明这些差异会不会影响兼容性。

## Related Pages

- [[windows-development-related]]
- [[windows-management-instrumentation]]
- [[windows]]
- [[glossary]]
- [[overview]]
