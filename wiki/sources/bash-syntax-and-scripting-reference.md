---
title: Bash 语法与脚本技巧
type: source
created: 2026-04-15
updated: 2026-04-15
sources: [bash.md]
tags: [bash, linux, shell, scripting, automation, cheat-sheet, source]
---

这是一份面向 Linux/终端工作流的 Bash 速查与脚本实践参考，覆盖变量、参数展开、流程控制、输入输出、错误处理和常见自动化技巧。

## Source Metadata

- Raw source: `raw/OS/Linux/Common/bash.md`
- 文档性质：语言速查 / 脚本实践笔记
- 主要对象：Bash 变量、参数展开、数组、条件与循环、函数、输入输出、错误处理
- 适用场景：命令行自动化、运维脚本、开发辅助脚本、批处理任务

## What the Source Covers

| 模块 | 覆盖内容 | 价值 |
|---|---|---|
| 变量与特殊参数 | 普通变量、只读变量、`unset`、`$0` 到 `$!`、默认值语法 | 建立脚本输入、状态和返回值的基本接口 |
| 字符串与参数展开 | 长度、截取、替换、前后缀删除、默认值、单引号转义 | 展示 Bash 内建文本处理能力，减少不必要的外部命令 |
| 数组与关联数组 | 索引数组、切片、遍历、`declare -A` | 支撑批量参数处理和简单键值映射 |
| 条件与循环 | `if`、`[[ ]]`、文件测试、数值比较、`for`/`while`/`until`/`case` | 构成脚本分支和批处理的基本控制流 |
| 函数与参数处理 | `function` / `local`、返回值、`getopts`、长选项解析 | 强化脚本复用性和命令行接口设计 |
| 输入输出与组合 | 重定向、管道、`PIPESTATUS`、Here Document、`read` | 体现 Unix 风格的命令组合和流式处理 |
| 算术与实用技巧 | `(( ))`、`let`、`expr`、`bc`、`mktemp`、并行执行 | 覆盖轻量自动化任务中的常见构件 |
| 安全与调试 | `set -euo pipefail`、`trap`、`bash -n`、`bash -x` | 强调脚本可靠性、清理逻辑和问题定位能力 |

## Core Mental Model Implied by the Source

这份来源虽然是语法速查，但隐含了一套很实用的 Bash 学习与写作顺序：

1. 先掌握变量、特殊参数和参数展开，理解脚本如何接收输入和生成输出。
2. 再掌握 `if`、循环、`case` 和函数，让脚本具备基本控制流。
3. 之后引入重定向、管道、Here Document 和 `read`，把脚本接入真实命令行工作流。
4. 最后补上 `getopts`、`set -euo pipefail`、`trap`、调试和临时文件等实践能力，让脚本更接近可长期复用的工具。

## Practical Patterns Highlighted by the Source

### Prefer Built-in Expansion Before External Text Processing

- 来源大量使用 `${VAR:-default}`、`${STR//x/y}`、`${#STR}`、`${arr[@]:1:3}` 等写法，说明 Bash 内建展开机制是脚本文本处理的第一层工具。
- `sed` 单引号转义示例也提醒：一旦转向外部工具，引用和转义复杂度会明显上升。

### Treat Scripts as Small CLIs

- `getopts`、长选项解析、`shift $((OPTIND-1))` 和剩余参数处理，说明来源把脚本视为带接口契约的小型命令行程序，而不只是一次性命令集合。
- 对 `Usage`、未知选项、帮助参数的处理，为后续文档化提供了明确入口。

### Reliability Comes From Guard Rails, Not Just Happy Paths

- `set -euo pipefail`、`trap ERR`、`trap EXIT`、`mktemp` 清理，构成了来源里的“安全脚本”基线。
- `bash -n` 和 `bash -x` 则对应语法验证和执行跟踪两类基本调试手段。

### Automation Is Mostly Orchestration

- 获取脚本绝对路径、检查命令是否存在、后台并行、xargs 并行、Git 快速推送脚本等示例，说明来源中的 Bash 重点不是复杂算法，而是围绕文件、命令和仓库操作做编排。

## Documentation Implications

- 文档应区分 Bash 内建能力与外部命令能力，例如 `getopts`、参数展开属于 Bash，而 `bc`、`git`、`curl`、`xargs` 依赖系统工具。
- 对 `关联数组（Bash 4+）`、`readlink -f`、`sed -i` 这类写法，应额外标明版本或平台差异，避免把 GNU/Linux 行为误写成通用 shell 事实。
- 如果把来源改写成教程，建议按“变量与展开 -> 控制流 -> 函数与参数 -> I/O -> 安全与调试”的层级组织，而不是只按语法类别平铺。
- `cd .. && git add . && git commit ...` 这类便利脚本适合作为示例，但正式文档应同步说明作用范围和仓库风险。

## Known Gaps from This Source

- 没有区分 POSIX `sh`、`Bash` 与其他 shell（如 `zsh`）之间的兼容边界。
- 没有系统解释引用、单词分割、glob 展开和 `IFS` 带来的常见脚本陷阱。
- 没有覆盖子 shell、进程替换、命令替换嵌套和高级 FD 管理等进阶主题。
- 没有提到 ShellCheck、测试框架或 CI 中的脚本验证实践。

## Related Pages

- [[bash]]
- [[shell-scripting]]
- [[glossary]]
- [[overview]]
- [[vim]]
