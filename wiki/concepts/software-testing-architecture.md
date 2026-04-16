---
title: 软件测试架构
type: concept
created: 2026-04-16
updated: 2026-04-16
sources: [Linux基础与测试专题.md]
tags: [concept, testing, quality, ci-cd, regression, unit-testing, integration-testing, e2e]
---

软件测试架构是在不同测试层级、测试数据类型、执行时机和环境真实度之间分配职责的质量体系，其目标不是堆积测试数量，而是用合适的反馈成本覆盖合适的风险类型。

## Definition

当前来源把这套架构组织成测试金字塔：

| 层级 | 比例 | 主要验证对象 | 设计原则 |
|---|---|---|---|
| 单元测试 + 属性测试 | 70%~80% | 业务逻辑、算法、数据转换、不变量 | 最多、最快、最贴近逻辑核心 |
| API / 集成测试 | 15%~25% | 模块协作、数据库/缓存、外部依赖、契约 | 适中、关注连接正确性 |
| E2E / 场景测试 | <=5% | 用户关键路径 | 最少、只测高价值路径 |
| 回归体系 | 贯穿各层 | 历史缺陷与已知风险 | 一旦加入就长期保留 |

## Test Data System

来源把测试数据也拆成独立体系，而不是把“数据准备”看成测试实现细节：

| 数据类型 | 作用 | 典型特征 |
|---|---|---|
| 固定用例（Golden Cases） | 作为 CI 中稳定、确定性的基线 | 可重复、可对比 |
| 随机数据（Property-based） | 自动探索输入空间和边界 | 覆盖未知边界条件 |
| 缺陷数据（Regression Set） | 固化历史 bug | 永不回归、长期累积 |

## Time-Based Execution Strategy

来源进一步把测试放到不同时间尺度上执行：

| 时机 | 推荐内容 | 目标 |
|---|---|---|
| 每次提交 | 单元测试、属性测试、轻量集成测试 | 几分钟内给出快速反馈 |
| Nightly | 全量集成测试、回归测试、大规模数据测试 | 扩大覆盖但不拖慢提交反馈 |
| 发布前 | E2E、性能测试、安全测试 | 对真实发布风险做最后验证 |

## Environment Model

| 环境 | 特点 | 更适合的测试 |
|---|---|---|
| Dev | 本地开发、依赖可 mock | 单元测试、属性测试 |
| Test / Staging | 接近生产、依赖更完整 | 集成测试、E2E |
| Production | 真实流量与真实状态 | 监控、告警、运行验证 |

这意味着“越接近生产越真实，但也越慢、越贵”，所以更需要控制数量和目标。

## Quality Capability Model

来源把整套测试再抽象成四层能力：

| 能力层 | 保证什么 | 主要实现方式 |
|---|---|---|
| Correctness | 逻辑正确 | 单元测试 + 属性测试 |
| Integration | 系统能协作 | 集成测试 + Contract Test |
| Usability | 用户真的能用 | E2E 测试 |
| Quality Evolution | 质量不退化 | 回归测试 + 缺陷数据闭环 |

## Why It Matters

- 它把“写测试”转成“设计质量反馈系统”。
- 它帮助团队控制反馈速度，避免把大量验证都堆到最慢、最脆弱的 E2E 层。
- 它把线上 bug 明确转化为未来自动化防线，而不是只在事故复盘里停留一次。

## Documentation Implications

- 测试文档应明确每一层要防什么风险、使用什么依赖、允许多慢，而不是只按目录介绍测试文件。
- CI/CD 文档应显式区分提交时、Nightly 和发布前的测试责任分工。
- 测试数据文档应区分固定用例、随机生成和缺陷回归数据，避免把所有数据都混成一类夹具。
- 当团队引入属性测试、Contract Test 或回归数据集时，文档应同步解释它们和普通单元测试的边界。

## Common Misconceptions

| 误解 | 更准确的理解 |
|---|---|
| E2E 越多越好 | E2E 成本高、脆弱度高，应只覆盖关键路径 |
| 单元测试能替代集成测试 | 逻辑正确不等于依赖协作正确 |
| 集成测试和 E2E 是一回事 | 集成测试关注系统连接，E2E 关注用户路径 |
| 回归测试只是“再跑一遍” | 回归测试的关键是把历史 bug 沉淀为长期自动化用例 |

## Related Pages

- [[linux-foundations-and-testing-special-topic]]
- [[property-based-testing]]
- [[devops-delivery-pipeline]]
- [[full-lifecycle-delivery-capability]]
- [[observability-and-reliability]]
- [[ai-era-full-stack-beginner]]
- [[growing-engineer]]
- [[glossary]]
- [[overview]]
