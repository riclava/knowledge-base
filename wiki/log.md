# Wiki Log

Append-only chronological record of all activity: ingests, queries, and lint passes.

To view recent activity: `grep "^## \[" log.md | tail -10`

---

## [2026-04-07] init | Wiki created

Wiki initialized for a technical writer's personal knowledge base.

Structure created:
- `raw/` — source documents folder
- `wiki/` — LLM-maintained knowledge base
- `wiki/sources/` — per-source summary pages
- `CLAUDE.md` — schema and operating instructions

Core pages created:
- `wiki/index.md`
- `wiki/log.md`
- `wiki/overview.md`
- `wiki/glossary.md`

Next step: Drop your first source into `raw/` and say **"ingest [filename]"**.

## [2026-04-14] ingest | 2025年技术线总结

Pages created:
- `wiki/sources/2025-technical-line-summary.md`
- `wiki/personas/technical-line-leader.md`
- `wiki/concepts/full-lifecycle-delivery-capability.md`
- `wiki/concepts/ai-enabled-software-delivery.md`
- `wiki/concepts/platform-foundation.md`
- `wiki/concepts/observability-and-reliability.md`

Pages updated:
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Filed the first source summary for a technical-line annual retrospective and planning report.
- Established core concepts for AI-enabled delivery, platform foundation, observability/reliability, and full-lifecycle delivery capability.
- Added a technical-line leader persona and canonical terminology for AI辅助开发、AI融合、底座、可观测性、LLMOps 等表述。

## [2026-04-14] ingest | Moshi 与神经音频编码技术架构解析

Pages created:
- `wiki/sources/moshi-neural-audio-codec-architecture-analysis.md`
- `wiki/products/moshi.md`
- `wiki/products/mimi.md`
- `wiki/concepts/speech-native-llm.md`
- `wiki/concepts/neural-audio-codec.md`
- `wiki/concepts/full-duplex-speech-interaction.md`

Pages updated:
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Added a source summary for Moshi, Mimi, and the Neural Audio Codec landscape.
- Established core concepts for speech-native LLMs, neural audio codecs, and full-duplex speech interaction.
- Added product pages for Moshi and Mimi, plus canonical terminology for speech-native LLM、全双工对话、Neural Audio Codec、语音 LLM tokenizer、RVQ 等表述。
