---
title: 神经音频编码（Neural Audio Codec）
type: concept
created: 2026-04-14
updated: 2026-04-14
sources: [Moshi 与神经音频编码（Neural Audio Codec）技术架构解析.md]
tags: [concept, audio-codec, tokenizer, speech-llm, compression]
---

Neural Audio Codec 在语音 LLM 中承担音频离散化与压缩角色，是语音 token 进入大模型的基础设施层。

## Definition

在当前来源语境中，Neural Audio Codec 不只是做音频压缩，还需要把连续语音转换成适合 LLM 消费的离散 token，同时尽量保留语义和声学信息。

## Why It Matters for Speech LLM

- 它决定模型看到的基本输入单位。
- 它直接影响序列长度、推理延迟和信息保留边界。
- 在语音原生路线里，它的地位接近 NLP 中的 tokenizer。

## Judgment Criteria

文中认为，一个模型要具备“语音 LLM tokenizer”价值，通常要满足：

1. 输出离散 token
2. token 频率足够低，通常低于 20Hz
3. 同时编码语义信息与声学信息

## Technology Lineage and Positioning

| 路线 | 文中定位 | 对语音 LLM 的意义 |
|---|---|---|
| SoundStream | 早期演化起点 | 提供基础 codec 思路 |
| EnCodec | 通用 baseline | RVQ 路线，适合理解后续演化 |
| Mimi | 当前优先选项 | 低 token 率，强调 LLM 适配 |
| SNAC | 前沿探索 | 多尺度量化，更偏向语义/声学分层 |
| SpeechTokenizer / SemanticCodec | 高语义表征 | token 率高，不利于长序列 LLM |
| SimWhisper-Codec | 语义优先路线 | 更适合理解任务而非实时生成 |
| Lyra / Satin | 通信 codec | 无离散 token，不适合语音 LLM |

## Documentation Implications

- 技术方案文档应把 codec 单独作为架构层，而不是附在 TTS 或音频处理章节后。
- 选型表至少应记录 token 率、离散化方式、延迟与语义/声学保留能力。
- 当讨论 speech-native LLM 时，应先解释 codec 的角色，再解释主模型结构。

## Related Pages

- [[mimi]]
- [[moshi]]
- [[speech-native-llm]]
- [[moshi-neural-audio-codec-architecture-analysis]]
