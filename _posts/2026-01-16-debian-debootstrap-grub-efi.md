---
layout: post
title: debian efi分区安装grub
tags: [linux]
---

#若出错grub-install: warning: EFI variables cannot be set on this system
#检查当前启动模式
ls /sys/firmware/efi

#进入chroot前挂载

for i in /dev /dev/pts /proc /sys /run; do sudo mount --rbind $i /mnt$i; done

sudo mount -t efivarfs efivarfs /mnt/sys/firmware/efi/efivars
#chroot后
apt install grub-efi-amd64
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=debian --recheck /dev/sda
update-grub 