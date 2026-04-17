# macOS 常用命令

## 系统信息

### 硬件与系统

```bash
# 查看系统版本
sw_vers

# 查看详细系统信息
system_profiler SPSoftwareDataType

# 查看 CPU 信息
sysctl -n machdep.cpu.brand_string

# 查看内存大小
sysctl hw.memsize | awk '{print $2/1024/1024/1024 " GB"}'

# 查看硬件概览
system_profiler SPHardwareDataType

# 查看电池状态（笔记本）
pmset -g batt

# 查看运行时间
uptime
```

### 磁盘与存储

```bash
# 查看磁盘使用情况
df -h

# 查看目录大小
du -sh /path/to/directory

# 查看当前目录下各文件夹大小
du -sh */ | sort -hr

# 列出所有挂载的磁盘
diskutil list

# 查看磁盘信息
diskutil info /dev/disk0

# 弹出磁盘
diskutil eject /dev/disk2

# 安全清空回收站
rm -rf ~/.Trash/*
```

## 网络相关

### 查看监听端口

```bash
# 查看所有监听端口
lsof -i -P | grep LISTEN

# 使用 netstat 查看监听端口
netstat -an | grep LISTEN

# 查看特定端口（例如 8080）
lsof -i :8080

# 查看占用某端口的进程
lsof -i tcp:8080

# 杀掉占用端口的进程
kill -9 $(lsof -t -i:8080)
```

### 路由设置

```bash
# 添加路由
sudo route -nv add 202.202.43.0/24 172.23.23.1

# 删除路由
sudo route -n delete x.x.x.x/24 x.x.x.x

# 查看路由表
netstat -nr
```

### 网络诊断

```bash
# 查看网络接口信息
ifconfig

# 查看 Wi-Fi 信息
/System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport -I

# 扫描附近 Wi-Fi
/System/Library/PrivateFrameworks/Apple80211.framework/Versions/Current/Resources/airport -s

# 查看 DNS 配置
scutil --dns

# 刷新 DNS 缓存
sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder

# 测试网络连通性
ping -c 5 google.com

# 追踪路由
traceroute google.com

# 查看网络统计
netstat -s

# 查看当前网络连接
netstat -an | grep ESTABLISHED

# 查看本机 IP 地址
ipconfig getifaddr en0

# 查看公网 IP
curl ifconfig.me
```

### 防火墙

```bash
# 查看防火墙状态
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --getglobalstate

# 开启防火墙
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setglobalstate on

# 关闭防火墙
sudo /usr/libexec/ApplicationFirewall/socketfilterfw --setglobalstate off
```

## 进程管理

### 查看进程

```bash
# 查看所有进程
ps aux

# 查看进程树
pstree

# 实时查看进程（类似 top）
top

# 更友好的进程查看（需安装 htop）
htop

# 查找特定进程
ps aux | grep nginx

# 查看进程详细信息
ps -p <PID> -o pid,ppid,user,%cpu,%mem,start,time,command
```

### 管理进程

```bash
# 杀掉进程
kill <PID>

# 强制杀掉进程
kill -9 <PID>

# 按名称杀掉进程
killall <process_name>

# 按名称杀掉进程（更安全）
pkill -f <pattern>

# 后台运行命令
nohup command &

# 查看后台任务
jobs

# 将后台任务调到前台
fg %1
```

## 文件操作

### 文件查找

```bash
# 按名称查找文件
find /path -name "*.txt"

# 按大小查找文件（大于 100MB）
find /path -size +100M

# 按修改时间查找（7 天内修改）
find /path -mtime -7

# 使用 mdfind（Spotlight 搜索）
mdfind -name "filename"

# 搜索文件内容
mdfind "search text"

# 在指定目录搜索
mdfind -onlyin ~/Documents "keyword"
```

### 文件属性

```bash
# 查看文件详细信息
stat filename

# 查看文件类型
file filename

# 查看文件 MD5
md5 filename

# 查看文件 SHA256
shasum -a 256 filename

# 查看扩展属性
xattr filename

# 删除扩展属性（如隔离属性）
xattr -d com.apple.quarantine filename

# 递归删除 .DS_Store
find . -name ".DS_Store" -delete
```

### 压缩解压

```bash
# 创建 zip
zip -r archive.zip folder/

# 解压 zip
unzip archive.zip

# 创建 tar.gz
tar -czvf archive.tar.gz folder/

# 解压 tar.gz
tar -xzvf archive.tar.gz

# 查看压缩包内容
tar -tzvf archive.tar.gz
```

## 系统管理

### 服务管理（launchctl）

```bash
# 查看所有服务
launchctl list

# 查看特定服务
launchctl list | grep <service_name>

# 加载服务
launchctl load /Library/LaunchDaemons/com.example.plist

# 卸载服务
launchctl unload /Library/LaunchDaemons/com.example.plist

# 启动服务
launchctl start <service_label>

# 停止服务
launchctl stop <service_label>
```

### 电源管理

```bash
# 查看电源设置
pmset -g

# 阻止系统休眠（直到按 Ctrl+C）
caffeinate

# 阻止休眠指定时间（秒）
caffeinate -t 3600

# 阻止休眠直到某命令完成
caffeinate -s command

# 立即休眠
pmset sleepnow

# 定时关机（分钟）
sudo shutdown -h +60

# 取消定时关机
sudo shutdown -c

# 立即重启
sudo shutdown -r now
```

### 用户管理

```bash
# 查看当前用户
whoami

# 查看所有用户
dscl . list /Users

# 查看用户信息
dscl . read /Users/username

# 查看用户组
groups

# 切换用户
su - username
```

## 开发相关

### Homebrew

```bash
# 安装软件
brew install <package>

# 卸载软件
brew uninstall <package>

# 更新 Homebrew
brew update

# 升级所有软件
brew upgrade

# 查看已安装软件
brew list

# 搜索软件
brew search <keyword>

# 查看软件信息
brew info <package>

# 清理旧版本
brew cleanup

# 检查系统问题
brew doctor
```

### 开发工具

```bash
# 安装 Xcode 命令行工具
xcode-select --install

# 查看当前 Xcode 路径
xcode-select -p

# 切换 Xcode 版本
sudo xcode-select -s /Applications/Xcode.app

# 接受 Xcode 许可
sudo xcodebuild -license accept

# 重置 Xcode 命令行工具
sudo xcode-select --reset
```

### 环境变量

```bash
# 查看所有环境变量
env

# 查看特定环境变量
echo $PATH

# 临时设置环境变量
export MY_VAR="value"

# 永久设置（添加到 ~/.zshrc 或 ~/.bash_profile）
echo 'export MY_VAR="value"' >> ~/.zshrc

# 重新加载配置
source ~/.zshrc
```

## 实用技巧

### 剪贴板操作

```bash
# 复制文件内容到剪贴板
cat file.txt | pbcopy

# 复制命令输出到剪贴板
ls -la | pbcopy

# 从剪贴板粘贴
pbpaste

# 从剪贴板粘贴到文件
pbpaste > output.txt
```

### 截图

```bash
# 截取全屏
screencapture screenshot.png

# 截取选定区域
screencapture -i screenshot.png

# 截取窗口
screencapture -w screenshot.png

# 延迟截图（5 秒）
screencapture -T 5 screenshot.png

# 截图到剪贴板
screencapture -c
```

### 文本转语音

```bash
# 朗读文本
say "Hello World"

# 指定语音
say -v Samantha "Hello World"

# 查看可用语音
say -v ?

# 朗读文件
say -f file.txt

# 保存为音频文件
say -o output.aiff "Hello World"
```

### 打开应用与文件

```bash
# 用默认应用打开文件
open file.pdf

# 用指定应用打开
open -a "Visual Studio Code" file.txt

# 打开 Finder 到当前目录
open .

# 打开 URL
open https://www.apple.com

# 显示文件在 Finder 中的位置
open -R file.txt
```

### 系统偏好设置

```bash
# 打开系统偏好设置
open "x-apple.systempreferences:"

# 打开特定设置面板
open "x-apple.systempreferences:com.apple.preference.network"

# 显示隐藏文件
defaults write com.apple.finder AppleShowAllFiles YES; killall Finder

# 隐藏隐藏文件
defaults write com.apple.finder AppleShowAllFiles NO; killall Finder

# 显示文件扩展名
defaults write NSGlobalDomain AppleShowAllExtensions -bool true; killall Finder

# 截图保存位置
defaults write com.apple.screencapture location ~/Pictures/Screenshots; killall SystemUIServer

# 截图格式（png/jpg/pdf）
defaults write com.apple.screencapture type jpg; killall SystemUIServer

# 禁用截图阴影
defaults write com.apple.screencapture disable-shadow -bool true; killall SystemUIServer

# Dock 自动隐藏延迟
defaults write com.apple.dock autohide-delay -float 0; killall Dock

# 恢复默认 Dock 延迟
defaults delete com.apple.dock autohide-delay; killall Dock
```

### 其他实用命令

```bash
# 生成 UUID
uuidgen

# 查看日历
cal

# 查看当前日期时间
date

# 计算器
bc -l

# 查看命令历史
history

# 清空终端
clear

# 查看命令帮助
man <command>

# 查看命令位置
which <command>

# 查看命令类型
type <command>

# 创建别名
alias ll='ls -la'

# 查看所有别名
alias
```
