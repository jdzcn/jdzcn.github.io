---
layout: post
title: 解决vsftpd中文乱码
tags: [linux]
---


``` shell
#vi /etc/vsftpd.conf

utf8_filesystem=YES
```

### 参考

- [Server World](https://www.server-world.info/en/note?os=Debian_11&p=ftp&f=1)