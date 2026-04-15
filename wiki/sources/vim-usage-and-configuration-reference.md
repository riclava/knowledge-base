---
title: Vim 常用配置与操作参考
type: source
created: 2026-04-15
updated: 2026-04-15
sources: [vim.md]
tags: [vim, linux, terminal-editor, configuration, cheat-sheet, source]
---

这是一份面向日常 Linux/终端工作流的 Vim 速查与配置参考，覆盖 `~/.vimrc` 常用设置、核心编辑命令、组合操作和插件管理。

## Source Metadata

- Raw source: `raw/OS/Linux/Common/vim.md`
- 文档性质：工具使用笔记 / 速查参考
- 主要对象：Vim、`~/.vimrc`、常用编辑命令、`vim-plug`
- 适用场景：终端编辑、代码修改、配置文件维护、批量文本处理

## What the Source Covers

| 模块 | 覆盖内容 | 价值 |
|---|---|---|
| 基础配置 | 行号、搜索、编码、状态栏、鼠标、buffer 行为 | 提升可读性与会话体验 |
| 缩进配置 | `autoindent`、`smartindent`、`expandtab`、按文件类型差异化缩进 | 让编辑行为贴近不同语言规范 |
| 外观与粘贴 | 配色、空白字符显示、折行、`pastetoggle` | 减少格式化误伤和隐藏字符问题 |
| 核心操作 | 模式切换、光标移动、删除复制粘贴、撤销重做 | 构成日常编辑基本盘 |
| 文本处理 | 搜索替换、排序、批量增删前后缀、去空行/去尾空格 | 适合快速批量处理文本 |
| 工作区管理 | buffer、窗口、标签页、折叠、diff | 适合多文件并行查看和修改 |
| 自动化 | 宏录制、可视块编辑、组合命令 | 适合重复性修改 |
| 扩展生态 | `vim-plug` 和常见插件示例 | 说明 Vim 可以从纯编辑器扩展到工作流中枢 |

## Configuration Themes

### Readability and Navigation

- 用 `number` + `relativenumber` 同时支持绝对定位和相对跳转。
- 用 `cursorline`、`showmatch`、`syntax on` 和空白字符显示强化视觉反馈。
- 用 `laststatus=2`、`showmode`、`showcmd` 提升状态可见性。

### Search and Text Manipulation

- 搜索行为以 `hlsearch`、`incsearch`、`ignorecase`、`smartcase` 组合为主，强调“边搜边看”和大小写智能判断。
- 替换示例覆盖当前行、全文件、指定范围和逐个确认四种常用模式。
- 批量文本处理示例强调 Vim 不只是逐字符编辑器，也是面向结构化重复修改的命令式工具。

### Editing Defaults and File-Type Specific Rules

- 默认缩进方案使用 `autoindent`、`smartindent`、`tabstop`、`shiftwidth`、`softtabstop`、`expandtab`。
- 通过 `autocmd FileType ... setlocal ...` 为 Python、JavaScript/TypeScript/JSON/YAML、Go 设置差异化缩进。
- `pastetoggle=<F2>` 说明来源特别关注“临时粘贴原始文本时不要破坏格式”这一常见问题。

### Session Behavior

- `hidden` 支持在未保存的 buffer 间切换，适合多文件来回修改。
- `mouse=a`、`history=1000` 偏向实用主义配置，而不是极简纯键盘主义。
- `nobackup` 与 `noswapfile` 被直接关闭，说明来源更偏好干净目录和轻量体验，但没有展开恢复与安全权衡。

## Core Mental Model Implied by the Source

这份来源虽然是速查表，但隐含了一套清晰的 Vim 学习顺序：

1. 先理解 `普通模式 / 插入模式 / 可视模式` 的模态切换。
2. 再掌握移动、删除、复制、粘贴、搜索这些高频基本功。
3. 之后进入 `di"`、`ci(`、`dt,` 这类“操作符 + 文本对象/目标”的组合式编辑。
4. 最后扩展到宏、窗口、标签页、diff 和插件，把 Vim 变成完整工作流工具。

## Plugin Strategy in This Source

### Plugin Manager

- 来源选择 `vim-plug` 作为插件管理器，安装方式简单，适合个人环境快速引入扩展。

### Sample Plugin Roles

| 插件 | 角色 |
|---|---|
| `preservim/nerdtree` | 文件树导航 |
| `junegunn/fzf` / `fzf.vim` | 模糊搜索文件、内容、buffer |
| `vim-airline` | 状态栏增强 |
| `vim-fugitive` | Git 集成 |
| `coc.nvim` | 代码补全 |
| `vim-commentary` | 注释切换 |
| `auto-pairs` | 括号自动配对 |
| `gruvbox` | 主题外观 |

### Practical Implication

- 来源把 Vim 视为一个可以逐步堆叠能力的工作台，而不是只靠内置命令的极简编辑器。
- 插件清单优先补齐导航、搜索、Git、补全和注释，基本对应代码编辑最常见的效率瓶颈。

## Documentation Implications

- 适合把 Vim 文档拆成“基础模式”“组合命令”“多文件工作流”“插件扩展”四个学习层级。
- 需要在文档里明确哪些能力是 Vim 内置，哪些依赖插件，否则用户容易把 `:Files`、`gcc` 等命令误认为默认功能。
- 对 `nobackup`、`noswapfile`、`mouse=a` 这类偏好型设置，文档应同时说明适用场景与取舍。
- 如果后续补充 Neovim 资料，应单独标出兼容项与差异项，而不是默认视为完全等同。

## Known Gaps from This Source

- 没有区分 Vim 版本、终端版/GUI 版或 Neovim 差异。
- 没有解释 `operator + motion + text object` 的语法原理，只给了例子。
- 没有讨论 swap/backup 关闭后的风险控制和恢复策略。
- 没有提供插件冲突、启动性能或最小化配置建议。

## Related Pages

- [[vim]]
- [[modal-editing]]
- [[glossary]]
- [[overview]]
