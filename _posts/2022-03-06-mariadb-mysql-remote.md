---
layout: post
title: 远程连接mariadb database
tags: [linux]
---

``` shell
grant all privileges on *.* to 'root'@'192.168.1.%' identified by 'songbin' with grant option;

select user,host from mysql.user where host<>'localhost';
```

nano /etc/mysql/my.cnf
``` shell
port=3306
```

nano /etc/mysql/mariadb.conf.d/50-server.cnf
``` shell
bind-address=0.0.0.0
```
