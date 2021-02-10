---
layout: post
title: debian设置samba匿名免密共享
---

samba是局域网共享文件的重要方式，本文介绍如何在debian10中建立免密访问的局域网文件夹。

### 安装

```shell
apt -y install samba
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

[Share]
    # shared directory
    path = /home/share
    # writable
    writable = yes
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

### 参考

- [server-world](https://www.server-world.info/en/note?os=Debian_10&p=samba&f=1)