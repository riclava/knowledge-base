# vim

## 常用配置 (~/.vimrc)

### 基础配置

```vim
" 显示行号
set number
" 显示相对行号
set relativenumber

" 语法高亮
syntax on

" 搜索高亮
set hlsearch
" 增量搜索（边输入边搜索）
set incsearch
" 搜索忽略大小写
set ignorecase
" 智能大小写（有大写时区分大小写）
set smartcase

" 显示当前行
set cursorline

" 显示匹配的括号
set showmatch

" 编码设置
set encoding=utf-8
set fileencoding=utf-8

" 显示状态栏
set laststatus=2
" 显示当前模式
set showmode
" 显示命令
set showcmd

" 启用鼠标
set mouse=a

" 历史记录数
set history=1000

" 禁用备份文件
set nobackup
set noswapfile

" 允许隐藏未保存的 buffer
set hidden
```

### 缩进配置

```vim
" 禁止自动缩进
set noautoindent

" 启用自动缩进
set autoindent
" 智能缩进
set smartindent

" Tab 设置
set tabstop=4       " Tab 显示宽度
set shiftwidth=4    " 自动缩进宽度
set softtabstop=4   " 编辑时 Tab 宽度
set expandtab       " 将 Tab 转换为空格

" 针对不同文件类型设置缩进
autocmd FileType python setlocal tabstop=4 shiftwidth=4
autocmd FileType javascript,typescript,json,yaml setlocal tabstop=2 shiftwidth=2
autocmd FileType go setlocal noexpandtab tabstop=4 shiftwidth=4
```

### 外观配置

```vim
" 配色方案
colorscheme desert

" 设置背景色
set background=dark

" 显示空白字符
set list
set listchars=tab:▸\ ,trail:·,extends:>,precedes:<

" 折行设置
set wrap           " 自动折行
set linebreak      " 在单词边界折行
```

### 粘贴模式

```vim
" 粘贴时保持格式（避免自动缩进）
set pastetoggle=<F2>
```

---

## 常用操作技巧

### 模式切换

| 按键 | 说明 |
|------|------|
| `i` | 在光标前插入 |
| `I` | 在行首插入 |
| `a` | 在光标后插入 |
| `A` | 在行尾插入 |
| `o` | 在下方新建一行并插入 |
| `O` | 在上方新建一行并插入 |
| `Esc` | 返回普通模式 |
| `v` | 可视模式（字符选择） |
| `V` | 可视模式（行选择） |
| `Ctrl+v` | 可视模式（块选择） |

### 光标移动

| 按键 | 说明 |
|------|------|
| `h/j/k/l` | 左/下/上/右 |
| `w` | 下一个单词开头 |
| `b` | 上一个单词开头 |
| `e` | 当前/下一个单词结尾 |
| `0` | 行首 |
| `^` | 行首（非空白字符） |
| `$` | 行尾 |
| `gg` | 文件开头 |
| `G` | 文件结尾 |
| `nG` 或 `:n` | 跳转到第 n 行 |
| `Ctrl+f` | 向下翻页 |
| `Ctrl+b` | 向上翻页 |
| `Ctrl+d` | 向下翻半页 |
| `Ctrl+u` | 向上翻半页 |
| `%` | 跳转到匹配的括号 |

### 编辑操作

| 按键 | 说明 |
|------|------|
| `x` | 删除当前字符 |
| `dd` | 删除当前行 |
| `dw` | 删除到下一个单词 |
| `d$` 或 `D` | 删除到行尾 |
| `d0` | 删除到行首 |
| `yy` | 复制当前行 |
| `yw` | 复制一个单词 |
| `p` | 在光标后粘贴 |
| `P` | 在光标前粘贴 |
| `u` | 撤销 |
| `Ctrl+r` | 重做 |
| `cc` | 删除当前行并进入插入模式 |
| `cw` | 删除单词并进入插入模式 |
| `r` | 替换当前字符 |
| `R` | 进入替换模式 |
| `.` | 重复上一次操作 |
| `>>` | 向右缩进 |
| `<<` | 向左缩进 |
| `J` | 合并当前行和下一行 |

### 搜索与替换

```vim
" 搜索
/pattern          " 向下搜索
?pattern          " 向上搜索
n                 " 下一个匹配
N                 " 上一个匹配
*                 " 搜索光标下的单词（向下）
#                 " 搜索光标下的单词（向上）

" 取消搜索高亮
:noh

" 替换
:s/old/new        " 替换当前行第一个匹配
:s/old/new/g      " 替换当前行所有匹配
:%s/old/new/g     " 替换全文所有匹配
:%s/old/new/gc    " 替换全文所有匹配（逐个确认）
:10,20s/old/new/g " 替换第 10-20 行
```

### 文件操作

```vim
:w                " 保存
:w filename       " 另存为
:q                " 退出
:q!               " 强制退出（不保存）
:wq 或 :x 或 ZZ   " 保存并退出
:e filename       " 打开文件
:e!               " 重新加载当前文件（放弃修改）
:bn               " 下一个 buffer
:bp               " 上一个 buffer
:bd               " 关闭当前 buffer
:ls               " 列出所有 buffer
```

### 多窗口操作

```vim
:sp filename      " 水平分割窗口
:vsp filename     " 垂直分割窗口
Ctrl+w h/j/k/l    " 切换窗口
Ctrl+w w          " 循环切换窗口
Ctrl+w =          " 使所有窗口等宽等高
Ctrl+w +/-        " 增加/减少窗口高度
Ctrl+w >/<        " 增加/减少窗口宽度
:close            " 关闭当前窗口
:only             " 只保留当前窗口
```

### 多标签页

```vim
:tabnew filename  " 新建标签页
:tabn 或 gt       " 下一个标签页
:tabp 或 gT       " 上一个标签页
:tabclose         " 关闭当前标签页
:tabs             " 列出所有标签页
```

### 代码折叠

```vim
" 配置
set foldmethod=indent  " 按缩进折叠
set foldmethod=syntax  " 按语法折叠
set foldmethod=marker  " 按标记折叠

" 操作
za                " 切换折叠状态
zc                " 关闭折叠
zo                " 打开折叠
zM                " 关闭所有折叠
zR                " 打开所有折叠
```

### 宏录制

```vim
qa                " 开始录制宏到寄存器 a
q                 " 停止录制
@a                " 执行宏 a
@@                " 重复执行上一个宏
10@a              " 执行宏 a 10 次
```

### 实用技巧

```vim
" 删除所有空行
:g/^$/d

" 删除行尾空格
:%s/\s\+$//g

" 在每行末尾添加内容
:%s/$/;/g

" 在每行开头添加内容
:%s/^/# /g

" 排序
:sort             " 升序排序
:sort!            " 降序排序
:sort u           " 排序并去重

" 显示当前文件信息
Ctrl+g

" 查看文件编码
:set fileencoding?

" 转换文件编码
:set fileencoding=utf-8

" 转换换行符格式
:set fileformat=unix
:set fileformat=dos

" 执行 shell 命令
:!command

" 将命令输出插入到文件
:r !command

" 比较两个文件
vim -d file1 file2
" 或在 vim 中
:diffsplit file2
```

### 常用组合操作

```vim
" 删除引号内的内容
di"               " 删除双引号内
di'               " 删除单引号内
di(               " 删除括号内
di{               " 删除花括号内
di[               " 删除方括号内

" 修改引号内的内容（删除并进入插入模式）
ci"
ci'
ci(

" 选中引号内的内容
vi"
vi'
vi(

" 删除/修改/选中包含引号本身
da"
ca"
va"

" 删除到某个字符
dt,               " 删除到逗号前
df,               " 删除到逗号（包含逗号）

" 快速注释（可视模式下）
Ctrl+v            " 进入块选择模式
选择多行
I                 " 在块前插入
# 或 //           " 输入注释符
Esc               " 应用到所有行
```

---

## 插件管理

### vim-plug 安装

```bash
# 安装 vim-plug
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

### 插件配置示例

```vim
" ~/.vimrc
call plug#begin('~/.vim/plugged')

" 文件树
Plug 'preservim/nerdtree'

" 模糊搜索
Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
Plug 'junegunn/fzf.vim'

" 状态栏
Plug 'vim-airline/vim-airline'

" Git 集成
Plug 'tpope/vim-fugitive'

" 代码补全
Plug 'neoclide/coc.nvim', {'branch': 'release'}

" 注释
Plug 'tpope/vim-commentary'

" 括号配对
Plug 'jiangmiao/auto-pairs'

" 主题
Plug 'morhetz/gruvbox'

call plug#end()

" 安装插件：:PlugInstall
" 更新插件：:PlugUpdate
" 删除插件：:PlugClean
```

### 常用插件快捷键

```vim
" NERDTree
:NERDTreeToggle   " 切换文件树
map <C-n> :NERDTreeToggle<CR>

" fzf
:Files            " 搜索文件
:Rg               " 搜索内容
:Buffers          " 搜索 buffer

" vim-commentary
gcc               " 注释/取消注释当前行
gc                " 可视模式下注释选中内容
```
