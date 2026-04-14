---
title: 全双工语音交互
type: concept
created: 2026-04-14
updated: 2026-04-14
sources: [Moshi 与神经音频编码（Neural Audio Codec）技术架构解析.md]
tags: [concept, dialogue, realtime, duplex, speech]
---

全双工语音交互允许系统边听边说、支持打断和语音重叠，是语音原生对话系统区别于轮次式交互的关键能力。

## Definition

与等待用户说完、系统再回复的 turn-based 模式不同，全双工交互要求输入语音流和输出语音流能够持续并行。

## Turn-Based vs Full-Duplex

| 维度 | 轮次式交互 | 全双工交互 |
|---|---|---|
| 说话节奏 | 一问一答 | 连续流式 |
| 打断支持 | 弱 | 强 |
| 重叠说话 | 通常不支持 | 可支持 |
| 体验特征 | 像操作机器 | 更接近自然对话 |

## Building Blocks in This Source

- User stream：持续监听用户语音输入
- Model stream：持续输出模型语音
- Inner monologue：在内部保留文本推理流，协调逻辑与表达
- 低延迟 codec 和推理链路：把总延迟压缩到约 160-200ms

## Why It Matters

- 打断能力决定语音系统是否能真正进入自然对话场景。
- 实时反馈会显著改变用户对“是否在交流”的感受。
- 只有当 codec、推理和生成都支持流式，full-duplex 才能成立。

## Documentation Implications

- 设计文档应说明支持哪些打断、抢话、重叠语音场景。
- 体验文档应把“边听边说”写成核心交互能力，而不是一句营销描述。
- 评估指标中要补充端到端延迟与打断恢复行为，而不仅是识别准确率。

## Related Pages

- [[moshi]]
- [[speech-native-llm]]
- [[mimi]]
- [[moshi-neural-audio-codec-architecture-analysis]]
