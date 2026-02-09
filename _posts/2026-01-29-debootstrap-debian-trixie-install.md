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
lsblk
ls /sys/firmware/efi  #检查当前引导方式uefi
fdisk -l

mkfs.ext4 /dev/sda1
apt install dosfstools
mkfs.vfat -F 32 -n EFI /dev/xxx 
mount /dev/sda2 /mnt
```

配置[软件源](http://jdztao.xyz/sblog/view.php?name=_posts/2021-11-18-mirrors-debian.md)
```shell
#/etc/apt/sources.list
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bookworm main contrib non-free non-free-firmware
```
### 安装

```sh
apt install debootstrap
debootstrap trixie /mnt http://mirrors.tuna.tsinghua.edu.cn/debian
```
#uefi模式需要：
mkdir -p /mnt/boot/efi
mount /dev/sda1 /mnt/boot/efi
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
UUID="" /boot/efi vfat defaults 0 2
```
mount -a #验证fstab 
切换新系统

```shell
cd /mnt

for i in /dev /dev/pts /proc /sys /run; do sudo mount --rbind $i /mnt$i; done



chroot /mnt
```

设置密码，主机名称

```shell
passwd
hostname yourhostname
```

安装内核及grub等

```shell
apt install linux-image-amd64  sudo

#bios模式
apt install grub-pc
grub-install /dev/sda

#uefi模式使用
apt install grub-efi-amd64
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=debian --recheck  /dev/sda
##若出错grub-install: warning: EFI variables cannot be set on this system,
#退到chroot前，运行：
#sudo mount -t efivarfs efivarfs /mnt/sys/firmware/efi/efivars

#若要修改默认启动系统或时间
#sudo nano /etc/default/grub
#修改项GRUB_DEFAULT=0

update-grub
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

配置HOSTS

```shell
#/etc/hosts
	127.0.0.1	localhost
	::1		localhost
	127.0.0.1	myhostname.localdomain	myhostname
```

配置时区同步

```shell
sudo timedatectl set-timezone Asia/Shanghai
sudo timedatectl set-ntp true
sudo systemctl start systemd-timesyncd
```

常用软件

```shell
sudo apt install i3 lightdm
sudo apt install openbox xterm alsa-utils mousepad pcmanfm tint2 fonts-wqy-microhei trojan firefox-esr git volumeicon-alsa file-roller nomacs imagemagick -y
```
配置默认启动界面
```shell
systemctl set-default multi-user.target/graphical.target
```
