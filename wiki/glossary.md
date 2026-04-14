---
title: Glossary
type: glossary
created: 2026-04-07
updated: 2026-04-14
sources: [2025年技术线总结.md, Moshi 与神经音频编码（Neural Audio Codec）技术架构解析.md]
tags: [terminology, style, glossary, ai, engineering-management, speech-llm]
---

# Glossary

Living reference of terms, definitions, and style conventions. The LLM checks this before using any technical term. Updated on every ingest that introduces new or refined terminology.

---

## How to Read This Glossary

Each entry follows this format:

**Term** *(canonical form)*
: Definition. Usage notes. Related terms.
- Preferred: `term` / Avoid: `deprecated term`
- See also: `\[\[related-page\]\]`

---

## Terminology

**AI辅助开发** *(canonical form)*
: 指在编码、调试、测试、运维等研发活动中引入 AI 协同，以提升局部效率和交付速度。
- Preferred: `AI辅助开发` / Avoid: `只把 AI 当成代码补全工具`
- See also: [[ai-enabled-software-delivery]]

**AI融合** *(canonical form)*
: 指把 AI 能力嵌入现有项目或业务功能中，形成用户可见、可使用、可衡量的功能结果。
- Preferred: `AI融合` / Avoid: `AI 试点` when the work is already part of正式项目交付
- See also: [[ai-enabled-software-delivery]]

**完整交付能力** *(canonical form)*
: 指覆盖需求、设计、开发、测试、发布和运维的端到端软件产品交付能力。
- Preferred: `完整交付能力` / Avoid: `只强调开发能力`
- See also: [[full-lifecycle-delivery-capability]]

**底座** *(canonical form)*
: 指支撑复用、治理和 AI 落地的平台基础能力集合，可写作“平台底座”。
- Preferred: `底座` or `平台底座` / Avoid: `公共部分` when referring to strategic shared capability
- See also: [[platform-foundation]]

**可观测性** *(canonical form)*
: 通过日志、指标和 Trace 理解系统运行状态、定位问题和支撑治理的能力。
- Preferred: `可观测性` / Avoid: `只说监控` when logs, metrics, and traces are all involved
- See also: [[observability-and-reliability]]

**LLMOps** *(canonical form)*
: 面向大语言模型应用的工程化运营能力，通常涉及模型接入、评估、运行、监控和治理。
- Preferred: `LLMOps`
- See also: [[ai-enabled-software-delivery]], [[platform-foundation]]

**烟囱式系统** *(canonical form)*
: 指重复建设、彼此割裂、复用性差的系统形态，是平台化建设试图减少的反模式。
- Preferred: `烟囱式系统`
- See also: [[platform-foundation]]

**鸿蒙移动端适配** *(canonical form)*
: 指面向鸿蒙生态进行移动端适配的工作方向。在当前来源中，它被标记为尚未形成理想落地结果。
- Preferred: `鸿蒙移动端适配`
- See also: [[2025-technical-line-summary]]

**信创 Linux** *(canonical form)*
: 指面向信创环境的 Linux 适配与落地工作。在当前来源中，四川和重庆区域尚未形成较好落地。
- Preferred: `信创 Linux`
- See also: [[2025-technical-line-summary]]

**speech-native LLM** *(canonical form)*
: 指直接对语音 token 进行建模、推理与生成的大模型，内部可保留文本流，但不再依赖 `ASR -> LLM -> TTS` 串行级联作为主路径。
- Preferred: `speech-native LLM` or `语音原生大模型` / Avoid: `只是接了语音接口的文本 LLM`
- See also: [[speech-native-llm]], [[moshi]]

**全双工对话** *(canonical form)*
: 指系统可以边听边说、支持打断与语音重叠的实时对话模式，区别于严格轮次式交互。
- Preferred: `全双工对话` / Avoid: `实时语音` when the key point is simultaneous listening and speaking
- See also: [[full-duplex-speech-interaction]], [[moshi]]

**Neural Audio Codec** *(canonical form)*
: 指把音频压缩并离散化为 token 的神经编解码技术。在语音 LLM 语境中，它承担类似文本 tokenizer 的基础设施角色。
- Preferred: `Neural Audio Codec` or `神经音频编码`
- See also: [[neural-audio-codec]], [[mimi]]

**语音 LLM tokenizer** *(canonical form)*
: 指可输出低频离散 token，并同时保留语义与声学信息、适合接入语音大模型的音频 tokenizer。
- Preferred: `语音 LLM tokenizer`
- See also: [[neural-audio-codec]], [[mimi]]

**Moshi** *(canonical form)*
: 代表语音原生、全双工、低延迟对话路线的 speech-native LLM 产品/模型名称。
- Preferred: `Moshi`
- See also: [[moshi]], [[speech-native-llm]]

**Mimi** *(canonical form)*
: Moshi 路线中的 Neural Audio Codec + Tokenizer，用于把 24kHz 音频编码为低频离散 token。
- Preferred: `Mimi`
- See also: [[mimi]], [[neural-audio-codec]]

**inner monologue** *(canonical form)*
: 指语音原生模型内部保留的文本推理流，用于逻辑组织和语义规划，不直接等同于最终输出语音。
- Preferred: `inner monologue`
- See also: [[moshi]], [[speech-native-llm]]

**Temporal Transformer** *(canonical form)*
: 在 Moshi 架构中负责时间维建模、语义理解和对话逻辑的主模型层。
- Preferred: `Temporal Transformer`
- See also: [[moshi]]

**Depth Transformer** *(canonical form)*
: 在 Moshi 架构中负责音频 token 细粒度声学关系建模的模型层。
- Preferred: `Depth Transformer`
- See also: [[moshi]]

**残差向量量化（RVQ）** *(canonical form)*
: 一种常见的音频离散化与压缩方法，EnCodec 使用该路线；在当前来源中，RVQ 是理解 codec 演化谱系的基础术语。
- Preferred: `RVQ` or `残差向量量化`
- See also: [[neural-audio-codec]]

**轮次式交互（turn-based）** *(canonical form)*
: 指系统遵循“用户说完一轮，模型再回复一轮”的串行交互方式，通常不支持边听边说和自然打断。
- Preferred: `轮次式交互` or `turn-based`
- See also: [[full-duplex-speech-interaction]]

**EnCodec** *(canonical form)*
: Meta 提出的通用 Neural Audio Codec baseline，在当前来源中被视为 Mimi 的重要前身之一。
- Preferred: `EnCodec`
- See also: [[neural-audio-codec]], [[mimi]]

**SNAC** *(canonical form)*
: 一种强调多尺度量化的新一代 Neural Audio Codec 路线，在当前来源中被视为值得跟踪的前沿探索方向。
- Preferred: `SNAC`
- See also: [[neural-audio-codec]], [[mimi]]

**SpeechTokenizer / SemanticCodec** *(canonical form)*
: 指更强调语义表达的 codec/tokenizer 路线，在当前来源中因 token 率较高而被认为不利于直接接入长序列语音 LLM。
- Preferred: `SpeechTokenizer` or `SemanticCodec`
- See also: [[neural-audio-codec]]

**SimWhisper-Codec** *(canonical form)*
: 基于 Whisper encoder 的语义优先 codec 路线，在当前来源中更适合语音理解类任务。
- Preferred: `SimWhisper-Codec`
- See also: [[neural-audio-codec]]

**Lyra / Satin** *(canonical form)*
: 主要面向通信压缩的 codec 路线，在当前来源中因不输出离散 token 而不被归入语音 LLM tokenizer。
- Preferred: `Lyra` / `Satin`
- See also: [[neural-audio-codec]]

---

## Style Conventions

*(Writing rules and tone guidelines specific to this knowledge base's domain. Will populate as style guides and branded content are ingested.)*

| Convention | Rule | Example |
|---|---|---|
| Canonical AI terms | Use `AI辅助开发` for engineering-process collaboration and `AI融合` for product/function embedding. | “通过 AI辅助开发提效，并在项目中推进 AI融合。” |
| Platform terminology | Use `底座` or `平台底座` for shared strategic infrastructure, not generic “公共能力”. | “新系统优先复用平台底座能力。” |
| Delivery scope | Use `完整交付能力` when describing end-to-end ownership across the full lifecycle. | “小团队模式依赖完整交付能力。” |

---

## Deprecated / Avoid List

Terms that have been replaced, renamed, or should not be used:

| Avoid | Use Instead | Reason |
|---|---|---|
| 只说“监控” | `可观测性` | 当前语境明确包含日志、指标和 Trace，范围大于传统监控。 |
| 把 AI能力 统称为“工具” | `AI辅助开发` or `AI融合` | 需要区分工程过程提效与业务功能落地。 |
| 把底座写成“公共部分” | `底座` or `平台底座` | “底座”更准确表达复用、治理和战略支撑含义。 |

---

## Regional / Variant Terms

Terms that differ between audiences, teams, or locales:

| Term | Region/Context | Notes |
|---|---|---|
| 信创 Linux | 行业/政企项目语境 | 与国产化软硬件适配和落地相关。 |
| 鸿蒙移动端适配 | 终端生态语境 | 指对鸿蒙端的产品或应用适配工作。 |

---

## Related Pages

- [[overview]] — big-picture synthesis
- [[index]] — master catalog
- [[2025-technical-line-summary]] — first ingested source summary
- [[technical-line-leader]] — primary management persona inferred from the source
- [[full-lifecycle-delivery-capability]] — end-to-end delivery concept
- [[ai-enabled-software-delivery]] — AI-enabled engineering concept
- [[platform-foundation]] — shared platform and governance concept
- [[observability-and-reliability]] — observability and reliability concept
- [[moshi-neural-audio-codec-architecture-analysis]] — source summary for Moshi and codec architecture
- [[moshi]] — speech-native LLM product page
- [[mimi]] — audio tokenizer product page
- [[speech-native-llm]] — core speech-first architecture concept
- [[neural-audio-codec]] — codec infrastructure concept
- [[full-duplex-speech-interaction]] — real-time duplex interaction concept
