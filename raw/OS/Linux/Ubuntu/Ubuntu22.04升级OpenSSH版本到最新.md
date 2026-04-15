# Ubuntu22.04升级OpenSSH版本到最新

## 依赖

```bash
apt update
apt install -y build-essential wget gcc make
apt install -y zlib1g-dev libssl-dev libpam0g-dev libselinux1-dev
```

> 验证 PAM 开发包是否正确安装：

```bash
# 检查 PAM 头文件是否存在
ls -la /usr/include/security/pam_appl.h
```

## 找到最新版本的安装包

+ [OpenSSH](https://cdn.openbsd.org/pub/OpenBSD/OpenSSH/portable/)
+ 下载到服务器并解压

```bash
# 示例：下载最新版本
cd /tmp
wget https://cdn.openbsd.org/pub/OpenBSD/OpenSSH/portable/openssh-9.5p1.tar.gz
tar -xzf openssh-9.5p1.tar.gz
cd openssh-9.5p1
```

## 备份当前配置

```bash
# 备份配置文件
cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
```

> 注意，升级过程中不要断开当前终端连接，建议使用 screen 或 tmux，切记

## 停止 SSH 服务（先不要执行）

> **重要**：不要先停止 SSH 服务！保持当前连接，直接编译安装即可覆盖

## 编译

> 如果需要升级 openssl，可以参考相关文档，然后把下面的 `/usr` 替换为自定义的 openssl 安装路径（如 `/usr/local/openssl`）

+ configure

```bash
./configure \
  --prefix=/usr \
  --sysconfdir=/etc/ssh \
  --with-pam \
  --with-zlib \
  --with-ssl-dir=/usr \
  --without-hardening
```

> 注意：
> - `--with-md5-passwords` 和 `--with-tcp-wrappers` 选项在新版本中已被移除
> - Ubuntu 22.04 不再支持 TCP Wrappers

+ make & install

```bash
make -j$(nproc) && make install
```

## 配置

+ 保护私钥（确保权限正确）

```bash
chmod 600 /etc/ssh/ssh_host_rsa_key /etc/ssh/ssh_host_ecdsa_key /etc/ssh/ssh_host_ed25519_key
```

+ 配置文件（根据需求调整，系统默认的配置已经备份）

```bash
# 允许 root 登录（根据安全需求决定）
sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/g' /etc/ssh/sshd_config
# 允许密码认证
sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/g' /etc/ssh/sshd_config
```

> 注意：Ubuntu 22.04 系统已经自带了 systemd 服务文件，编译安装只是升级二进制文件，不需要重新配置服务

## 重启服务

```bash
# 重启 SSH 服务（Ubuntu 上服务名为 ssh）
systemctl restart ssh
# 确认服务已启用开机自启（系统默认已启用）
systemctl enable ssh
```

## 验证

+ 查看版本

```bash
ssh -V
```

+ 查看服务状态

```bash
systemctl status ssh
```

## 故障恢复

如果升级后无法连接，在当前终端执行：

```bash
# 恢复配置文件
cp /etc/ssh/sshd_config.bak /etc/ssh/sshd_config
# 重启服务
systemctl restart ssh
```
