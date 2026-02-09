---
layout: post
title: debian设置samba匿名免密共享
tags: [linux]
---

samba是局域网共享文件的重要方式，本文介绍如何在debian10中建立免密访问的局域网文件夹。

### 安装

```shell
apt -y install samba
```
### 配置防火墙
```shell
ufw allow from 192.168.1.0/24 to any app Samba
```

### 配置

建立共享文件夹

```shell
mkdir /home/share
chmod 777 /home/share
```

修改配置文件（/etc/samba/smb.conf）

```
# line 25: add

unix charset = UTF-8
# line 30: change (Windows' default)

workgroup = WORKGROUP
# line 37: uncomment and change IP address you allow

interfaces = 127.0.0.0/8 10.0.0.0/24
# line 58: uncomment and add

bind interfaces only = yes
map to guest = Bad User
# add to the end

# any share name you like

[movie]
    # shared directory
    path = /home/sb/movie
    # writable
    writable = yes
    browseable = yes
    public = yes
    # guest OK
    guest ok = yes
    # guest only
    guest only = yes
    # fully accessed
    create mode = 0777
    # fully accessed
    directory mode = 0777
```

### 运行

```shell
systemctl restart smbd 
```
### linux访问
````
sudo apt install cifs-utils
smbclient -L 192.168.1.5
sudo mount -t cifs //192.168.1.21/movie movie -o username=sb

```
### 参考

- [server-world](https://www.server-world.info/en/note?os=Debian_10&p=samba&f=1)