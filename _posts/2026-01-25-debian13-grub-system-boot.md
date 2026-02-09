---
layout: post
title: debian 使用systemd-boot启动
tags: [linux]
---

从 GRUB 切换到 systemd-boot 可以显著提升启动速度并简化引导配置。在 Debian 13 (Trixie) 中，由于其对 UEFI 的良好支持，这个过程已经变得相对简单。

⚠️ 注意： > 1. systemd-boot 仅支持 UEFI 模式。如果你的电脑还在用传统的 BIOS (Legacy) 模式，请不要尝试。 2. 操作引导程序涉及系统底层，建议在操作前备份重要数据，并准备一个 Debian Live USB 以防万一。

第一步：确认环境
首先，确保你的系统是以 UEFI 模式启动的：

Bash

ls /sys/firmware/efi/efivars
如果该目录存在且有文件，说明你是 UEFI 模式。

第二步：安装与初始化
Debian 13 默认已经集成了 systemd-boot 工具，你只需要执行安装命令：

安装引导程序：

Bash

sudo bootctl install
这会将 systemd-boot 的二进制文件复制到你的 EFI 系统分区（通常是 /boot/efi）。

配置自动更新： 为了让以后内核升级时能自动更新引导项，建议安装 systemd-boot-efi 钩子（如果尚未安装）：

Bash

sudo apt install systemd-boot-efi
第三步：配置 Loader
systemd-boot 的配置文件非常直观，主要分为“全局配置”和“启动项配置”。

编辑主配置文件：

Bash

sudo nano /boot/efi/loader/loader.conf
写入以下内容：

Plaintext

default  debian.conf
timeout  3
console-mode max
创建 Debian 启动项： 你需要查看当前正在使用的内核版本和根分区 UUID：

Bash

# 查看内核版本
uname -r
# 查看根分区 UUID (假设你的根目录是 / )
lsblk -f
创建启动项文件：

Bash

sudo nano /boot/efi/loader/entries/debian.conf
写入以下内容（请根据你的实际 UUID 和内核版本修改）：

Plaintext

title   Debian 13
linux   /vmlinuz
initrd  /initrd.img
options root=UUID=你的根分区UUID rw quiet
注：Debian 通常会在 EFI 分区根目录创建 vmlinuz 和 initrd.img 的符号链接，如果没有，你需要手动指定完整路径。
sudo cp /vmlinuz /boot/efi/
sudo cp /initrd.img /boot/efi/

第四步：清理 GRUB（可选但建议）
确认 systemd-boot 配置无误后，你可以移除 GRUB 以避免冲突：

卸载 GRUB 软件包：

Bash

sudo apt purge grub-common grub-pc-bin grub-efi-amd64
sudo apt autoremove
清理残留文件： 删除 /boot/grub 目录。

验证与重启
执行以下命令检查引导设置状态：

Bash

bootctl status
如果一切显示正常，你可以放心地重启系统了。

============================================================
           systemd-boot 多余启动项清理指南
============================================================

1. 确认当前正在使用的内核（千万不要删这个！）
------------------------------------------------------------
命令：uname -r
提示：删除前请确保你要删的版本号与此输出不符。

2. 删除启动菜单条目 (.conf 文件)
------------------------------------------------------------
systemd-boot 的每个菜单项都对应一个文件，存放路径通常在：
路径：/boot/loader/entries/  (或 /efi/loader/entries/)

操作：
# 列出所有启动项
ls /boot/loader/entries/

# 删除不需要的项（将 <VERSION> 替换为实际版本号）
sudo rm /boot/loader/entries/debian-<VERSION>.conf

3. 清理 EFI 分区内的物理文件 (内核与 Initrd)
------------------------------------------------------------
如果只是删除了 .conf 文件，内核镜像依然占用 ESP 分区空间。
通常它们存放于：
- /boot/<machine-id>/<version>/
- 或者 /boot/vmlinuz-<version> (取决于你的配置)

操作：
# 检查占用空间的内核文件夹
ls /boot/<你的Machine-ID>/

# 删除对应的旧内核文件夹
sudo rm -rf /boot/<Machine-ID>/<旧版本号>/

4. 标准做法：通过 APT 卸载（推荐）
------------------------------------------------------------
在 Debian 中，最干净的方法是直接卸载内核包。这会自动触发脚本
删除上述的 .conf 文件和 EFI 分区文件。

操作：
# 查找所有已安装的内核包
dpkg --list | grep linux-image

# 彻底卸载旧内核
sudo apt purge linux-image-<旧版本号>-amd64

5. 清理 UEFI 固件残留 (可选)
------------------------------------------------------------
如果主板 BIOS 启动项里仍有已删掉的系统名称，请使用 efibootmgr。

操作：
# 查看引导列表
sudo efibootmgr

# 删除指定编号（例如编号为 0005）
sudo efibootmgr -b 0005 -B

============================================================
注意：操作前建议执行 `bootctl list` 确认当前生效的配置文件路径。
============================================================

你会遇到麻烦吗？ 如果在重启后无法进入系统，请进入 BIOS 选择启动项，手动指定 EFI 分区中的 systemd-boot 路径。