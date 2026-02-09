---
layout: post
title: Apache2设置基础用户认证
tags: [linux]
---


``` shell

apt -y install apache2-utils

vi /etc/apache2/sites-available/auth-basic.conf
<Directory /var/www/html/auth-basic>
    SSLRequireSSL
    AuthType Basic
    AuthName "Basic Authentication"
    AuthUserFile /etc/apache2/.htpasswd
    require valid-user
</Directory> 

htpasswd -c /etc/apache2/.htpasswd debian

New password:     
# set password
Re-type new password:
Adding password for user debian
root@www:~# 
mkdir /var/www/html/auth-basic

root@www:~# 
a2ensite auth-basic

systemctl restart apache2
```

### 参考

- [Server World](https://www.server-world.info/en/note?os=Debian_11&p=httpd&f=8)