---
layout: post
title: debian建立btrfs分区
tags: [linux]
---


Debian 13 Btrfs 手动安装全流程脚本
Bash
```
#!/bin/bash
# 假设 /dev/sda1 是 EFI，/dev/sda2 是 Btrfs 主分区

# --- 1. 物理挂载与子卷创建 ---
mount /dev/sda2 /mnt
btrfs subvolume create /mnt/@
btrfs subvolume create /mnt/@home
btrfs subvolume create /mnt/@log
btrfs subvolume create /mnt/@snapshots
umount /mnt

# --- 2. 逻辑挂载（安装视角） ---
# 开启 zstd 压缩，节省空间并保护 SSD
mount -o subvol=@,compress=zstd:3,noatime /dev/sda2 /mnt

mkdir -p /mnt/{home,var/log,.snapshots,boot/efi}
mount -o subvol=@home,compress=zstd:3,noatime /dev/sda2 /mnt/home
mount -o subvol=@log,compress=zstd:3,noatime /dev/sda2 /mnt/var/log
mount -o subvol=@snapshots,compress=zstd:3,noatime /dev/sda2 /mnt/.snapshots
mount /dev/sda1 /mnt/boot/efi

# --- 3. 执行 debootstrap ---
# 使用中国科学技术大学镜像源加速
debootstrap --arch amd64 --include=linux-image-amd64,grub-efi,btrfs-progs,nano,locales trixie /mnt https://mirrors.ustc.edu.cn/debian/

# --- 4. 绑定系统环境并进入 chroot ---
for i in /dev /dev/pts /proc /sys /run; do mount -B $i /mnt$i; done
```
echo "系统已就绪，请执行：chroot /mnt /bin/bash 开启内部配置"
🛠️ 进入 Chroot 后的必做操作（复制即用）
进入 chroot /mnt 后，依次执行：

设置主机名： echo "my-debian" > /etc/hostname

生成 fstab（关键）：
```
Bash
cat <<EOF > /etc/fstab
UUID=$(blkid -s UUID -o value /dev/sda2) / btrfs subvol=@,compress=zstd:3,noatime 0 0
UUID=$(blkid -s UUID -o value /dev/sda2) /home btrfs subvol=@home,compress=zstd:3,noatime 0 0
UUID=$(blkid -s UUID -o value /dev/sda2) /var/log btrfs subvol=@log,compress=zstd:3,noatime 0 0
UUID=$(blkid -s UUID -o value /dev/sda2) /.snapshots btrfs subvol=@snapshots,compress=zstd:3,noatime 0 0
UUID=$(blkid -s UUID -o value /dev/sda1) /boot/efi vfat defaults 0 2
EOF
#配置网络与用户： apt update && apt install network-manager passwd root

#安装并更新引导： grub-install /dev/sda update-grub
```
💡 最后的避坑指南
/var/log 子卷：如果在安装某些包时报错，是因为有些程序在 /var/log 下需要特定的文件夹。手动 mkdir -p /var/log/apt 等即可解决。

写时复制（CoW）：如果你以后要跑虚拟机（如 VirtualBox），记得对存放镜像的目录执行 chattr +C，否则 Btrfs 的 CoW 会让虚拟机运行卡顿。