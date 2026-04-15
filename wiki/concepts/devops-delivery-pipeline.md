---
title: DevOps 交付流水线
type: concept
created: 2026-04-15
updated: 2026-04-15
sources: [快速基于 AI 入门全栈研发.md]
tags: [concept, devops, ci-cd, delivery, deployment, observability]
---

DevOps 交付流水线描述的是把开发、测试、扫描、构建、部署和运行反馈串成一条持续交付闭环，而不是把“上线”当作研发流程的终点。

## Definition

在当前来源中，这条链路从开发者本地工作台开始，经过 CI/CD，一直延伸到生产环境和监控告警反馈。

## Stage Breakdown

| 阶段 | 关键动作 | 说明 |
|---|---|---|
| 开发 | 编码、AI 辅助、本地测试、`git push` / `PR` | 代码变更进入团队协作轨道 |
| CI | Lint / Format、单元测试 / E2E、代码扫描 | 在合并或发布前做自动质量门禁 |
| CD | 构建 Docker 镜像、推送镜像仓库、分环境部署 | 把代码制品化并送到运行环境 |
| 生产 | 反向代理、应用容器、数据库/缓存 | 承接真实流量与状态 |
| 反馈 | 日志、性能监控、告警通知 | 用运行数据反向驱动修复与迭代 |

## Why It Matters

- 它把“开发完成”的定义从“代码写完了”扩展成“能被验证、部署、运行并持续观察”。
- 这条链路也是理解“完整交付能力”的最直观框架之一。
- 在 AI 协作时代，生成代码的速度会提高，因此 CI/CD 和运行反馈更像稳定人机协作质量的护栏。

## Documentation Implications

- 入门文档应明确每一阶段的进入条件、产出和失败返回路径。
- 流程图里出现的具体工具名应标明是示例还是团队标准，例如 SonarQube、ELK、Loki、Prometheus。
- 部署文档应和监控、告警、回滚文档互相链接，避免把运行期责任从交付链路里切掉。
- 如果 AI 参与代码生成或任务执行，文档还应说明哪些环节必须人工 review、哪些检查必须自动通过。

## Known Gaps from This Source

- 来源只给出了高层流程图，没有补充分支策略、环境晋级规则、回滚方案和发布权限模型。
- 也没有展开镜像版本管理、数据库迁移、灰度发布、SLO 或事故响应细节。

## Related Pages

- [[ai-full-stack-development-onboarding]]
- [[full-lifecycle-delivery-capability]]
- [[observability-and-reliability]]
- [[ai-enabled-software-delivery]]
- [[docker]]
- [[glossary]]
