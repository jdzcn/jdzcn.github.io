---
layout: post
title: Apache2设置用户认证（BASIC）
tags: [linux]
---

Apache2——设置用户认证（BASIC）

``` shell
cd /etc/apache2
mkdir authentication

root@VM-0-9-ubuntu:~# htpasswd -c users user1
New password: 
Re-type new password: 
Adding password for user user1

cd /etc/apache2/sites-enabled

nano 000-default.conf

#Download Authentication
		#设置要进行认证的目录，此处为/var/www/html/download。
        <Directory "/var/www/html/download">
        	#设置认证类型（由mod_auth提供的Basic）
                AuthType Basic
                #设置认证领域，相同领域内避免用户重复输入密码
                AuthName "Download"
                #设置密码文件
                AuthUserFile /etc/apache2/authentication/users
                #设置允许访问的用户(valid-user：允许所有合法的用户访问;user user1:仅限用户user1访问；user user1 user2：仅限用户user1和user2访问)
                Require valid-user
        </Directory>
        
systemctl restart apache2.service
```    