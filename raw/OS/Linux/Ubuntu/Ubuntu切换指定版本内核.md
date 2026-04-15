# Ubuntu切换指定版本内核

## 1. 查看内核版本

```bash
uname -r
```

## 2. 查询期望安装的内核

```bash
apt search linux-image-5.15

# linux-image-5.15.0-117-generic
```

## 3. 安装

```bash
apt install -y linux-image-5.15.0-117-generic
apt install -y linux-headers-5.15.0-117-generic
apt install -y linux-modules-5.15.0-117-generic
apt install -y linux-modules-extra-5.15.0-117-generic

# 修复依赖
# apt --fix-broken install

# 查看安装结果
# dpkg -l | grep 5.15.0-117-generic
```

## 4. 切换内核

+ 查看

```bash
grep menuentry /boot/grub/grub.cfg
# 获得 Ubuntu, with Linux 5.15.0-117-generic
```

+ vim /etc/default/grub

```bash
# GRUB_DEFAULT=0 修改为如下，携带查看的结果
GRUB_DEFAULT="Advanced options for Ubuntu>Ubuntu, with Linux 5.15.0-117-generic"
```

+ 更新 grub

```bash
update-grub
```

+ 重启

+ 验证 `uname -r`