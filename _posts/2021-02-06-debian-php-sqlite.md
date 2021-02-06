---
layout: post
title: debian10搭建本地php和sqlite环境
tags: [linux]
---

php是被广泛使用的网络服务器语言，一般搭配mysql应用于网络数据库系统。但是对于一些个人和小型系统，sqlite数据库更加适合。sqlite是嵌入式关系型数据库，有广泛 的设备支持，轻量而易于维护。

### 安装

```shell
sudo apt install php php-sqlite3
```

### 运行

```shell
php -S localhost:8000 -t sblog/
```

-S指定本地浏览器访问地址，-t 指定服务器本地目录，缺省为当前目录。