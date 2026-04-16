---
title: USE 方法
type: concept
created: 2026-04-16
updated: 2026-04-16
sources: [Linux基础与测试专题.md]
tags: [concept, observability, linux, monitoring, performance, troubleshooting, metrics]
---

USE 方法是一种按资源类型排查系统性能与稳定性问题的框架，要求对每种资源都检查 Utilization、Saturation、Errors 三个维度。

## Definition

在当前来源中，USE 方法主要用于 CPU、内存、磁盘 I/O 和网络 I/O 四大资源的第一轮诊断。

| 资源 | Utilization | Saturation | Errors |
|---|---|---|---|
| CPU | CPU 使用率、`idle` 占比 | 运行队列、load average | 通常不作为主指标 |
| 内存 | 已用/可用内存 | Swap 使用、OOM 压力 | 分配失败 |
| 磁盘 I/O | `%util`、I/O 时间 | 等待队列、`iowait` | 读写错误 |
| 网络 I/O | 收发带宽使用率 | 丢包、重传、连接堆积 | 网卡/链路错误 |

## Why It Matters

- 它能把“系统变慢了”从模糊感受，先收敛成“到底是哪类资源出了问题”。
- 它天然适合作为命令行巡检和监控大盘之间的共同语言。
- 它避免团队只盯某一个工具输出，而忽略资源之间的真正瓶颈位置。

## From Kernel Signals to Monitoring Stack

来源给出了一条很实用的观察链：

1. Linux 通过 `/proc` 和 `/sys` 暴露内核与进程状态。
2. `node_exporter` 读取这些状态并转成标准指标。
3. `Prometheus` 负责采集、存储和告警规则。
4. `Grafana` 负责把这些指标可视化。

这让命令行和监控平台之间形成了清晰映射：

| 命令行入口 | 典型文件来源 | 监控侧常见信号 |
|---|---|---|
| `top`、`uptime`、`mpstat` | `/proc/stat` | CPU 利用率、load |
| `free -h`、`/proc/meminfo` | `/proc/meminfo` | `MemAvailable`、Swap 使用 |
| `iostat`、`iotop` | `/proc/diskstats` | 磁盘繁忙度、I/O 时间 |
| `ss -s`、`sar -n DEV` | `/proc/net/*` | 流量、连接状态、错误率 |

## Documentation Implications

- 监控文档适合把每个指标明确映射到资源类别和观测目的，而不是只罗列指标名。
- 排障 runbook 应优先给出 USE 的排查顺序，再给出更细分的服务级检查。
- 告警阈值应解释其含义和上下文，例如“接近 CPU 核数的 load”或“Swap 持续增长意味着内存压力正在外溢”。
- 当文档引入 Prometheus 或 Grafana 时，最好同步写出与命令行的对应验证方式，降低“只会看图不会交叉核对”的风险。

## Common Misconceptions

| 误解 | 更准确的理解 |
|---|---|
| 只看 utilization 就够了 | 饱和度和错误同样会决定系统是否真正卡住 |
| load average 就是 CPU 使用率 | load 更接近等待运行资源的队列压力，而不只是 CPU 百分比 |
| 监控系统自己会解释根因 | 监控只能提供信号，排障仍需结合资源模型和上下文 |
| 命令行巡检和监控大盘是两套互不相干的体系 | 它们本质上是在看同一批底层系统信号的不同表述 |

## Related Pages

- [[linux-foundations-and-testing-special-topic]]
- [[observability-and-reliability]]
- [[linux]]
- [[linux-command-line-operations]]
- [[software-testing-architecture]]
- [[devops-delivery-pipeline]]
- [[glossary]]
- [[overview]]
