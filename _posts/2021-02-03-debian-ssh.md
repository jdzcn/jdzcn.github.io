---
layout: post
title: debian10中的ssh常用设置
tags: [linux]
---

ssh是远程管理linux的重要方式，本文讨论ssh在debian10中的设置。

#### 安装

服务器

```shell
apt -y install openssh-server
systemctl restart ssh 
```

客户端

```shell
apt -y install openssh-client 
```

#### 配置

免密登录(git需复制~/.ssh/id_rsa.pub内容到git设置)

```shell
ssh-keygen
ssh-copy-id username@remote-server.org
```

google配置客户端登录

```shell
sudo -i
vi /etc/ssh/sshd_config
		PermitRootLogin yes
		PasswordAuthentication yes
service sshd restart
passwd root 
```
配置防火墙
```shell
ufw allow ssh
ufw allow from 192.168.1.0/24 to any port 22
```


#### 参考

- [server-world](https://www.server-world.info/en/note?os=Debian_10&p=ssh&f=1)

- [archlinux](https://wiki.archlinux.org/index.php/OpenSSH)