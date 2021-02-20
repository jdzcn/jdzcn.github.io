---
layout: post
title: sshfs挂载远程目录
tags: [linux]
---

在使用VPS过程中，我们可以用SSHFS把远程目录挂载到本地使用。
### 安装
```shell
sudo apt install sshfs
```
### 使用
默认挂载
```shell
sshfs user@host:remotedir localdir
```
根据需要使用参数挂载
```shell
-o allow_other,default_permissions,IdentityFile=~/.ssh/id_rsa
```
### 参考
- [How To](https://www.digitalocean.com/community/tutorials/how-to-use-sshfs-to-mount-remote-file-systems-over-ssh)
- [arch wiki](https://wiki.archlinux.org/index.php/SSHFS)
- [gitub](https://github.com/libfuse/sshfs)
