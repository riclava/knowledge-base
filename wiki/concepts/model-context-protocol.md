---
title: Model Context Protocol（MCP）
type: concept
created: 2026-04-15
updated: 2026-04-15
sources: [快速基于 AI 入门全栈研发.md]
tags: [concept, ai, mcp, tooling, agent, integration]
---

Model Context Protocol（MCP）是让 AI 模型连接外部工具和数据源的协议层，使其从“只会对话”扩展为“可查询、可执行、可操作”的协作系统。

## Definition

在当前来源中，MCP 被描述为“给 AI 装上手”的开放协议。

- AI 不再只生成建议，还可以读取文件、调用 API、查询数据库、执行命令。
- 开发者可以通过编写或接入 MCP Server，把现有系统能力暴露给 AI。
- 重点不在某个具体产品，而在“模型如何安全、结构化地获得工具使用权”。

## Operational Pattern

来源给出的典型交互是：

```text
用户说：“帮我查一下数据库里最近注册的 10 个用户”
  -> AI 调用 MCP Database Server
  -> 执行 SQL 查询
  -> 返回结果并解释
```

这说明 MCP 的价值是把“意图理解”与“外部系统操作”接到一起。

## Common Capability Surface

- 文件系统：读写项目文件、配置、文档
- 数据库：执行查询、解释结果、辅助排障
- 浏览器控制：验证页面行为、抓取界面信息
- 开发协作系统：Git、Slack、Jira 等

## Why It Matters

- MCP 让 AI 从“代码生成器”升级为“工作流参与者”。
- 一旦 AI 可以调用真实工具，文档就不能只描述 Prompt，还要描述可用能力边界、权限、失败处理和审计方式。
- 对入门者来说，MCP 是理解 AI Agent 为什么能“真的做事”的关键概念。

## Documentation Implications

- 应明确区分“模型知道什么”和“模型能操作什么”。
- 应列出可接入的工具类别、授权边界和典型使用场景，而不是只说“支持插件”。
- 如果接入数据库、代码库或内部系统，文档需要补充风险控制、最小权限和人工复核要求。
- 当知识库记录 AI 工作流时，适合把 MCP 作为基础设施概念，而不是散落在单个工具介绍里。

## Related Pages

- [[ai-full-stack-development-onboarding]]
- [[ai-enabled-software-delivery]]
- [[project-steering-context]]
- [[devops-delivery-pipeline]]
- [[full-lifecycle-delivery-capability]]
- [[glossary]]
