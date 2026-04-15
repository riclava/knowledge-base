# Bash 语法与脚本技巧

## 变量

### 基本用法

```bash
# 定义变量（等号两边不能有空格）
NAME="value"

# 使用变量
echo $NAME
echo ${NAME}

# 只读变量
readonly PI=3.14

# 删除变量
unset NAME
```

### 特殊变量

```bash
$0          # 脚本名称
$1 ~ $9     # 位置参数
$#          # 参数个数
$@          # 所有参数（作为独立字符串）
$*          # 所有参数（作为单个字符串）
$?          # 上一条命令的退出状态
$$          # 当前脚本的 PID
$!          # 最后一个后台进程的 PID
```

### 默认值

```bash
# 如果变量未定义，使用默认值
${VAR:-default}

# 如果变量未定义，设置并使用默认值
${VAR:=default}

# 如果变量未定义，报错退出
${VAR:?error message}

# 如果变量已定义，使用替代值
${VAR:+alternate}
```

---

## 字符串操作

### 字符串比较

```bash
S1="a"
S2="b"

if [ "$S1" = "$S2" ]; then
  echo "equal"
else
  echo "not equal"
fi

# 其他比较
[ "$S1" != "$S2" ]    # 不相等
[ -z "$S1" ]          # 为空
[ -n "$S1" ]          # 非空
```

### 字符串处理

```bash
STR="Hello World"

# 长度
echo ${#STR}          # 11

# 截取
echo ${STR:0:5}       # Hello（从位置 0 开始，取 5 个字符）
echo ${STR:6}         # World（从位置 6 到末尾）

# 替换
echo ${STR/World/Bash}    # Hello Bash（替换第一个）
echo ${STR//l/L}          # HeLLo WorLd（替换所有）

# 删除匹配
echo ${STR#Hello }    # World（从开头删除最短匹配）
echo ${STR##* }       # World（从开头删除最长匹配）
echo ${STR%World}     # Hello （从结尾删除最短匹配）
echo ${STR%%o*}       # Hell（从结尾删除最长匹配）
```

### 单引号转义

当使用 sed 替换文本时，比如要去掉单引号：

```bash
# 错误写法
sed -i 's/'//g' file.txt

# 正确写法：使用 '"'"' 来转义单引号
sed -i 's/'"'"'//g' file.txt
```

---

## 数组

### 基本操作

```bash
# 定义数组
arr=(a b c d e)
arr[0]="first"

# 访问元素
echo ${arr[0]}        # 第一个元素
echo ${arr[@]}        # 所有元素
echo ${arr[*]}        # 所有元素

# 数组长度
echo ${#arr[@]}

# 数组切片
echo ${arr[@]:1:3}    # 从索引 1 开始取 3 个

# 添加元素
arr+=(f g)

# 删除元素
unset arr[2]
```

### 遍历数组

```bash
arr=(a b c d)

# 遍历元素
for item in "${arr[@]}"; do
  echo "$item"
done

# 遍历索引
for i in "${!arr[@]}"; do
  echo "$i: ${arr[$i]}"
done
```

### 关联数组（Bash 4+）

```bash
declare -A map

map[name]="John"
map[age]=30

echo ${map[name]}
echo ${!map[@]}       # 所有键
echo ${map[@]}        # 所有值
```

---

## 条件判断

### if 语句

```bash
if [ condition ]; then
  # ...
elif [ condition ]; then
  # ...
else
  # ...
fi

# 使用 [[ ]] 支持更多特性
if [[ "$str" == *"pattern"* ]]; then
  echo "contains pattern"
fi
```

### 文件测试

```bash
[ -e file ]     # 存在
[ -f file ]     # 是普通文件
[ -d file ]     # 是目录
[ -r file ]     # 可读
[ -w file ]     # 可写
[ -x file ]     # 可执行
[ -s file ]     # 文件大小 > 0
[ -L file ]     # 是符号链接
[ file1 -nt file2 ]   # file1 比 file2 新
[ file1 -ot file2 ]   # file1 比 file2 旧
```

### 数值比较

```bash
[ $a -eq $b ]   # 等于
[ $a -ne $b ]   # 不等于
[ $a -gt $b ]   # 大于
[ $a -ge $b ]   # 大于等于
[ $a -lt $b ]   # 小于
[ $a -le $b ]   # 小于等于

# 使用 (( )) 进行算术比较
if (( a > b )); then
  echo "a > b"
fi
```

### 逻辑运算

```bash
# 在 [ ] 中
[ condition1 ] && [ condition2 ]
[ condition1 ] || [ condition2 ]
[ ! condition ]

# 在 [[ ]] 中
[[ condition1 && condition2 ]]
[[ condition1 || condition2 ]]
```

---

## 循环

### for 循环

```bash
# 列表遍历
for i in 1 2 3 4 5; do
  echo $i
done

# 范围遍历
for i in {1..10}; do
  echo $i
done

# 带步长
for i in {0..10..2}; do
  echo $i
done

# C 风格
for ((i=0; i<10; i++)); do
  echo $i
done

# 遍历文件
for file in *.txt; do
  echo "$file"
done
```

### while 循环

```bash
count=0
while [ $count -lt 5 ]; do
  echo $count
  ((count++))
done

# 读取文件
while IFS= read -r line; do
  echo "$line"
done < file.txt

# 无限循环
while true; do
  # ...
  sleep 1
done
```

### until 循环

```bash
count=0
until [ $count -ge 5 ]; do
  echo $count
  ((count++))
done
```

### 循环控制

```bash
# 跳出循环
break

# 跳过本次迭代
continue

# 跳出多层循环
break 2
```

---

## case 语句

```bash
case "$var" in
  pattern1)
    # ...
    ;;
  pattern2|pattern3)
    # ...
    ;;
  *)
    # 默认
    ;;
esac
```

### 判断操作系统

```bash
OS="UNKNOWN"
case "$OSTYPE" in
  solaris*) OS="SOLARIS";      echo $OS ;;
  darwin*)  OS="OSX";          echo $OS ;; 
  linux*)   OS="LINUX";        echo $OS ;;
  bsd*)     OS="BSD";          echo $OS ;;
  msys*)    OS="WINDOWS";      echo $OS ;;
  cygwin*)  OS="ALSO WINDOWS"; echo $OS ;;
  *)        OS="$OSTYPE";      echo $OS ;;
esac
```

---

## 函数

### 定义与调用

```bash
# 定义函数
function greet() {
  echo "Hello, $1!"
}

# 或者
greet() {
  echo "Hello, $1!"
}

# 调用
greet "World"
```

### 返回值

```bash
# 使用 return（只能返回 0-255 的整数）
is_valid() {
  if [ "$1" -gt 0 ]; then
    return 0    # 成功
  else
    return 1    # 失败
  fi
}

if is_valid 5; then
  echo "valid"
fi

# 使用 echo 返回字符串
get_name() {
  echo "John"
}

name=$(get_name)
```

### 局部变量

```bash
func() {
  local var="local value"
  echo $var
}
```

---

## 参数处理

### 基本参数

```bash
#!/bin/bash

echo "脚本名: $0"
echo "参数个数: $#"
echo "第一个参数: $1"
echo "所有参数: $@"
```

### getopts 处理选项

```bash
#!/bin/bash

while getopts "hf:v" opt; do
  case $opt in
    h)
      echo "Usage: $0 [-h] [-f file] [-v]"
      exit 0
      ;;
    f)
      FILE="$OPTARG"
      ;;
    v)
      VERBOSE=true
      ;;
    \?)
      echo "Invalid option: -$OPTARG"
      exit 1
      ;;
  esac
done

# 移除已处理的选项
shift $((OPTIND-1))

# 剩余参数
echo "Remaining args: $@"
```

### 长选项处理

```bash
#!/bin/bash

while [[ $# -gt 0 ]]; do
  case $1 in
    -h|--help)
      echo "Usage: $0 [options]"
      exit 0
      ;;
    -f|--file)
      FILE="$2"
      shift 2
      ;;
    -v|--verbose)
      VERBOSE=true
      shift
      ;;
    *)
      echo "Unknown option: $1"
      exit 1
      ;;
  esac
done
```

---

## 输入输出

### 重定向

```bash
# 标准输出重定向
command > file        # 覆盖
command >> file       # 追加

# 标准错误重定向
command 2> file
command 2>> file

# 同时重定向
command > file 2>&1
command &> file       # 简写

# 丢弃输出
command > /dev/null 2>&1
```

### 管道

```bash
command1 | command2 | command3

# 管道状态
echo ${PIPESTATUS[@]}
```

### Here Document

```bash
# 写入文件
cat <<EOF > /tmp/test.txt
line 1
line 2
EOF

# 追加到文件
cat <<EOF >> /tmp/test.txt
more content
EOF

# 禁用变量替换
cat <<'EOF'
$VAR will not be expanded
EOF

# 去除前导 Tab
cat <<-EOF
	indented content
EOF
```

### 读取输入

```bash
# 读取用户输入
read -p "Enter name: " name

# 读取密码（不显示）
read -sp "Enter password: " password

# 设置超时
read -t 5 -p "Quick! " answer

# 读取到数组
read -a arr <<< "a b c d"
```

---

## 算术运算

```bash
# 使用 $(( ))
result=$((1 + 2))
result=$((a * b))
((count++))
((count += 5))

# 使用 let
let result=1+2
let count++

# 使用 expr（旧式）
result=$(expr 1 + 2)

# 浮点运算（使用 bc）
result=$(echo "scale=2; 10/3" | bc)
```

---

## 常用技巧

### 获取脚本绝对路径

```bash
#!/bin/bash

SCRIPT=$(readlink -f "$0")
SCRIPT_PATH=$(dirname "$SCRIPT")

cd "$SCRIPT_PATH" || exit
```

### 同一目录下创建多个子目录

```bash
mkdir -p /data/{run,data,logs}
```

### 检查命令是否存在

```bash
if command -v docker &> /dev/null; then
  echo "docker is installed"
fi

# 或者
if ! type git &> /dev/null; then
  echo "git is not installed"
  exit 1
fi
```

### 错误处理

```bash
#!/bin/bash
set -e          # 遇到错误立即退出
set -u          # 使用未定义变量时报错
set -o pipefail # 管道中任一命令失败则整体失败

# 常用组合
set -euo pipefail

# 自定义错误处理
trap 'echo "Error on line $LINENO"; exit 1' ERR

# 清理函数
cleanup() {
  rm -f /tmp/tempfile
}
trap cleanup EXIT
```

### 调试脚本

```bash
# 显示执行的命令
set -x

# 关闭调试
set +x

# 只检查语法不执行
bash -n script.sh

# 调试模式运行
bash -x script.sh
```

### 快速推送代码脚本

```bash
#!/bin/bash

SCRIPT=$(readlink -f "$0")
SCRIPT_PATH=$(dirname "$SCRIPT")

cd "$SCRIPT_PATH" || exit

INFO=$1

if [ -z "$INFO" ]; then
  INFO="docs: misc"
fi

cd .. && git add . && git commit -m "$INFO" && git push
```

### 并行执行

```bash
# 后台并行
for i in {1..5}; do
  do_something $i &
done
wait    # 等待所有后台任务完成

# 使用 xargs 并行
cat urls.txt | xargs -P 4 -I {} curl -O {}
```

### 临时文件

```bash
# 创建临时文件
tmpfile=$(mktemp)
tmpdir=$(mktemp -d)

# 使用后清理
trap "rm -f $tmpfile" EXIT
```
