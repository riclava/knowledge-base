# 基于docker构建ubuntu20.04开发环境

## 启动脚本

```bash
#!/bin/bash

docker run -d \
	--name=ubuntu20 \
	--restart=unless-stopped \
	ubuntu:20.04 \
	sleep infinity
```

## 初始化脚本

```bash
sed -i 's@//.*archive.ubuntu.com@//mirrors.ustc.edu.cn@g' /etc/apt/sources.list
sed -i 's/security.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list

apt-get -y update && apt-get -y dist-upgrade
apt -y install tzdata vim curl wget git

```
