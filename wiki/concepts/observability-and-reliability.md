---
title: 可观测性与稳定性
type: concept
created: 2026-04-14
updated: 2026-04-17
sources: [2025年技术线总结.md, 快速基于 AI 入门全栈研发.md, Linux基础与测试专题.md, 分布式核心原理.md]
tags: [concept, observability, reliability, operations, devops, monitoring, linux, metrics, tracing, distributed-systems]
---

可观测性与稳定性是当前知识库里的工程基本盘之一，目标是让系统运行从“不稳定”走向“可预测”，并把运行期判断建立在可解释的主机信号、指标、日志和链路事实之上。

## Definition

这里的可观测性由日志、指标和 Trace 组成，稳定性则覆盖事故管理、容灾、降级和核心服务可用性等能力。新来源进一步把这些能力落到了 Linux 主机的 CPU、内存、磁盘、网络与 `/proc` / `/sys` 等可检查事实之上。

## Scope Across Sources

- 建设日志、指标、Trace 一体化的可观测性能力。
- 优化事前、事中、事后的事故管理流程。
- 补齐容灾与降级方案。
- 通过平台能力和标准流程降低线上不确定性。
- 在新的全栈入门来源里，可观测性还被画进部署后的反馈回路，和开发、测试、部署共同构成交付闭环。
- 在新的 Linux 基础来源里，可观测性被进一步拆成四大资源、USE 方法以及 `/proc` -> `node_exporter` -> `Prometheus` -> `Grafana` 的观测链路。
- 在新的分布式系统来源里，稳定性被进一步落实为超时、重试、幂等、限流、熔断、降级和故障转移这些面向异常路径的工程机制。

## Operational Stack

| 层 | 角色 | 当前来源里的关键对象 |
|---|---|---|
| 宿主机事实层 | 暴露 CPU、内存、磁盘、网络与进程状态 | `/proc`、`/sys`、`top`、`free -h`、`iostat`、`ss` |
| 指标采集层 | 把宿主机事实转成标准指标 | `node_exporter` |
| 存储与告警层 | 持续抓取、存储、计算规则、触发告警 | `Prometheus` |
| 可视化层 | 把趋势和状态转换成仪表盘 | `Grafana` |
| 旁路证据层 | 告诉你“是什么问题”和“哪里问题” | Logs、Traces |

## USE as First-Triage Model

新来源引入 USE 方法，要求团队先对 CPU、内存、磁盘 I/O 和网络 I/O 依次检查：

- Utilization：资源用了多少。
- Saturation：是否存在等待队列或压力堆积。
- Errors：是否已经出现显式错误或失败。

这让“系统慢了”不再只是经验判断，而变成一个可复用的排障顺序。

## Example Signals

- 核心可用性 >= 99.9%
- P0/P1 事故减少 40%
- `node_load1`、`MemAvailable`、磁盘 I/O 时间、网络收发量等宿主机级指标成为运行判断的基础信号。

## Why It Matters

- 即使软件形态和开发方式发生变化，可靠性仍被视为核心约束。
- AI辅助开发可以提升效率，但线上稳定性仍然需要制度、工具和流程共同保证。
- 当团队采用小团队模式时，更需要强可观测性来降低对个体经验的依赖。
- 它还能帮助新人理解“监控平台不是魔法”，而是对 Linux 运行时事实的持续组织和呈现。

## Documentation Implications

- 需要统一定义日志、指标、Trace、事故等级、复盘模板和容灾策略。
- 需要让稳定性文档与平台底座文档互相链接，而不是分散存放。
- 适合沉淀 runbook、故障处置流程和 SLI/SLO 说明。
- 入门型交付文档也应把监控、告警和运行反馈放进主流程，而不是只在上线后附带一句“记得看日志”。
- 当文档列出监控指标时，最好同步给出命令行交叉验证入口和其对应的 Linux 事实来源。
- 对性能或容量问题，文档适合先按 USE 方法组织，再继续进入服务级深挖。

## Related Pages

- [[linux-foundations-and-testing-special-topic]]
- [[2025-technical-line-summary]]
- [[ai-full-stack-development-onboarding]]
- [[technical-line-leader]]
- [[platform-foundation]]
- [[full-lifecycle-delivery-capability]]
- [[devops-delivery-pipeline]]
- [[use-methodology]]
- [[linux]]
- [[linux-command-line-operations]]
- [[distributed-systems-foundations]]
- [[distributed-systems-resilience-patterns]]
