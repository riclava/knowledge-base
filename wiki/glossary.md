---
title: Glossary
type: glossary
created: 2026-04-07
updated: 2026-04-14
sources: [2025年技术线总结.md]
tags: [terminology, style, glossary, ai, engineering-management]
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
