---
layout: post
title: 使用命令行启动 VirtualBox 虚拟机
tags: [linux]
---


``` shell
VBoxManage list vms
VBoxManage startvm XP --type gui
$ VBoxManage list runningvms # 列出运行中的虚拟机
$ VBoxManage controlvm XP acpipowerbutton # 关闭虚拟机，等价于点击系统关闭按钮，正常关机
$ VBoxManage controlvm XP poweroff # 关闭虚拟机，等价于直接关闭电源，非正常关机
$ VBoxManage controlvm XP pause # 暂停虚拟机的运行
$ VBoxManage controlvm XP resume # 恢复暂停的虚拟机
$ VBoxManage controlvm XP savestate # 保存当前虚拟机的运行状态
```

