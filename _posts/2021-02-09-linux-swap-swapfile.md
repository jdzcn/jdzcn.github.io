---
layout: post
title: Linux建立交换文件
---

Linux安装都需要建立交换分区，我们也可以在安装完成后建立交换文件。

### bash代码

```shell
#!/bin/bash
sudo dd if=/dev/zero of=/swapfile bs=1024 count=1048576
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo echo '/swapfile swap swap defaults 0 0'>>/etc/fstab
```

### 参考

- [How to](https://linuxize.com/post/how-to-add-swap-space-on-debian-10/)