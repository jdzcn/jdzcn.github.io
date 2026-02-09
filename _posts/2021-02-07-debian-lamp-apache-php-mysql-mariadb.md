---
layout: post
title: debian10搭建lamp环境
tags: [linux]
---

lamp包括linux,apache,mariadb,php，是经典的网络开发环境。

### Apache

安装

```shell
# apt install apache2 
```

运行、查看状态等

```shell
# systemctl start apache2.service 
# systemctl restart apache2.service 
# systemctl stop apache2.service
# systemctl reload apache2.service 
# systemctl status apache2.service 
```

开启防火墙

```shell
# ufw allow www
# ufw allow https
# ufw status
```

测试

浏览器输入以下网址

```
http://127.0.0.1/
OR
http://localhost/
```



### MariaDB

安装

```shell
# apt install mariadb-server
```

配置

```shell
# mysql_secure_installation
```

新建数据库和用户

```shell
# mysql -u root -p
MariaDB> CREATE USER 'song'@'localhost' IDENTIFIED BY '123456';
MariaDB> GRANT ALL PRIVILEGES ON *.* TO 'song'@'localhost';
MariaDB> FLUSH PRIVILEGES;

MariaDB> create database myshop;

#import data
#mysql -uroot -psongbin myshop<myshop.sql
#mysql -uroot -psongbin store<store.sql
```

### PHP

安装

```shell
# apt install php libapache2-mod-php php-mysql
```

重启apache

```shell
# systemctl reload apache2
# systemctl status apache2
```

测试

```shell
echo '<?php phpinfo(); ?>'>/var/www/html/info.php
```

![](https://i.loli.net/2021/02/07/jeJfBbXrlNa5A4q.png)

### 参考

- [How to Install LAMP on Debian 10 Server](https://www.tecmint.com/install-lamp-on-debian-10-server/)
