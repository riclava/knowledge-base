# Linux 常用命令

## 文件操作

### 复制文件/目录并保留权限

#### 方式一：cp

```bash
# 复制文件保留权限
cp -p source-file dest-file

# 复制目录保留权限（递归）
cp -rp source-dir/ dest-dir/
```

- `-p`：保留文件的模式、所有权和时间戳

#### 方式二：rsync

```bash
# 基本用法（-a 启用存档模式，保留权限和所有权）
rsync -a source-dir/ dest-dir

# 详细输出 + 人类可读格式 + 显示进度
rsync -avxP source-dir/ dest-dir

# 远程同步
rsync -avzP source-dir/ user@host:/path/to/dest/
```

### 删除乱码文件名的文件（inode 方式）

当文件名包含特殊字符无法直接删除时，可以通过 inode 删除：

```bash
# 查看文件的 inode 号
ls -i

# 通过 inode 删除文件
find . -inum 134441392 -delete
```

### 查找文件

```bash
# 按名称查找
find /path -name "*.log"

# 按大小查找（大于 100M）
find /path -size +100M

# 按修改时间查找（7 天内修改）
find /path -mtime -7

# 查找并执行命令
find /path -name "*.tmp" -exec rm {} \;

# 查找空目录
find /path -type d -empty
```

---

## 文本处理

### grep 文本搜索

```bash
# 递归搜索
grep -r "pattern" /path

# 显示行号
grep -n "pattern" file

# 忽略大小写
grep -i "pattern" file

# 反向匹配（不包含）
grep -v "pattern" file

# 只显示文件名
grep -l "pattern" *.txt

# 正则表达式
grep -E "pattern1|pattern2" file
```

### sed 文本替换

```bash
# 替换并直接修改文件
sed -i 's/old/new/g' file

# 删除空行
sed -i '/^$/d' file

# 删除包含特定内容的行
sed -i '/pattern/d' file

# 在第 n 行后插入
sed -i '3a\new line' file

# 替换第 n 行
sed -i '3c\replaced line' file
```

### awk 文本处理

```bash
# 打印指定列
awk '{print $1, $3}' file

# 指定分隔符
awk -F':' '{print $1}' /etc/passwd

# 条件过滤
awk '$3 > 100 {print $0}' file

# 统计行数
awk 'END {print NR}' file

# 求和
awk '{sum += $1} END {print sum}' file
```

### 其他文本工具

```bash
# 排序
sort file
sort -n file          # 数字排序
sort -r file          # 逆序
sort -u file          # 去重排序

# 去重（需先排序）
sort file | uniq
uniq -c file          # 统计重复次数

# 统计行数/单词数/字符数
wc -l file            # 行数
wc -w file            # 单词数
wc -c file            # 字节数

# 截取列
cut -d':' -f1 /etc/passwd

# 合并文件列
paste file1 file2

# 比较文件差异
diff file1 file2
diff -u file1 file2   # unified 格式
```

---

## 磁盘与存储

### 磁盘使用情况

```bash
# 查看磁盘使用
df -h

# 查看目录大小
du -sh /path
du -sh *              # 当前目录下各项大小
du -h --max-depth=1   # 只显示一级目录

# 查看最大的文件/目录
du -ah /path | sort -rh | head -20
```

### 使用 dd 测试磁盘读写速度

> 测试前先用 `df -h` 确认当前所在磁盘

#### 基础测试

```bash
# 写入测试
dd if=/dev/zero of=testfile bs=1M count=3000 conv=fdatasync

# 读取测试
dd if=testfile of=/dev/null bs=1M count=3000
```

#### 直接 I/O 测试（绕过缓存）

参数说明：
- `iflag=direct` / `oflag=direct`：使用直接 I/O 模式，绕过文件系统缓存
- `bs`：块大小
- `count`：块数量

```bash
# 大块写入测试
dd if=/dev/zero of=testfile bs=1G count=1 oflag=direct

# 小块写入测试
dd if=/dev/zero of=testfile bs=4k count=10000 oflag=direct

# 大块读取测试
dd if=testfile of=/dev/null bs=1G count=1 iflag=direct

# 小块读取测试
dd if=testfile of=/dev/null bs=4k count=10000 iflag=direct
```

---

## 进程管理

### 查看进程

```bash
# 查看所有进程
ps aux
ps -ef

# 按 CPU/内存排序
ps aux --sort=-%cpu | head
ps aux --sort=-%mem | head

# 实时监控
top
htop                  # 更友好的界面（需安装）

# 查看进程树
pstree
pstree -p             # 显示 PID
```

### 僵尸进程处理

```bash
# 查找僵尸进程
ps aux | grep Z

# 杀死僵尸进程（通过杀死父进程）
kill -9 $(ps -A -ostat,ppid | grep -e '[zZ]' | awk '{ print $2 }')
```

### 后台任务管理

```bash
# 后台运行
command &
nohup command &       # 忽略挂断信号
nohup command > output.log 2>&1 &

# 查看后台任务
jobs

# 前后台切换
fg %1                 # 切到前台
bg %1                 # 切到后台
Ctrl+Z                # 暂停当前任务
```

---

## 网络相关

### 网络状态

```bash
# 查看网络连接
netstat -tunlp
ss -tunlp             # 更快的替代

# 查看路由表
ip route
route -n

# 查看网卡信息
ip addr
ifconfig

# DNS 查询
nslookup domain.com
dig domain.com
```

### 网络测试

```bash
# 连通性测试
ping -c 4 host

# 端口测试
telnet host port
nc -zv host port

# 路由追踪
traceroute host
mtr host              # 更好的替代

# 下载文件
wget url
curl -O url
curl -o filename url
```

### 防火墙（firewalld）

```bash
# 查看状态
firewall-cmd --state
firewall-cmd --list-all

# 开放端口
firewall-cmd --permanent --add-port=8080/tcp
firewall-cmd --reload

# 关闭端口
firewall-cmd --permanent --remove-port=8080/tcp
firewall-cmd --reload
```

---

## 系统信息

```bash
# 系统版本
cat /etc/os-release
uname -a

# CPU 信息
lscpu
cat /proc/cpuinfo

# 内存信息
free -h
cat /proc/meminfo

# 硬件信息
lshw -short
dmidecode

# 系统负载
uptime
w

# 开机时间
who -b
```

---

## 用户与权限

### 用户管理

```bash
# 添加用户
useradd username
useradd -m -s /bin/bash username    # 创建家目录，指定 shell

# 设置密码
passwd username

# 删除用户
userdel username
userdel -r username   # 同时删除家目录

# 修改用户
usermod -aG groupname username      # 添加到组
usermod -s /bin/zsh username        # 修改 shell
```

### 权限管理

```bash
# 修改权限
chmod 755 file
chmod -R 755 dir      # 递归
chmod u+x file        # 给所有者添加执行权限

# 修改所有者
chown user:group file
chown -R user:group dir

# 特殊权限
chmod u+s file        # SUID
chmod g+s dir         # SGID
chmod +t dir          # Sticky bit
```

---

## 压缩与解压

```bash
# tar.gz
tar -czvf archive.tar.gz dir/       # 压缩
tar -xzvf archive.tar.gz            # 解压
tar -xzvf archive.tar.gz -C /path   # 解压到指定目录

# tar.bz2
tar -cjvf archive.tar.bz2 dir/
tar -xjvf archive.tar.bz2

# zip
zip -r archive.zip dir/
unzip archive.zip
unzip archive.zip -d /path

# 查看压缩包内容
tar -tzvf archive.tar.gz
unzip -l archive.zip
```

---

## 定时任务

### crontab

```bash
# 编辑定时任务
crontab -e

# 查看定时任务
crontab -l

# 删除所有定时任务
crontab -r
```

crontab 格式：
```
# 分 时 日 月 周 命令
# *  *  *  *  *  command

# 每天凌晨 2 点执行
0 2 * * * /path/to/script.sh

# 每 5 分钟执行
*/5 * * * * /path/to/script.sh

# 每周一 9 点执行
0 9 * * 1 /path/to/script.sh
```

---

## 日志查看

```bash
# 实时查看日志
tail -f /var/log/syslog
tail -f -n 100 logfile    # 显示最后 100 行并持续跟踪

# 查看系统日志（systemd）
journalctl
journalctl -u nginx       # 指定服务
journalctl -f             # 实时跟踪
journalctl --since "1 hour ago"

# 日志轮转
logrotate -f /etc/logrotate.conf
```
