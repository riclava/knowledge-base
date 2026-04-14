---
title: 平台底座
type: concept
created: 2026-04-14
updated: 2026-04-14
sources: [2025年技术线总结.md]
tags: [concept, platform, foundation, governance, reuse]
---

平台底座是技术线用于支撑复用、治理和 AI 落地的共享基础能力集合，也是减少烟囱式系统的主要手段。

## Definition

在这份汇报里，底座不是泛指“公共组件”，而是带有明确治理目标的平台化能力。它既服务传统工程效率，也服务 AI 基础层建设。

## Included Capabilities

| 能力类型 | 文档中提到的能力 |
|---|---|
| 基础平台 | 配置中心、日志监控告警、权限网关 |
| AI 基础层 | 推理服务、向量存储、LLMOps |
| 工程治理 | 统一代码风格、接口规范、文档标准 |

## Strategic Role

- 通过架构收敛减少烟囱式系统。
- 让新系统复用共享能力，而不是从零重复建设。
- 为 1-2 人小团队提供可依赖的高复用环境。
- 把 AI 从试点项目扩展为可复制能力。

## Metrics

- 新系统 70% 复用平台能力
- 核心服务标准化 >= 80%

## Documentation Implications

- 需要一份平台能力地图，区分共享能力与项目定制能力。
- 需要记录底座治理原则、接入方式和复用边界。
- 需要把 AI 基础层纳入平台文档，而不是单独散落在项目说明里。

## Related Pages

- [[2025-technical-line-summary]]
- [[technical-line-leader]]
- [[ai-enabled-software-delivery]]
- [[full-lifecycle-delivery-capability]]
- [[observability-and-reliability]]
