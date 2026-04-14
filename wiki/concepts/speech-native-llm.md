---
title: 语音原生大模型
type: concept
created: 2026-04-14
updated: 2026-04-14
sources: [Moshi 与神经音频编码（Neural Audio Codec）技术架构解析.md]
tags: [concept, speech-llm, realtime, multimodal, tokenizer]
---

语音原生大模型是直接以语音 token 为核心输入输出对象的大模型架构，目标是绕开文本中心流水线带来的延迟、信息损失与交互僵硬问题。

## Definition

相较于传统 `ASR -> LLM -> TTS` 级联，speech-native LLM 的主路径是直接对语音进行建模与生成，必要时只在内部保留文本流作为推理辅助。

## Traditional vs Speech-Native

| 维度 | 传统级联系统 | 语音原生大模型 |
|---|---|---|
| 主模态 | 文本 | 语音 |
| 架构形态 | 多模块串行 | 端到端或近端到端 |
| 交互节奏 | turn-based | streaming / full-duplex |
| 信息保留 | 容易丢失韵律、语气、情绪 | 更容易保留语音原生信息 |
| 关键基础设施 | ASR/TTS 质量 | Neural Audio Codec / tokenizer |

## Core Characteristics

- 语音输入和语音输出都是一等公民，而不是文本系统外围接口。
- 常见设计会把语义逻辑和声学细节拆分成不同建模层。
- 低延迟、边听边说和可打断性不是增强功能，而是架构目标。
- 是否有合适的音频 tokenizer，直接影响模型可训练性与可交互性。

## Architectural Pattern in This Source

- 外部交互：用户语音流和模型语音流持续并行。
- 内部推理：可保留 `inner monologue` 文本流，用于逻辑性和语义规划。
- 分层建模：时间维负责语义和对话，深度维负责声学细节。

## Why It Matters

- 语音原生路线把语音交互从“慢一步的界面包装”变成“实时交互系统”。
- 这类系统更适合自然对话、实时反馈和可打断场景。
- 技术选型重点会从单独优化 ASR/TTS 转向优化 codec、延迟预算和流式推理。

## Documentation Implications

- 写作时要区分“语音输入能力”和“语音原生架构能力”。
- 架构图中应明确音频 tokenizer、流式交互和内部文本流的关系。
- 对外介绍应优先解释交互体验差异，再解释模型结构细节。

## Related Pages

- [[moshi]]
- [[mimi]]
- [[neural-audio-codec]]
- [[full-duplex-speech-interaction]]
- [[moshi-neural-audio-codec-architecture-analysis]]
