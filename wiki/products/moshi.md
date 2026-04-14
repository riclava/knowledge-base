---
title: Moshi
type: product
created: 2026-04-14
updated: 2026-04-14
sources: [Moshi 与神经音频编码（Neural Audio Codec）技术架构解析.md]
tags: [product, speech-llm, speech-native, realtime-dialogue, duplex]
---

Moshi 是一个面向实时语音对话的 speech-native LLM，通过全双工交互和低延迟语音生成来替代传统级联式语音流水线。

## Product Snapshot

| 项目 | 内容 |
|---|---|
| 产品类型 | speech-native LLM / 实时语音对话模型 |
| 交互方式 | Full-Duplex，支持边听边说与打断 |
| 输入输出 | 语音流到语音流，内部保留文本流 |
| 核心目标 | 降低延迟、减少信息损失、提升自然对话能力 |
| 端到端延迟 | 约 160-200ms（按来源描述） |

## What Problem It Solves

- 避免 `ASR -> 文本 -> LLM -> 文本 -> TTS` 的串行延迟累积。
- 保留语气、情绪、韵律等在文本中难以完整表达的信息。
- 让系统从轮次式问答转向连续对话和实时反馈。

## Architecture

### Three Streams

- User stream：持续输入的用户语音流
- Model stream：持续输出的模型语音流
- Inner monologue：内部文本流，用于推理与语义规划

### Model Layers

- Temporal Transformer：负责时间维建模、语义理解和对话逻辑
- Depth Transformer：负责音频 token 之间的细粒度声学关系
- Mimi：提供离散音频 token，承担 tokenizer 基础设施角色

## Why It Matters

- 它把语音从“文本系统的 I/O 外壳”提升为主模态。
- 它把低延迟与可打断性视为架构基本能力，而不是后处理优化。
- 它说明语音 LLM 的关键壁垒不仅在大模型本身，也在 codec/tokenizer 层。

## Documentation Notes

- 文档应明确区分 `speech-native LLM` 和“接了语音接口的文本 LLM”。
- 介绍 Moshi 时，建议同时呈现交互流、模型层次和 latency budget。
- 若用于产品文档，应单独说明“可打断”“边听边说”“连续对话”的用户体验含义。

## Known Gaps from This Source

- 来源没有提供训练数据规模、部署资源、评估基准或开源状态。
- 来源强调技术架构意义，但没有展开具体应用场景、失败模式和工程限制。

## Related Pages

- [[moshi-neural-audio-codec-architecture-analysis]]
- [[mimi]]
- [[speech-native-llm]]
- [[full-duplex-speech-interaction]]
- [[neural-audio-codec]]
