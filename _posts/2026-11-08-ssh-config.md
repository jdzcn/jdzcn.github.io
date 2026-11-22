---
layout: post
title: arch配置SSH主机简称
tags: [linux]
---


### 安装
``` 
sudo pacman -S openssh && sudo systemctl enable --now sshd
```

### 防火墙开启
```
ufw allow ssh
ufw allow from 192.168.1.0/24 to any port 22
sudo ufw allow from 192.168.1.0/24 to any port 22 proto tcp
```
### 生成密钥
```
ssh-keygen
ssh-copy-id username@remote-server.org
```
### 配置主机简称

```
Host mi
    HostName 192.168.1.53
    Port 8022
    User u0_a228
    IdentityFile ~/.ssh/id_ed25519

Host arch
    HostName 192.168.1.75
    Port 22
    User sb
    IdentityFile ~/.ssh/id_ed25519
```
