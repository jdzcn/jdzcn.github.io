---
layout: post
title: 自定义grub启动windows
---

安装linux,windows多系统时，偶见windows分区无法启动，可将以下内容修改UUID，命名为custom.cfg,复制到/boot/grub。
```
#sudo cp custom.cfg /boot/grub

if [ "${grub_platform}" == "pc" ]; then
	menuentry "Microsoft Windows Vista/7/8/8.1/10 BIOS/MBR" {
		insmod part_msdos
		insmod ntfs
		insmod ntldr     
		search --no-floppy --fs-uuid --set=root --hint-bios=hd0,msdos1 --hint-efi=hd0,msdos1 --hint-baremetal=ahci0,msdos1 44B0C989B0C98242
		ntldr /bootmgr
	}
fi
menuentry "System restart" {
	echo "System rebooting..."
	reboot
}
menuentry "System shutdown" {
	echo "System shutting down..."
	halt
}
```