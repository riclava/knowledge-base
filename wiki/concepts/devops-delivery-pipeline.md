---
title: DevOps 交付流水线
type: concept
created: 2026-04-15
updated: 2026-04-16
sources: [快速基于 AI 入门全栈研发.md, Linux基础与测试专题.md]
tags: [concept, devops, ci-cd, delivery, deployment, observability, testing, quality]
---

DevOps 交付流水线描述的是把开发、测试、扫描、构建、部署和运行反馈串成一条持续交付闭环，而不是把“上线”当作研发流程的终点。新来源进一步把这条流水线中的测试执行时机和反馈速度拆成了更清晰的层次。

## Definition

在当前来源中，这条链路从开发者本地工作台开始，经过 CI/CD，一直延伸到生产环境和监控告警反馈。新的测试专题则补上了“哪些测试应该在什么时候跑、为什么不能全堆在一层”的质量节奏。

## Stage Breakdown

| 阶段 | 关键动作 | 说明 |
|---|---|---|
| 开发 | 编码、AI 辅助、本地测试、`git push` / `PR` | 代码变更进入团队协作轨道 |
| CI | Lint / Format、单元测试 / E2E、代码扫描 | 在合并或发布前做自动质量门禁 |
| CD | 构建 Docker 镜像、推送镜像仓库、分环境部署 | 把代码制品化并送到运行环境 |
| 生产 | 反向代理、应用容器、数据库/缓存 | 承接真实流量与状态 |
| 反馈 | 日志、性能监控、告警通知 | 用运行数据反向驱动修复与迭代 |

## Quality Gates by Time Scale

| 时机 | 适合的验证 | 设计原则 |
|---|---|---|
| 每次提交 | 单元测试、属性测试、轻量集成测试 | 反馈必须快，避免团队绕过流水线 |
| Nightly | 全量集成、回归、大规模数据测试 | 扩大覆盖，但不阻塞日常提交 |
| 发布前 | E2E、性能测试、安全测试 | 面向真实发布风险做最终确认 |

## Environment Alignment

| 环境 | 主要目的 | 更适合的验证 |
|---|---|---|
| Dev | 本地开发与快速反馈 | 单元测试、属性测试 |
| Test / Staging | 逼近真实依赖与部署形态 | 集成测试、E2E |
| Production | 接受真实流量并持续观测 | 监控、告警、运行验证 |

## Why It Matters

- 它把“开发完成”的定义从“代码写完了”扩展成“能被验证、部署、运行并持续观察”。
- 这条链路也是理解“完整交付能力”的最直观框架之一。
- 在 AI 协作时代，生成代码的速度会提高，因此 CI/CD 和运行反馈更像稳定人机协作质量的护栏。
- 快速反馈同样重要；如果提交级流水线过慢，团队往往会绕过测试，质量门禁就会失效。

## Documentation Implications

- 入门文档应明确每一阶段的进入条件、产出和失败返回路径。
- 流程图里出现的具体工具名应标明是示例还是团队标准，例如 SonarQube、ELK、Loki、Prometheus。
- 部署文档应和监控、告警、回滚文档互相链接，避免把运行期责任从交付链路里切掉。
- 如果 AI 参与代码生成或任务执行，文档还应说明哪些环节必须人工 review、哪些检查必须自动通过。
- 测试规范应与流水线文档互相链接，明确单元、属性、集成、E2E、性能和安全测试分别位于哪条门禁上。

## Known Gaps from This Source

- 来源只给出了高层流程图，没有补充分支策略、环境晋级规则、回滚方案和发布权限模型。
- 也没有展开镜像版本管理、数据库迁移、灰度发布、SLO 或事故响应细节。
- 目前仍缺少团队级的提交耗时目标、回归数据管理规范和测试失败分流策略。

## Related Pages

- [[ai-full-stack-development-onboarding]]
- [[linux-foundations-and-testing-special-topic]]
- [[full-lifecycle-delivery-capability]]
- [[observability-and-reliability]]
- [[ai-enabled-software-delivery]]
- [[software-testing-architecture]]
- [[property-based-testing]]
- [[docker]]
- [[glossary]]
