---
layout: post
title: win10免密登录
tags: [win10]
---

打开注册表路径

HKEY_LOCAL_MACHINE＼SOFTWARE＼Microsoft＼Windows NT＼CurrentVersion＼PasswordLess＼Device

修改下面的DevicePasswordLessBuildVersion值为0，那么再运行Netplwiz，【要使用本计算机，用户必须输入用户名和密码】复选框就又出现了。
运行
