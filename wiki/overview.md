---
title: Overview
type: overview
created: 2026-04-07
updated: 2026-04-15
sources: [2025年技术线总结.md, Moshi 与神经音频编码（Neural Audio Codec）技术架构解析.md, vim.md]
tags: [overview, synthesis, engineering-management, ai, speech-llm, developer-tooling, linux, vim]
---

# Knowledge Base Overview

*This page is the LLM's working synthesis of everything in the wiki. It updates after every ingest that shifts the big picture.*

---

## Current State

This wiki currently covers AI-era engineering management, speech-native AI architecture, and practical Linux/developer-tooling knowledge, combining strategic planning material with hands-on workflow references.

**Source count:** 3
**Wiki pages:** 19
**Last ingest:** 2026-04-15 — [[vim-usage-and-configuration-reference]]
**Last lint:** —

---

## What This Wiki Covers

- 技术线年度复盘与规划材料
- AI 时代的软件交付方式变化
- 平台底座、稳定性和治理能力建设
- 技术管理者关注的组织、流程和指标体系
- 语音原生 LLM、Neural Audio Codec 与实时语音交互架构
- Linux / 终端环境中的开发者工具与文本编辑工作流

---

## Key Themes

- AI 不会替代工程基本盘，而是在其上叠加模型、数据、平台和新的组织分工。
- 小团队高效交付依赖完整交付能力，而不是单点英雄主义。
- 平台底座是复用、治理和 AI 落地的共同支点，目标是减少烟囱式系统。
- 可观测性、事故管理和容灾能力仍然是不可让步的工程基本功。
- 当前最大的治理短板是项目运营规范化尚未制度化。
- 在语音交互方向，关键基础设施正从传统 ASR/TTS 模块转向可供 LLM 直接消费的音频 tokenizer。
- 自然语音对话的竞争点不只是模型推理能力，还包括全双工交互、延迟预算和 codec 设计。
- 在终端工作流中，Vim 的价值不只是“能编辑文件”，而是通过模态编辑、组合命令和轻量配置把高频文本修改提炼成可复用操作语言。

---

## Open Questions

- 技术线在 2025 年完成交付的 AI 项目具体包括哪些项目和功能？
- 项目运营规范化未来会形成哪些具体制度、模板或会议机制？
- AI融合的“用户活跃度较高”采用什么统一衡量口径？
- 鸿蒙移动端适配和信创 Linux 落地是否已有后续补充材料？
- Moshi 的训练数据、评估指标、部署成本和开源状态是什么？
- Mimi 与 SNAC 在真实语音对话任务中的取舍边界是什么？
- 当前团队的编辑器基线是 Vim、Neovim，还是多编辑器并存？
- 是否还会补充 shell、tmux、git 或其他 Linux 通用工具资料，形成完整终端工作流体系？

---

## Knowledge Gaps

- 缺少平台底座能力清单和接入方式文档。
- 缺少事故管理、可观测性和容灾方案的具体规范。
- 缺少代码风格、接口规范、文档标准等治理类正式文档。
- 缺少对 1-2 人小团队模式的流程细化和案例材料。
- 缺少 speech-native LLM 的系统架构图、组件职责说明和延迟预算模板。
- 缺少 Neural Audio Codec 的选型矩阵、评估指标和任务分类方法。
- 缺少围绕 shell、tmux、git 和远程开发的配套开发者工具文档，暂未形成完整 Linux 工作流知识链。

---

## Related Pages

- [[index]] — full catalog of all wiki pages
- [[glossary]] — terminology and style conventions
- [[2025-technical-line-summary]] — current primary source summary
- [[moshi-neural-audio-codec-architecture-analysis]] — speech-native LLM source summary
- [[technical-line-leader]] — current primary persona
- [[full-lifecycle-delivery-capability]] — key delivery concept
- [[ai-enabled-software-delivery]] — AI transition concept
- [[platform-foundation]] — platform reuse and governance concept
- [[observability-and-reliability]] — stability and operations concept
- [[moshi]] — speech-native LLM product page
- [[mimi]] — audio tokenizer product page
- [[speech-native-llm]] — speech-first model architecture
- [[neural-audio-codec]] — codec/tokenizer infrastructure
- [[full-duplex-speech-interaction]] — real-time duplex interaction model
- [[vim-usage-and-configuration-reference]] — Vim source summary and command reference
- [[vim]] — modal editor tool page
- [[modal-editing]] — editing model behind Vim command composition
