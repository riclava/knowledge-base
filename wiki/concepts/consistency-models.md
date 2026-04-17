---
title: 一致性模型
type: concept
created: 2026-04-17
updated: 2026-04-17
sources: [分布式核心原理.md]
tags: [concept, distributed-systems, consistency, linearizability, causal-consistency, eventual-consistency]
---

一致性模型描述的是分布式系统中“读操作能看到什么样的写结果”，本质上是在业务可接受的旧数据窗口、顺序要求和系统代价之间定义一条可承诺的边界。

## Definition

当前来源把一致性模型组织成由强到弱的一条谱系，用来回答一个关键问题：

> 业务到底能不能接受读到旧数据，或者接受不同节点在短时间内看到不同状态？

## Consistency Spectrum

| 模型 | 核心承诺 | 典型代价 | 更适合的场景 |
|---|---|---|---|
| 线性一致性（Linearizability） | 写成功后，所有后续读都像发生在同一全局时线上 | 延迟高、分区时更容易拒绝请求 | 余额、库存、锁、配置主状态 |
| 顺序一致性（Sequential Consistency） | 所有进程观察到同一个全局顺序，但不要求严格符合真实时间 | 语义弱于线性一致性，仍需要全局排序 | 某些共享内存或并发语义场景 |
| 因果一致性（Causal Consistency） | 有因果依赖的操作必须被按依赖顺序观察到 | 需要跟踪因果关系 | 评论、会话状态、社交互动 |
| 最终一致性（Eventual Consistency） | 没有新写入时，副本最终收敛到相同值 | 存在不确定长度的不一致窗口 | 点赞数、头像、缓存、DNS |
| 弱一致性（Weak Consistency） | 只提供非常有限或上下文相关的可见性保证 | 代价低，但业务语义需要自己兜底 | 非关键缓存或近实时统计 |

## Session-Level Guarantees

当前来源把若干常见弱化保证单独列出，这些保证常见于“最终一致但又不想完全反直觉”的系统：

| 保证 | 含义 | 解决的直觉问题 |
|---|---|---|
| Read Your Writes | 自己写完后，至少自己能读到新值 | “我刚改了头像却看不到” |
| Monotonic Reads | 后续读取不会比之前更旧 | “我刷新后不该倒退” |
| Monotonic Writes | 同一客户端发起的写按顺序生效 | “后发的更新不能被先发覆盖” |

## Choosing by Business Impact

| 业务对象 | 是否能接受旧数据 | 更合理的选择 |
|---|---|---|
| 银行余额 / 扣款 | 不能 | 线性一致性或等价强语义 |
| 库存扣减 | 通常不能 | 强一致或至少严格串行化写路径 |
| 评论列表 | 通常能接受短暂延迟，但要保留上下文顺序 | 因果一致性或最终一致 + 排序规则 |
| 点赞数 / 阅读数 | 可以 | 最终一致 |
| 用户头像 / 昵称 | 通常可以 | 最终一致 + session 保证 |

## Relationship to CAP and PACELC

- CAP 讨论的是网络分区时系统在一致性和可用性之间的选择。
- PACELC 讨论的是无分区时，低延迟和更强一致性之间仍需权衡。
- 一致性模型则把这种权衡进一步落到具体“读会看到什么”的语义承诺上。

## Why It Matters

- 它把“系统会不会返回旧数据”从模糊担心变成可被产品、后端和客户端共同理解的契约。
- 它避免团队把“高可用”误写成“所有地方都立刻一致”。
- 它帮助文档把用户体验问题翻译成后端承诺，例如是否允许短暂延迟、是否需要因果顺序、是否需要读己之写。

## Documentation Implications

- 不要只写“强一致 / 最终一致”，最好补一句业务语言的承诺和例外。
- 当系统是最终一致时，应明确不一致窗口、刷新行为和客户端兜底策略。
- 对评论、消息、会话等强依赖顺序的业务，应单独写明是否需要因果一致性。
- 不要把 ACID 事务里的“Consistency”和 CAP 里的“Consistency”混写为同一个概念。

## Common Misconceptions

| 误解 | 更准确的理解 |
|---|---|
| 最终一致性等于系统不正确 | 它仍然有收敛目标，只是不保证立刻一致 |
| 强一致性一定最好 | 它更贵、延迟更高、故障时更可能牺牲可用性 |
| 因果一致性只是“排序更好看” | 它是在保护依赖关系不被乱序打破 |
| 一致性模型只影响数据库实现 | 它也影响缓存、API、客户端刷新和产品体验承诺 |

## Related Pages

- [[distributed-systems-core-principles]]
- [[distributed-systems-foundations]]
- [[distributed-consensus]]
- [[data-replication-and-partitioning]]
- [[logical-time-and-causality]]
- [[distributed-systems-resilience-patterns]]
- [[glossary]]
- [[overview]]

