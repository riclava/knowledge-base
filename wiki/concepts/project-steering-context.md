---
title: 项目 Steering 上下文
type: concept
created: 2026-04-15
updated: 2026-04-15
sources: [快速基于 AI 入门全栈研发.md]
tags: [concept, ai, steering, context, project-guidance, documentation]
---

项目 Steering 上下文指把技术栈、团队规范和业务背景持续写成项目级说明文件，让 AI 协作者能够基于真实项目约束给出更稳定、更贴近实际的输出。

## Definition

来源把 `Skills / Steering` 作为 AI 协作质量的重要抓手，核心意思不是“再写一段 Prompt”，而是把上下文持久化。

- 技术栈说明：使用什么框架、数据库、语言和基础设施
- 团队规范：命名、提交、分支、测试和代码风格约定
- 项目背景：业务目标、核心模块、边界和关键术语

来源用 `.kiro/steering/*.md` 作为例子，但这个概念本身并不依赖某个特定工具。

## What It Changes

- 让 AI 每次进入仓库时都有稳定的“项目记忆”
- 降低回答泛化、脱离上下文或违背团队规范的概率
- 把原本只存在于口头传承里的规则显式写出来，便于人和 AI 共享

## Relationship to Prompting

- Prompt 更像一次性任务指令。
- Steering 更像长期有效的项目运行手册。
- 两者不是替代关系，而是“稳定上下文 + 当前任务”组合。

## Why It Matters

- 当 AI 从“代码补全”升级到“任务执行”后，项目约束如果不显式表达，就会被默认值替代。
- 对新手来说，Steering 文件也是理解项目结构和团队规则的快速入口。
- 对技术写作来说，这类文件天然适合作为知识沉淀对象，因为它们跨越单次对话、单个 PR 和单个开发者。

## Documentation Implications

- 应把项目背景、技术栈、命名规范、测试要求和分支策略写成稳定文档，而不是散落在聊天记录里。
- 如果不同仓库或团队有不同 AI 协作边界，应单独记录，不要把局部约定误写成全局事实。
- Steering 文档适合与术语表、架构概览、贡献指南和测试规范互相链接。
- 写作时应优先记录“为什么这么做”和“哪些约束不能破坏”，而不只是列一串命令。

## Common Misconceptions

- “Steering 文件就是厂商专属功能。”
  它更接近项目级持久化上下文，只是不同工具提供的承载形式不同。
- “有了 Steering，AI 就能自动遵守所有规范。”
  它只能提高一致性，不能替代 review、测试和验收门禁。

## Related Pages

- [[ai-full-stack-development-onboarding]]
- [[model-context-protocol]]
- [[ai-enabled-software-delivery]]
- [[full-lifecycle-delivery-capability]]
- [[growing-engineer]]
- [[glossary]]
