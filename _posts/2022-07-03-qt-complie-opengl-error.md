---
layout: post
title: Linux qt 安装及解决cannot find -lGL错误
tags: [linux]
---

linux qt install
``` shell

sudo apt install cmake gcc g++ 
```
继续运行qt安装文件


1.查找opengl库

``` shell
find /usr -name libGL*
```

2.根据搜索结果，建立连接：

``` shell
sudo  ln -s /usr/lib/x86_64-linux-gnu/libGL.so.1 /usr/lib/libGL.so
```