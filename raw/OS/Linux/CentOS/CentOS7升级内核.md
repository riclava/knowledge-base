# CentOS7升级内核

## 查看系统版本

`cat /etc/redhat-release`

## 查看当前内核版本

`uname -r`

## 检查是否安装ELRepo

`yum --disablerepo="*" --enablerepo="elrepo-kernel" list available`

### 安装/升级ELRepo

> <http://elrepo.org/tiki/HomePage>

+ 更新yum源仓库

`yum -y update`

+ 载入ELRepo仓库的公共密钥

`rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org`

+ 安装或升级ELRepo仓库的yum源

`yum install -y https://www.elrepo.org/elrepo-release-7.el7.elrepo.noarch.rpm`

OR

`rpm -Uvh https://www.elrepo.org/elrepo-release-7.el7.elrepo.noarch.rpm`

## 再次查看可用安装包

`yum --disablerepo="*" --enablerepo="elrepo-kernel" list available`

> 长期维护版本为lt，最新主线稳定版为ml

## 替换 elrepo 的源【可选】

<https://mirrors.tuna.tsinghua.edu.cn/help/elrepo/>

```ini
### Name: ELRepo.org Community Enterprise Linux Repository for el7
### URL: https://elrepo.org/

[elrepo]
name=ELRepo.org Community Enterprise Linux Repository - el7
baseurl=https://mirrors.tuna.tsinghua.edu.cn/elrepo/elrepo/el7/$basearch/
# mirrorlist=http://mirrors.elrepo.org/mirrors-elrepo.el7
enabled=1
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-elrepo.org
protect=0

[elrepo-testing]
name=ELRepo.org Community Enterprise Linux Testing Repository - el7
baseurl=https://mirrors.tuna.tsinghua.edu.cn/elrepo/testing/el7/$basearch/
# mirrorlist=http://mirrors.elrepo.org/mirrors-elrepo-testing.el7
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-elrepo.org
protect=0

[elrepo-kernel]
name=ELRepo.org Community Enterprise Linux Kernel Repository - el7
baseurl=https://mirrors.tuna.tsinghua.edu.cn/elrepo/kernel/el7/$basearch/
# mirrorlist=http://mirrors.elrepo.org/mirrors-elrepo-kernel.el7
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-elrepo.org
protect=0

[elrepo-extras]
name=ELRepo.org Community Enterprise Linux Extras Repository - el7
baseurl=https://mirrors.tuna.tsinghua.edu.cn/elrepo/extras/el7/$basearch/
# mirrorlist=http://mirrors.elrepo.org/mirrors-elrepo-extras.el7
enabled=0
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-elrepo.org
protect=0
```

## 安装最新的内核

`yum --enablerepo=elrepo-kernel install -y kernel-lt`

## 查看可用内核版本及启动顺序

`awk -F\' '$1=="menuentry " {print i++ " : " $2}' /boot/grub2/grub.cfg`

## 安装辅助工具（非必须，有些系统自带该工具）：grub2-pc

`yum install -y grub2-pc`

## 设置内核默认启动顺序

`grub2-set-default 0`

OR

`grub-set-default 'CentOS Linux (5.4.96-1.el7.elrepo.x86_64) 7 (Core)'`

> CentOS Linux (5.4.96-1.el7.elrepo.x86_64) 7 (Core)  这个是具体的版本

## 修改 /etc/default/grub

```ini
GRUB_TIMEOUT=1
GRUB_DISTRIBUTOR="$(sed 's, release .*$,,g' /etc/system-release)"
GRUB_DEFAULT=saved  #---将这里的saved修改成0----
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="crashkernel=auto spectre_v2=retpoline rhgb quiet net.ifnames=0 console=tty0 console=ttyS0,115200n8 noibrs"
GRUB_DISABLE_RECOVERY="true"
```

## 生成grub 配置文件

`grub2-mkconfig -o /boot/grub2/grub.cfg`

## 重启系统

`reboot`

> 重启完成后，查看内核版本是否正确 `uname -r`

## 查看系统中已安装的内核

`rpm -qa | grep kernel`

```log
kernel-devel-3.10.0-1160.11.1.el7.x86_64
kernel-tools-3.10.0-1160.11.1.el7.x86_64
kernel-3.10.0-1160.el7.x86_64
kernel-3.10.0-1160.11.1.el7.x86_64
kernel-lt-5.4.108-1.el7.elrepo.x86_64
kernel-tools-libs-3.10.0-1160.11.1.el7.x86_64
kernel-headers-3.10.0-1160.11.1.el7.x86_64
```

## 删除旧内核，这一步是【可选】的

`yum remove -y kernel-devel-3.10.0 kernel-3.10.0 kernel-headers-3.10.0`

## 查看已安装内核

`rpm -qa | grep kernel`

## 也可以安装 yum-utils 工具，当系统安装的内核大于3个时，会自动删除旧的内核版本

`yum install -y  yum-utils`

## 升级内核工具包

### 删除旧版本工具包--可选

`yum remove kernel-tools-libs.x86_64 kernel-tools.x86_64`

### 安装新版本工具包

`yum --disablerepo=\* --enablerepo=elrepo-kernel install -y kernel-lt-tools.x86_64`

## 查看当前已安装内核

`rpm -qa | grep kernel`
