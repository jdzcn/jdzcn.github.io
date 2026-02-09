---
layout: post
title: debian自动登录i3或sway
tags: [linux]
---

[步骤1: 自动登录配置]
路径: sudo systemctl edit getty@tty1.service
内容:
[Service]
ExecStart=
ExecStart=-/sbin/agetty --autologin 你的用户名 --noclear %I $TERM

[步骤2: i3启动配置]
路径: ~/.xinitrc
内容:
exec i3

[步骤3: 自动触发配置]
路径: ~/.bash_profile
内容:
if [[ -z $DISPLAY && $(tty) == /dev/tty1 ]]; then
    exec startx
fi
