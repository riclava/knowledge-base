# CentOS7 升级 OpenSSL 和 OpenSSH

本文档介绍如何在 CentOS7 上升级 OpenSSL 和 OpenSSH 到最新版本。

> **注意**：升级 OpenSSH 依赖 OpenSSL，如果需要同时升级两者，请先升级 OpenSSL。

## 1. 升级 OpenSSL

### 1.1 安装依赖

```bash
yum install gcc gcc-c++ autoconf automake zlib zlib-devel pcre-devel -y
```

### 1.2 下载源码

从 [OpenSSL 官网](https://www.openssl.org/source/) 下载最新版本，例如：

```bash
wget https://www.openssl.org/source/openssl-1.1.1l.tar.gz
tar -xzf openssl-1.1.1l.tar.gz
cd openssl-1.1.1l
```

### 1.3 编译安装

```bash
./config shared --openssldir=/usr/local/openssl --prefix=/usr/local/openssl
make -j4 && make install
```

### 1.4 配置系统

```bash
# 备份并替换旧版本
mv /usr/bin/openssl /tmp/
ln -s /usr/local/openssl/bin/openssl /usr/bin/openssl

# 配置动态链接库
echo "/usr/local/openssl/lib/" >> /etc/ld.so.conf
ldconfig
```

### 1.5 验证

```bash
openssl version
```

## 2. 升级 OpenSSH

### 2.1 安装依赖

```bash
yum install wget gcc -y
yum install -y zlib-devel openssl-devel
yum install pam-devel libselinux-devel zlib-devel openssl-devel -y
```

### 2.2 下载源码

从 [OpenSSH 官网](https://cdn.openbsd.org/pub/OpenBSD/OpenSSH/portable/) 下载最新版本，上传到服务器并解压。

### 2.3 删除旧版本

```bash
rpm -e --nodeps `rpm -qa | grep openssh`
```

> ⚠️ **警告**：执行此命令后，不要断开当前终端连接！

### 2.4 编译安装

根据是否升级了 OpenSSL，选择对应的 `--with-ssl-dir` 路径：
- 如果升级了 OpenSSL：使用 `/usr/local/openssl`
- 如果未升级 OpenSSL：使用 `/usr/local/ssl`

```bash
./configure \
  --prefix=/usr \
  --sysconfdir=/etc/ssh \
  --with-md5-passwords \
  --with-pam \
  --with-zlib \
  --with-tcp-wrappers \
  --with-ssl-dir=/usr/local/openssl \
  --without-hardening

make -j4 && make install
```

### 2.5 配置

```bash
# 保护私钥文件权限
chmod 600 /etc/ssh/ssh_host_rsa_key /etc/ssh/ssh_host_ecdsa_key /etc/ssh/ssh_host_ed25519_key

# 配置启动脚本和 PAM
cp -a contrib/redhat/sshd.init /etc/init.d/sshd
cp -a contrib/redhat/sshd.pam /etc/pam.d/sshd.pam
chmod u+x /etc/init.d/sshd

# 允许 root 登录和密码认证
sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config
sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/g' /etc/ssh/sshd_config
```

### 2.6 设置自启动

```bash
chkconfig --add sshd
chkconfig sshd on
systemctl restart sshd
```

### 2.7 验证

```bash
ssh -V
```

## 3. 常见问题

### 3.1 升级后无法连接

确保以下配置正确：
- `/etc/ssh/sshd_config` 中 `PermitRootLogin` 和 `PasswordAuthentication` 设置正确
- 私钥文件权限为 600
- 防火墙允许 SSH 端口

### 3.2 动态链接库问题

如果出现 OpenSSL 相关的库找不到错误，检查 `/etc/ld.so.conf` 配置并执行 `ldconfig`。
