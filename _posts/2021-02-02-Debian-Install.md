---
layout: post
title: 使用debootscrap安装debian
tags: [linux]
---
通过官方ISO安装是通常做法，但是基于极简原则，我尝试通过debootstrap命令安装，以获得一个最小化系统。

### 准备磁盘

切换到root用户

```shell
sudo -i
```

