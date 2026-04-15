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

## [2026-04-15] ingest | Vim 常用配置与操作参考

Pages created:
- `wiki/sources/vim-usage-and-configuration-reference.md`
- `wiki/products/vim.md`
- `wiki/concepts/modal-editing.md`

Pages updated:
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Added the first Linux/developer-tooling source summary, centered on Vim configuration, editing commands, batch text operations, and plugin management.
- Established `Vim` as a product/tool page and `模态编辑` as a reusable editing-model concept page.
- Added canonical terminology for Vim、vimrc、模态编辑、文本对象、buffer 和 vim-plug, and expanded the overview to include terminal workflow knowledge.

## [2026-04-15] ingest | Bash 语法与脚本技巧

Pages created:
- `wiki/sources/bash-syntax-and-scripting-reference.md`
- `wiki/products/bash.md`
- `wiki/concepts/shell-scripting.md`

Pages updated:
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Added a Bash source summary covering variables, parameter expansion, arrays, control flow, option parsing, I/O, error handling, and common scripting tricks.
- Established `Bash` as a product/tool page and `shell scripting` as a reusable automation concept page.
- Added canonical terminology for Bash、shell scripting、参数展开、Here Document、getopts 和 `set -euo pipefail`, and expanded the overview from editor-centric Linux tooling toward broader terminal automation knowledge.

## [2026-04-15] ingest | Linux 常用命令

Pages created:
- `wiki/sources/linux-common-commands-reference.md`
- `wiki/products/linux.md`
- `wiki/concepts/linux-command-line-operations.md`

Pages updated:
- `wiki/products/bash.md`
- `wiki/concepts/shell-scripting.md`
- `wiki/glossary.md`
- `wiki/index.md`
- `wiki/overview.md`
- `wiki/log.md`

Key additions:
- Added a Linux command-line source summary spanning file operations, text processing, storage, processes, networking, permissions, archives, cron, and logs.
- Established `Linux` as a platform/tool page and `Linux command-line operations` as a reusable operations concept page.
- Added canonical terminology for Linux、`rsync`、`inode`、`grep/sed/awk`、`systemd`、`journalctl`、`crontab`、`firewalld` 和 `direct I/O`, and clarified how Bash scripts orchestrate common Linux commands.
