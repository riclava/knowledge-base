---
title: 属性测试
type: concept
created: 2026-04-16
updated: 2026-04-16
sources: [Linux基础与测试专题.md]
tags: [concept, testing, property-testing, quality, unit-testing, regression, invariants]
---

属性测试是一种不只验证少量手写样例，而是通过自动生成输入来检查某些通用性质是否始终成立的测试方法。

## Definition

当前来源把它和单元测试区分得很清楚：

| 维度 | 单元测试 | 属性测试 |
|---|---|---|
| 核心问题 | 这个具体例子对不对 | 这个规律是否对任意输入都成立 |
| 输入来源 | 手写样例 | 自动生成数据 |
| 验证方式 | 点验证 | 面覆盖 |
| 更擅长发现 | 已知边界和回归 bug | 未知边界和隐藏异常 |

## Common Property Families

来源中的例子可以抽象成几类常见属性：

| 属性类型 | 例子 | 典型场景 |
|---|---|---|
| Roundtrip | `Parse(Generate(x)) == x` | 编解码、token、序列化 |
| Invariant | 排序后仍然有序、长度不变、元素不丢失 | 排序、转换、集合操作 |
| Safety Bound | 总金额 `>= 0`、结果不溢出 | 金额计算、边界控制 |
| Idempotence | 同一请求多次执行结果一致 | API、任务执行、批处理 |
| Order Independence | 输入顺序改变不应改变集合结果 | 批处理、聚合、去重 |

## Where It Fits in the Testing Pyramid

- 它主要位于单元测试层，用来补足“少量样例无法覆盖整个输入空间”的问题。
- 在少量场景下，它也可以进入集成测试层，用来验证系统性质，例如 API 幂等性或批处理顺序无关性。
- 它不是为了替代单元测试，而是和单元测试形成“具体例子 + 通用规律”的组合。

## Why It Matters

- 它能把“我只想到几个例子”扩展成“我对这类输入空间的规律有把握”。
- 它特别适合验证哈希、token、编解码、金额计算、缓存语义和排序等有清晰性质的逻辑。
- 它可以在 bug 尚未出现之前暴露一些人为难以枚举的边界条件。

## Documentation Implications

- 文档应先写清属性是什么，再写工具和语法；核心不是“随机”，而是“不变量”。
- 当使用属性测试时，最好说明生成器覆盖了什么输入边界、排除了什么无效域。
- 对团队培训材料，适合把“点验证 vs 面覆盖”作为最先建立的心智模型。
- 如果某个历史 bug 能被抽象成通用性质，文档应说明它何时应升级成属性测试，而不只是留作单个回归样例。

## Common Misconceptions

| 误解 | 更准确的理解 |
|---|---|
| 属性测试可以替代所有单元测试 | 它更适合补强，而不是替代具象例子和回归样例 |
| 属性测试只适合数学算法 | 认证、缓存、批处理、接口幂等性等业务逻辑同样适合 |
| 自动生成输入就一定会让测试不稳定 | 关键在于性质设计、输入约束和失败结果是否可复现 |

## Related Pages

- [[linux-foundations-and-testing-special-topic]]
- [[software-testing-architecture]]
- [[devops-delivery-pipeline]]
- [[full-lifecycle-delivery-capability]]
- [[validation-driven-design]]
- [[glossary]]
- [[overview]]
