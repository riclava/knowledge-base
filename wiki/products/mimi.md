---
title: Mimi
type: product
created: 2026-04-14
updated: 2026-04-14
sources: [Moshi 与神经音频编码（Neural Audio Codec）技术架构解析.md]
tags: [product, audio-codec, tokenizer, speech-llm, low-latency]
---

Mimi 是面向语音 LLM 的 Neural Audio Codec + Tokenizer，把连续语音压缩成低频离散 token，兼顾语义表达、声学保真和低延迟。

## Product Snapshot

| 项目 | 内容 |
|---|---|
| 输入音频 | 24kHz |
| 输出 token rate | 12.5Hz |
| 码率 | ~1.1 kbps |
| 单组件延迟 | ~80ms |
| 主要用途 | 供 speech-native LLM 使用的音频 tokenizer |

## Role in the Stack

- 把音频转换成 LLM 可消费的离散 token。
- 降低序列长度，使实时推理可行。
- 尽量同时保留语义与声学信息，减少模型侧的信息缺口。

## Why Mimi Is Positioned as Infrastructure

- 文中把 Mimi 类比为 NLP 中的 `BPE / Tokenizer`。
- 它决定后续大模型看到的基本单位、序列长度和可恢复的信息类型。
- 在语音原生架构里，codec 不再只是编码器，而是模型能力边界的一部分。

## Comparison Context

| 对比项 | Mimi | EnCodec | SNAC |
|---|---|---|---|
| 目标 | 语音 LLM tokenizer | 通用 codec baseline | 前沿多尺度 codec |
| 适配重点 | LLM 接入与实时性 | 通用音频编码 | 语义/声学分层建模 |
| 文中定位 | 当前优先选项 | 演化前身 | 前沿探索方向 |

## Selection Guidance from This Source

- 构建类似 Moshi 的语音 LLM：优先考虑 Mimi。
- 探索更前沿的 codec：可关注 SNAC。
- 纯语音理解任务：更适合 SimWhisper-Codec 这类语义优先路线。

## Documentation Notes

- 评估 Mimi 时不能只写“压缩率”，还应记录 token 率、延迟和语义/声学保留能力。
- 与 Moshi 一起描述时，Mimi 应归入“基础设施层”而不是附属组件。

## Related Pages

- [[moshi-neural-audio-codec-architecture-analysis]]
- [[moshi]]
- [[neural-audio-codec]]
- [[speech-native-llm]]
