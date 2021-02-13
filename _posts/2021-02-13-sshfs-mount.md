---
layout: post
title: sshfs挂载云目录
tags: [linux]
---

在使用VPS过程中，我们可以用SSHFS把远程目录挂载到本地使用。
### 安装
```shell
sudo apt install sshfs
```
### 使用
```shell
sshfs user@host:remotedir localdir
```
