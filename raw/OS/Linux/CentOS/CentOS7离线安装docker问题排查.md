# 【问题排查】CentOS7离线安装docker问题排查

## OS

> [root@ecs-f446-0322459 lib]# uname -a
Linux ecs-f446-0322459 3.10.0-123.el7.x86_64 #1 SMP Mon Jun 30 12:09:22 UTC 2014 x86_64 x86_64 x86_64 GNU/Linux

## docker version

> [root@ecs-f446-0322459 lib]# docker --version
Docker version 20.10.8, build 3967b7d

## install steps

> yum downloadonly from another centos 7 server

## remove bridge/interface/link

```bash
ifconfig docker0 down
ifconfig br-xxxx down
ifconfig vethxxxx down

brctl delbr docker0
brctl delbr br-xxxx
ip link del vethxxxx
```

## Flask iptables

```bash
iptables -F
iptables -t nat -F
```

## Test

```bash
systemctl restart docker
docker run -ti --rm -p 8080:80 nginx:1.18.0
# on another term
curl http://localhost:8080
# 报错 Reset
```

## 排查问题

+ 卸载 docker
+ 清空 iptables
+ 删除 /var/lib/docker
+ 删除网桥，网卡
+ 重装 docker
+ 问题依然存在

## 继续排查

+ 由于是网络问题，对比另外一个正常的 CentOS7 环境，启动 nginx 之后的 ip a 情况

+ 正常

```bash
[root@localhost ~]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: ens33: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:0c:29:e9:31:27 brd ff:ff:ff:ff:ff:ff
    inet 192.168.140.129/24 brd 192.168.140.255 scope global noprefixroute dynamic ens33
       valid_lft 1449sec preferred_lft 1449sec
    inet6 fe80::b23d:c59e:7907:eb51/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:39:46:84:49 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:39ff:fe46:8449/64 scope link 
       valid_lft forever preferred_lft forever
25: vethefb71da@if24: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default 
    link/ether f6:fd:e9:7c:28:87 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet6 fe80::f4fd:e9ff:fe7c:2887/64 scope link 
       valid_lft forever preferred_lft forever
```

+ 异常

```bash
[root@ecs-f446-0322459 hpc]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default 
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether fa:16:3e:1a:3f:61 brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.137/24 brd 192.168.0.255 scope global noprefixroute dynamic eth0
       valid_lft 81863sec preferred_lft 81863sec
    inet6 fe80::f816:3eff:fe1a:3f61/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
44: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:bc:df:fc:01 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:bcff:fedf:fc01/64 scope link 
       valid_lft forever preferred_lft forever
52: veth8058b16: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default 
    link/ether 9e:f4:0f:5d:82:6f brd ff:ff:ff:ff:ff:ff
    inet6 fe80::9cf4:fff:fe5d:826f/64 scope link 
       valid_lft forever preferred_lft forever
54: vethadc9346: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default 
    link/ether 6e:3d:be:4c:1a:69 brd ff:ff:ff:ff:ff:ff
    inet6 fe80::6c3d:beff:fe4c:1a69/64 scope link 
       valid_lft forever preferred_lft forever
```

+ 对比发现，异常部分，没有 link-netnsid
+ `ip netns list-id` 发现报 `RTM_GETNSID is not supported by the kernel.`
+ 升级内核，重新执行 `ip netns list-id` 不再报异常

```bash
[root@ecs-f446-0322459 hpc]# uname -a
Linux ecs-f446-0322459.novalocal 3.10.0-1160.36.2.el7.x86_64 #1 SMP Wed Jul 21 11:57:15 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
[root@ecs-f446-0322459 hpc]# ip netns list-id
nsid 0 
[root@ecs-f446-0322459 hpc]# ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether fa:16:3e:1a:3f:61 brd ff:ff:ff:ff:ff:ff
    inet 192.168.0.137/24 brd 192.168.0.255 scope global noprefixroute dynamic eth0
       valid_lft 73212sec preferred_lft 73212sec
    inet6 fe80::f816:3eff:fe1a:3f61/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:30:d2:f7:de brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:30ff:fed2:f7de/64 scope link 
       valid_lft forever preferred_lft forever
5: vethd0b1d1d@if4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default 
    link/ether 3e:72:2e:fe:a1:68 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet6 fe80::3c72:2eff:fefe:a168/64 scope link 
       valid_lft forever preferred_lft forever
[root@ecs-f446-0322459 hpc]# 
```
