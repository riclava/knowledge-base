---
title: 分布式共识
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: [分布式核心原理.md]
tags: [concept, distributed-systems, consensus, paxos, raft, quorum, leader-election]
---

分布式共识是在多个节点可能崩溃、消息可能延迟或丢失的前提下，让一组节点对某个值或日志顺序达成一致的机制；它是配置服务、元数据服务、分布式锁和复制日志系统的核心基础设施。

## Definition

当前来源把共识问题拆成三个基础性质：

| 性质 | 含义 |
|---|---|
| 终止性（Termination） | 正常节点最终应该做出决定 |
| 一致性（Agreement） | 正常节点不能做出互相冲突的决定 |
| 有效性（Validity） | 被决定的值必须来自某个提议 |

FLP 的重要性在于：这些目标在纯异步系统里无法同时得到“必然终止”的确定性保证，因此工程实现总要引入额外假设。

## Paxos and Raft

| 方面 | Paxos | Raft |
|---|---|---|
| 历史定位 | 第一个被严格证明正确的共识算法 | 面向可理解性重构的工程化共识算法 |
| 核心结构 | Proposer / Acceptor / Learner | Follower / Candidate / Leader |
| 工程心智模型 | 更抽象，学习门槛高 | 强 Leader，流程更直观 |
| 常见用途 | 理论基础、很多后续算法的源头 | etcd、Consul、TiKV 等工程实现 |

## Leader-Based Consensus Path

来源把 Raft 作为工程首选，原因在于它把复杂问题拆开了：

1. Leader 选举
2. 日志复制
3. 安全性保证

典型流程是：

`客户端请求 -> Leader 追加日志 -> 复制给多数派 -> 提交 -> 应用到状态机 -> 返回结果`

这意味着：

- 共识系统通常不是“所有节点都能独立写”，而是先收敛到一个写入口。
- “已复制”和“已应用”是两个不同阶段。
- 多数派是安全性的关键，不等于所有副本都同步完成。

## Where Consensus Shows Up

| 场景 | 需要共识的原因 |
|---|---|
| 配置中心 / 元数据服务 | 所有客户端必须看到同一份权威状态 |
| 主节点选举 | 不能同时出现两个有效主节点 |
| 分布式锁 | 锁授予必须全局唯一且可线性化 |
| 复制状态机 | 多个节点需要按相同日志顺序执行命令 |

## Operational Notes

- 超时是共识协议的工程入口之一，因为节点必须在“继续等”和“发起选举”之间做决定。
- Quorum 是安全边界，多数派交集保证新 Leader 不会丢失已提交日志。
- 强 Leader 虽然更易理解，但也意味着 Leader 压力、心跳稳定性和故障切换时间都需要专门设计。
- 对分布式锁等场景，仅靠“锁过期”不够，来源引入的 fencing token 思路能防止旧持锁者恢复后继续操作资源。

## Why It Matters

- 它把“多副本”从简单备份升级成带顺序语义的复制状态机。
- 它决定了哪些系统可以作为“权威协调面”，哪些只能作为最终一致的数据面。
- 它也是理解 etcd、Kafka controller、数据库主选举等系统行为的共同底层语言。

## Documentation Implications

- 文档应明确写出选主、日志提交和故障恢复的判断条件，而不是只画一个“主从架构图”。
- 当系统依赖多数派确认时，应说明副本总数、写确认数和故障容忍度的关系。
- 对外部依赖共识服务的业务文档，应明确哪些能力来自共识保证，哪些仍需业务层幂等或补偿。

## Common Misconceptions

| 误解 | 更准确的理解 |
|---|---|
| 共识就是“大家互相通知一下” | 共识的难点是故障、乱序、超时和部分失联下仍要保证单一决定 |
| 多数派确认等于所有副本都一样新 | 少数副本可能落后，恢复时还要靠日志追赶 |
| Raft 只是 Paxos 的“简单版” | 它更像围绕可理解性重组问题的工程化协议家族 |
| 用了共识算法就不需要业务幂等 | 共识保护的是日志顺序，不直接消除客户端重试带来的重复副作用 |

## Related Pages

- [[distributed-systems-core-principles]]
- [[distributed-systems-foundations]]
- [[consistency-models]]
- [[data-replication-and-partitioning]]
- [[logical-time-and-causality]]
- [[distributed-systems-resilience-patterns]]
- [[state-and-data-flow-modeling]]
- [[overview]]

