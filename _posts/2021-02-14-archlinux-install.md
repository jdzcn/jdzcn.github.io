---
layout: post
title: archlinux安装指南
tags: [linux]
---
### 制作启动U盘

通过[官网](https://archlinux.org/)或[清华](https://mirrors.tuna.tsinghua.edu.cn/)等镜像网站下载ISO文件，实体安装需制作安装U盘，虚拟机则不必。Linux制作启动U盘命令：

```shell
$ sudo dd if=name-of-iso.iso of=/dev/sdb status="progress"
```

### 准备磁盘

启动安装盘，磁盘分区格式化挂载

```shell
fdisk -l
mkfs.ext4 /dev/sda1
mount /dev/sda1 /mnt
```

### 安装

选择镜像，将中国服务器移到最前面

```shell
#nano /etc/pacman.d/mirrorlist
```

复制文件

```sh
pacstrap /mnt base linux linux-firmware
```

### 配置系统

生成分区表

```shell
genfstab -U /mnt >> /mnt/etc/fstab
```

切换目标系统

```shell
arch-chroot /mnt
```

网络设置

```shell
# nano /etc/systemd/network/20-wired.network

	[Match]
	Name=enp1s0

	[Network]
	DHCP=ipv4

	#使用静态 IP 的有线适配器

	[Match]
	Name=enp1s0

	[Network]
	Address=10.1.10.9/24
	Gateway=10.1.10.1
	DNS=10.1.10.1
	#DNS=8.8.8.8

	#启动网络服务
	systemctl start/enable systemd-networkd
```

设置语言

```shell
	#编辑locale.gen,取消所需语言#号
	#nano /etc/locale.gen
	locale-gen
	echo LANG=en_US.UTF-8 > /etc/locale.conf
```

设置主机名称

```shell
	#echo myhostname >/etc/hostname
	#nano /etc/hosts
	127.0.0.1	localhost
	::1		localhost
	127.0.0.1	myhostname.localdomain	myhostname
```

设置密码

```shell
passwd
```

安装引导grub

```shell
	#pacman -S grub ntfs-3g os-prober nano 
	#grub-install --target=i386-pc /dev/sdX #sdX目标硬盘
	#grub-mkconfig -o /boot/grub/grub.cfg
	#exit && umount-R /mnt && shutdown -h now
	
	#若需要修改GRUB默认启动项
	#sudo nano /etc/default/grub
	GRUB_DEFAULT=0
	#重建grub配置
	grub-mkconfig -o /boot/grub/grub.cfg
```

### 可选配置

安装openbox，常用配置请参考[本文](https://jdzcn.xyz/view.php?name=jdzcn.github.io/_posts/2021-02-02-debootstrap-debian-install.md)

```shell
	lspci | grep -e VGA -e 3D
	pacman -Ss xf86-video
	pacman -S xf86-video-intel
	pacman -S xorg-server xorg-xinit lightdm openbox dmenu wqy-microhei xterm leafpad pcmanfm git firefox trojan tint2 volumeicon
```

设置声卡

```shell
	pacman -S alsa-utils
	#常用命令：alsamixer,amixer sset Master unmute,aplay -l,amixer scontrols
	#若无声音
	#nano ~/.asoundrc
	defaults.pcm.card 1
	defaults.pcm.device 0
	defaults.ctl.card 1
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

中文输入法

```shell
pacman -S fcitx fcitx-im fcitx-configtool fcitx-table-extra fcitx-sunpinyin 
pacman -S fcitx-gtk2 fcitx-gtk3 fcitx-qt4 fcitx-qt5
```

