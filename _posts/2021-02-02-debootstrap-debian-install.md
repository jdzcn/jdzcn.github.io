---
layout: post
title: 用debootscrap最小化安装debian
tags: [linux]
---
通过官方ISO安装是通常做法，但是基于极简原则，我尝试通过debootstrap命令安装，以获得一个最小化系统。

### 准备磁盘

切换到root用户

```shell
sudo -i
```

磁盘分区格式化挂载

```shell
fdisk -l
mkfs.ext4 /dev/sda1
mount /dev/sda1 /mnt
```

### 安装

```sh
debootstrap buster /mnt http://mirrors.tuna.tsinghua.edu.cn/debian
```

### 配置系统

复制网络设置

```shell
cp /etc/network/interfaces /mnt/etc/network
```

获取分区UUID

```shell
blkid > /mnt/etc/fstab
```

生成fstab

```
echo 'UUID=*** / ext4 defaults 0 1' >/mnt/etc/fstab
```

切换新系统

```shell
cd /mnt
mount --bind /dev dev
mount --bind /dev/pts dev/pts
mount --bind /proc proc
mount --bind /sys sys
chroot /mnt
```

设置密码，主机名称

```shell
passwd
hostname yourhostname
```

安装内核及grub等

```shell
apt install linux-image-amd64 grub-pc sudo

#grub-install /dev/sda
#update-grub
```

### 可选配置

安装非免固件或AMD图形固件

```shell
#apt install firmware-amd-graphics -y
#apt install firmware-linux-nonfree
```

新建用户配置sudo免密

```shell
useradd -m -g users sb -s /bin/bash
passwd sb
echo 'sb ALL=(ALL)NOPASSWD:ALL'>>/etc/sudoers
```

配置时区同步

```shell
sudo timedatectl set-timezone Asia/Shanghai
sudo timedatectl set-ntp true
```

常用软件

```shell
sudo apt install openbox xterm alsa-utils mousepad pcmanfm tint2 
fonts-wqy-microhei trojan firefox-esr git volumeicon-alsa file-roller -y
```

