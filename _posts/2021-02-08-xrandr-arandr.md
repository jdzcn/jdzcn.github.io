---
layout: post
title: Linux桌面分辨率设置工具xrandr
tags: [linux]
---

xrandr是Linux桌面分辨率设置工具，图形前端为arandr。

### 安装
debian
```shell
sudo apt install arandr
```
arch
```shell
pacman -S arandr
```
### 检测设备分辨率
```shell
xrandr
```
### 设置
在桌面自启文件中设置(~/.config/openbox/autostart)
```shell
xrandr --output Virtual1 --mode 1440x900
```

### 参考
- [arch xrandr](https://wiki.archlinux.org/index.php/Xrandr)
