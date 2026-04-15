# CentOS操作系统初始化流程

+ 0. 镜像获取 [ISO](http://umirrors.un-net.com/centos/7/isos/x86_64/)

+ 1. 安装
  + 1.1 选择 minimal 方式安装，尽量避免安装多余的套件
  + 1.2 磁盘空间配置，1. 如果是测试环境，点击现在配置磁盘 -> 自动创建 -> 删除掉 /home，直接将 / 分区设置为大于整个磁盘大小的值，即可自动将所有剩余空间分配给根分区； 2. 如果是其他环境，根分区依然可以如此配置，其他磁盘可以单独处理

+ 2. 账户配置
  + 2.1 root 密码配置必须满足复杂度，禁止弱密码，至少包含 大写字母、小写字母、数字、特殊字符 中的其中三种及其以上
  + 2.2 如无其他需求，不创建无关的账户

+ 3. IP地址配置，在 IaaS 环境下，必须提出申请，DNS服务器为202.202.32.33，搜索域为 cqupt.edu.cn
  + 3.1 域名配置，联系 曾宏、樊红

+ 4. SSH端口配置
  + 4.1 SSH 默认端口为 22，按照学校的要求，调整为 5122 以便 vpn 环境下可以访问，也可以同时支持 22/5122 两个端口（使用端口映射的方式）

    ```bash
    # 22 端口最简单方式映射
    iptables -t nat -A PREROUTING -p tcp --dport 5122 -j REDIRECT --to-ports 22
    iptables-save > /etc/sysconfig/iptables
    ```

  + 备注： 重邮 vpn 默认开通的端口为 80, 443, 5122, 8989，如果是 windows rdp 协议，走 8989 端口

+ 5. 镜像配置，以 CentOS 7 为例

  ```bash
  cd /etc/yum.repos.d
  rm -rf *.repo
  curl -O http://umirrors.un-net.com/centos-usage/CentOS7/CentOS-Base.repo
  yum makecache

  sudo yum install -y epel-release
  sudo sed -e 's!^mirrorlist=!#mirrorlist=!g' \
       -e 's!^#baseurl=!baseurl=!g' \
       -e 's!//download\.fedoraproject\.org/pub!//umirrors.un-net.com!g' \
       -e 's!http://umirrors\.un-net!http://umirrors.un-net!g' \
       -i /etc/yum.repos.d/epel.repo /etc/yum.repos.d/epel-testing.repo
  sudo sed 's/metalink/# metalink/g' -i /etc/yum.repos.d/epel.repo /etc/yum.repos.d/epel-testing.repo
  ```

+ 6. 时间同步配置，使用 ntp 或者 chrony，同步地址为 time.un-net.com 或者 202.202.43.209

  ```conf
  server time.un-net.com iburst
  ```

+ 7. 其他配置

  + selinux
  
    ```bash
    # 需要重启的方式
    sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
    # 无需重启，但是当前生效重启无效的方式
    setenforce 0
    # 可以两者结合，实现无需重启关闭，下次启动依然已经关闭
    ```

  + firewalld 如果不熟悉，可以关掉，使用 iptables

    ```bash
    systemctl disable firewalld
    systemctl stop firewalld
    ```

  + 常用工具

    ```bash
    yum -y install net-tools tcpdump vim telnet lrzsz unzip
    ```

  + OpenJDK 环境配置（如需要）

    ```bash
    # 安装 OpenJDK（注意安装 devel 包）
    yum install -y java-11-openjdk-devel
    
    # 设置 JAVA_HOME
    export JAVA_HOME=$(readlink -f /usr/bin/javac | sed "s:/bin/javac::")
    
    # 写入 profile 永久生效
    echo 'export JAVA_HOME=$(readlink -f /usr/bin/javac | sed "s:/bin/javac::")' >> /etc/profile
    ```
