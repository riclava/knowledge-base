# CentOS7-samba-映射Linux磁盘到Windows

## steps

+ install

```bash
yum -y install samba
```

+ prepare
disable firewalld, iptables, selinux

+ vim /etc/samba/smb.conf , replace Ricl to your name

```conf
[global]
         workgroup = WORKGROUP
         server string = Ricl Samba Server %v
         netbios name = RiclSamba
         security = user
         map to guest = Bad User
         passdb backend = tdbsam

[data]
         comment = project development directory
         path = /data
         valid users = ricl
         write list = ricl
         printable = no
         create mask = 0644
         directory mask = 0755
```

+ vim /etc/security/limits.conf

```conf
* soft nofile 65535
* hard nofile 65535
```

+ config

```bash
useradd ricl
smbpasswd -a ricl

mkdir /data
chown -R ricl.ricl /data

systemctl enable smb --now
```

+ On windows

计算机 -> 右键网络 -> 映射网络驱动器 -> \\{{IP}}\data -> 完成
