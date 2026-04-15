---
title: Glossary
type: glossary
created: 2026-04-07
updated: 2026-04-15
sources: [2025年技术线总结.md, Moshi 与神经音频编码（Neural Audio Codec）技术架构解析.md, vim.md, bash.md, commands.md, CentOS6由于镜像废弃无法使用的解决办法.md]
tags: [terminology, style, glossary, ai, engineering-management, speech-llm, developer-tooling, linux, command-line, vim, bash, shell, centos, yum, repository]
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

**Linux** *(canonical form)*
: 以命令行、小工具组合和文本接口著称的 Unix-like 操作系统家族；在当前知识库中，它既是 [[bash]] 和 [[vim]] 的运行环境，也是系统巡检与运维操作的对象。
- Preferred: `Linux`
- See also: [[linux]], [[linux-command-line-operations]], [[bash]]

**信创 Linux** *(canonical form)*
: 指面向信创环境的 Linux 适配与落地工作。在当前来源中，四川和重庆区域尚未形成较好落地。
- Preferred: `信创 Linux`
- See also: [[2025-technical-line-summary]]

**CentOS** *(canonical form)*
: 基于 RPM/YUM 生态的企业级 Linux 发行版家族；在当前知识库中，主要出现在遗留版本维护和仓库恢复场景。
- Preferred: `CentOS`
- See also: [[centos]], [[linux]]

**YUM** *(canonical form)*
: CentOS/RHEL 家族常见的软件包管理与仓库访问工具，负责读取 repo 配置、获取元数据并安装 RPM 包。
- Preferred: `YUM` or `yum`
- See also: [[centos]], [[legacy-repository-repointing]]

**repo 文件** *(canonical form)*
: 指 `/etc/yum.repos.d/*.repo` 这类仓库配置文件，用来声明 `mirrorlist`、`baseurl`、`gpgcheck`、`gpgkey` 等字段。
- Preferred: `repo 文件` or `仓库配置文件`
- See also: [[centos]], [[legacy-repository-repointing]]

**mirrorlist / baseurl** *(canonical form)*
: `YUM` 仓库中两类常见的软件源定位方式；`mirrorlist` 倾向动态获取镜像列表，`baseurl` 倾向直接指定固定仓库地址。
- Preferred: `mirrorlist` / `baseurl`
- See also: [[centos]], [[legacy-repository-repointing]]

**vault 仓库** *(canonical form)*
: 指为历史版本保留包文件的 archive/vault 型静态仓库，常用于老版本发行版在默认镜像退役后的临时维护。
- Preferred: `vault 仓库` or `archive/vault 仓库`
- See also: [[legacy-repository-repointing]], [[centos]]

**fastestmirror** *(canonical form)*
: `YUM` 的镜像选择插件，用于从可用镜像中挑选更快的源；当发行版镜像已退役时，可能需要临时关闭。
- Preferred: `fastestmirror`
- See also: [[centos6-archive-repository-workaround]], [[legacy-repository-repointing]]

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

**Vim** *(canonical form)*
: 一个以模态编辑和组合式命令为核心的文本编辑器，常用于 Linux/终端环境中的代码、脚本和配置文件修改。
- Preferred: `Vim`
- See also: [[vim]], [[modal-editing]], [[vim-usage-and-configuration-reference]]

**vimrc** *(canonical form)*
: 用户的 Vim 主配置文件，通常指 `~/.vimrc`，用于设置界面、搜索、缩进、映射和插件加载行为。
- Preferred: `vimrc` or `~/.vimrc` / Avoid: 只写“配置文件”而不指明 Vim 语境
- See also: [[vim]], [[vim-usage-and-configuration-reference]]

**模态编辑（modal editing）** *(canonical form)*
: 一种根据当前模式切换按键语义的编辑模型。普通模式偏向导航和命令执行，插入模式偏向文本输入，可视模式偏向选择。
- Preferred: `模态编辑` or `modal editing`
- See also: [[modal-editing]], [[vim]]

**文本对象（text object）** *(canonical form)*
: 指可被操作符直接作用的一类结构化文本范围，例如引号内内容、括号内内容或连同包围符在内的整体区域。
- Preferred: `文本对象` or `text object`
- See also: [[modal-editing]], [[vim]]

**buffer** *(canonical form)*
: Vim 中已打开文件或文本内容的内存表示，可以脱离当前窗口独立存在，并在多个 buffer 之间切换。
- Preferred: `buffer`
- See also: [[vim]], [[vim-usage-and-configuration-reference]]

**vim-plug** *(canonical form)*
: 一个轻量级 Vim 插件管理器，用于声明、安装、更新和清理插件。
- Preferred: `vim-plug`
- See also: [[vim]], [[vim-usage-and-configuration-reference]]

**Bash** *(canonical form)*
: 一种常见于 Linux/Unix 环境的 shell 与脚本语言，既可交互执行命令，也可编排自动化脚本。
- Preferred: `Bash` / Avoid: 在需要区分语言或解释器时只泛写 `shell`
- See also: [[bash]], [[bash-syntax-and-scripting-reference]], [[shell-scripting]]

**shell scripting** *(canonical form)*
: 指使用 shell 内建能力、外部命令、管道和重定向编写自动化脚本的实践；在当前来源中主要指 Bash 脚本。
- Preferred: `shell scripting` or `Shell 脚本` / Avoid: 把所有 shell script 都默认视为可移植的 POSIX `sh`
- See also: [[shell-scripting]], [[bash]]

**参数展开（parameter expansion）** *(canonical form)*
: Bash 内置的变量展开与字符串处理机制，如默认值、截取、替换和前后缀删除，是减少外部文本处理命令的重要手段。
- Preferred: `参数展开` or `parameter expansion`
- See also: [[bash]], [[bash-syntax-and-scripting-reference]]

**Here Document** *(canonical form)*
: 用 `<<EOF` 形式向命令或文件提供多行输入的 shell 语法，可控制变量是否展开，并支持 `<<-EOF` 这种更易排版的缩进写法。
- Preferred: `Here Document` or `heredoc`
- See also: [[bash]], [[shell-scripting]]

**getopts** *(canonical form)*
: Bash 内建的短选项解析机制，适合为脚本建立规范命令行入口，并配合 `OPTARG`、`OPTIND` 处理参数消费。
- Preferred: `getopts`
- See also: [[bash]], [[shell-scripting]], [[bash-syntax-and-scripting-reference]]

**set -euo pipefail** *(canonical form)*
: Bash 中常见的防御性脚本组合，用于在命令失败、未定义变量或管道中间步骤失败时尽早暴露问题。
- Preferred: `set -euo pipefail`
- See also: [[bash]], [[shell-scripting]]

**rsync** *(canonical form)*
: 一种支持本地或远程目录同步的复制工具，强调保留元数据、增量传输和可视化进度，常用于替代简单递归复制。
- Preferred: `rsync`
- See also: [[linux]], [[linux-command-line-operations]], [[linux-common-commands-reference]]

**inode** *(canonical form)*
: 文件系统为文件或目录分配的元数据索引号；当文件名损坏、包含特殊字符或难以直接引用时，可以借助 inode 定位和删除目标。
- Preferred: `inode`
- See also: [[linux-command-line-operations]], [[linux-common-commands-reference]]

**grep / sed / awk** *(canonical form)*
: Linux/Unix 经典文本处理工具组，分别偏向过滤搜索、流式替换和按字段/条件处理，经常通过管道协同工作。
- Preferred: `grep` / `sed` / `awk`
- See also: [[linux-command-line-operations]], [[bash]], [[shell-scripting]]

**systemd** *(canonical form)*
: Linux 上常见的初始化与服务管理体系；在当前来源中，`journalctl` 作为其日志查询入口出现。
- Preferred: `systemd`
- See also: [[linux]], [[linux-common-commands-reference]]

**journalctl** *(canonical form)*
: `systemd` 日志查询工具，可按服务、时间范围或实时流查看系统日志。
- Preferred: `journalctl`
- See also: [[linux]], [[linux-command-line-operations]], [[linux-common-commands-reference]]

**crontab** *(canonical form)*
: 基于 cron 的定时任务配置入口，用于按分钟、小时、日期或星期调度命令与脚本。
- Preferred: `crontab`
- See also: [[linux-command-line-operations]], [[shell-scripting]], [[linux-common-commands-reference]]

**firewalld / firewall-cmd** *(canonical form)*
: Linux 上一种动态防火墙管理服务及其命令行入口；在当前来源中，它用于开放和关闭端口。
- Preferred: `firewalld` / `firewall-cmd`
- See also: [[linux]], [[linux-common-commands-reference]]

**直接 I/O（direct I/O）** *(canonical form)*
: 通过 `iflag=direct` 或 `oflag=direct` 绕过文件系统缓存的 I/O 模式，常用于磁盘读写基准测试或区分缓存命中影响。
- Preferred: `直接 I/O` or `direct I/O`
- See also: [[linux-command-line-operations]], [[linux-common-commands-reference]]

---

## Style Conventions

*(Writing rules and tone guidelines specific to this knowledge base's domain. Will populate as style guides and branded content are ingested.)*

| Convention | Rule | Example |
|---|---|---|
| Canonical AI terms | Use `AI辅助开发` for engineering-process collaboration and `AI融合` for product/function embedding. | “通过 AI辅助开发提效，并在项目中推进 AI融合。” |
| Platform terminology | Use `底座` or `平台底座` for shared strategic infrastructure, not generic “公共能力”. | “新系统优先复用平台底座能力。” |
| Delivery scope | Use `完整交付能力` when describing end-to-end ownership across the full lifecycle. | “小团队模式依赖完整交付能力。” |
| Editor terminology | Use `Vim` for the editor, `vimrc` for its config file, and `模态编辑` for the editing model. | “先解释模态编辑，再介绍 `~/.vimrc` 常用配置。” |
| Shell terminology | Use `Bash` when the content depends on Bash-specific syntax; use `shell scripting` for the broader automation practice. | “这段脚本使用了 Bash 关联数组，属于 Bash 专用写法。” |
| Linux operations terminology | When behavior depends on a specific subsystem, use the exact command or service name such as `journalctl`, `crontab`, or `firewalld`, not a vague “Linux 命令”. | “查看 `systemd` 日志时使用 `journalctl`。” |

---

## Deprecated / Avoid List

Terms that have been replaced, renamed, or should not be used:

| Avoid | Use Instead | Reason |
|---|---|---|
| 只说“监控” | `可观测性` | 当前语境明确包含日志、指标和 Trace，范围大于传统监控。 |
| 把 AI能力 统称为“工具” | `AI辅助开发` or `AI融合` | 需要区分工程过程提效与业务功能落地。 |
| 把底座写成“公共部分” | `底座` or `平台底座` | “底座”更准确表达复用、治理和战略支撑含义。 |
| 把 Bash 专有写法统称为“shell 通用语法” | `Bash` | `[[ ]]`、关联数组、丰富参数展开等能力并非所有 shell 都支持。 |
| 把 `journalctl`、`firewall-cmd` 等接口写成所有 Linux 环境通用命令 | 写明 `systemd`、`firewalld` 或具体发行版前提 | 避免把子系统特定行为误写成普适事实。 |

---

## Regional / Variant Terms

Terms that differ between audiences, teams, or locales:

| Term | Region/Context | Notes |
|---|---|---|
| 信创 Linux | 行业/政企项目语境 | 与国产化软硬件适配和落地相关。 |
| 鸿蒙移动端适配 | 终端生态语境 | 指对鸿蒙端的产品或应用适配工作。 |
| Vim / vim | 开发者工具语境 | 文中提及产品名称时优先写 `Vim`；命令名或文件路径中可写小写 `vim`。 |
| Bash / shell | 开发者工具语境 | 讲具体语法、关联数组、`getopts`、`[[ ]]` 时优先写 `Bash`；讲更宽泛的自动化实践时再写 `shell scripting`。 |
| `ip` / `ss` vs `ifconfig` / `netstat` | Linux 网络运维语境 | 文档优先使用更现代的 `ip`、`ss`，但可注明在旧系统中仍会遇到后者。 |

---

## Related Pages

- [[overview]] — big-picture synthesis
- [[index]] — master catalog
- [[2025-technical-line-summary]] — first ingested source summary
- [[linux]] — Linux platform/tool page
- [[linux-command-line-operations]] — reusable Linux operations concept
- [[linux-common-commands-reference]] — Linux commands source summary
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
- [[bash-syntax-and-scripting-reference]] — Bash source summary and scripting reference
- [[bash]] — Bash product/tool page
- [[shell-scripting]] — shell automation concept
- [[vim-usage-and-configuration-reference]] — Vim source summary and command reference
- [[vim]] — Vim product/tool page
- [[modal-editing]] — modal editing concept
