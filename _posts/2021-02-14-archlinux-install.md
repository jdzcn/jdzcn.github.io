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
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
sudo pacman -Syy
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
pacman -S dhcpcd
systemctl enable dhcpcd
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

##### 安装显卡

```shell
	lspci | grep -e VGA -e 3D
	pacman -Ss xf86-video
	pacman -S xf86-video-intel xf86-video-amdgpu
```

##### 安装openbox等常用软件，常用配置请参考[本文](https://jdzcn.xyz/view.php?name=jdzcn.github.io/_posts/2021-02-02-openbox.md)

安装

```shell
	pacman -S xorg-server xorg-xinit openbox dmenu wqy-microhei xterm leafpad pcmanfm git firefox tint2 volumeicon
```

配置xinit启动

```shell
cp /etc/X11/xinit/xinitrc ~/.xinitrc && nano ~/.xinitrc
echo 'exec openbox-session'>>~/.xinitrc
startx
```

配置ly启动

```shell
wget http://172.96.193.223/sb/ly_0.5.0.zip
unzip ly_0.5.0.zip
sudo ./install.sh
systemctl enable ly
```
通过aur安装ly
``` shell
pacman -S --needed base-devel
git clone https://aur.archlinux.org/ly
cd ly
makepkg -si
systemctl enable ly
```


##### 设置声卡

```shell
	pacman -S alsa-utils
	#常用命令：alsamixer,amixer sset Master unmute,aplay -l,amixer scontrols
	#若无声音
	#nano ~/.asoundrc
	defaults.pcm.card 1
	defaults.pcm.device 0
	defaults.ctl.card 1
```

##### 新建用户配置sudo免密

```shell
useradd -m -g users sb -s /bin/bash
passwd sb
echo 'sb ALL=(ALL)NOPASSWD:ALL'>>/etc/sudoers
```

##### 配置时区同步

```shell
sudo timedatectl set-timezone Asia/Shanghai
sudo timedatectl set-ntp true
```

##### 中文输入法

```shell
pacman -S fcitx fcitx-im fcitx-configtool fcitx-table-extra fcitx-sunpinyin 
pacman -S fcitx-gtk2 fcitx-gtk3 fcitx-qt4 fcitx-qt5
```

### 参考

- [官方安装指南](https://wiki.archlinux.org/index.php/Installation_guide)