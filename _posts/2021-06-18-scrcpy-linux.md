---
layout: post
title: debian中使用scrcpy操作手机投屏
tags: [linux]
---



scrcpy是linux系统操作手机和手机投屏常用软件，安装方法如下：

### 安装

``` shell
sudo apt update
sudo apt install snapd
sudo snap install core

sudo snap install scrcpy
```

### 运行
打开手机设置->我的设备->全部参数,单击miui版本5次打开开发者模式。
打开设置->开发者选项->USB调试，数据线连接手机电脑。
```shell
/snap/bin/scrcpy
```
### 命令参数

关闭手机屏幕	scrcpy -S
限制画面分辨率	scrcpy -m 1024 (比如限制为 1024)
修改视频码率	scrcpy -b 4M (默认 8Mbps，改成 4Mbps)
裁剪画面	scrcpy -c 1224:1440:0:0
表示分辨率 1224x1440 并且偏移坐标为 (0,0)
多设备切换	scrcpy -s 设备ID (使用 adb devices 命令查看设备ID)
窗口置顶	scrcpy -T
显示触摸点击	scrcpy -t
在演示或录制教程时，可在画面上对应显示出点击动作
全屏显示	scrcpy -f
文件传输默认路径	scrcpy --push-target /你的/目录
将文件拖放到 scrcpy 可以传输文件，此命令指定默认保存目录
只读模式(仅显示不控制)	scrcpy -n
屏幕录像	scrcpy -r 视频文件名.mp4 或 .mkv
屏幕录像 (禁用电脑显示)	scrcpy -Nr 文件名.mkv
设置窗口标题	scrcpy --window-title '异次元好棒！'
同步传输声音	可借助 USBaudio 这个开源项目实现，但仅支持 Linux 系统

### 快捷键

切换全屏模式	Ctrl+F
将窗口调整为1：1（完美像素）	Ctrl+G
调整窗口大小以删除黑色边框	Ctrl+X | 双击黑色背景
设备 HOME 键	Ctrl+H | 鼠标中键
设备 BACK 键	Ctrl+B | 鼠标右键
设备 任务管理 键 (切换APP)	Ctrl+S
设备 菜单 键	Ctrl+M
设备音量+键	Ctrl+↑
设备音量-键	Ctrl+↓
设备电源键	Ctrl+P
点亮手机屏幕	鼠标右键
复制内容到设备	Ctrl+V
启用/禁用 FPS 计数器（stdout）	Ctrl+i
安装APK	将 apk 文件拖入投屏
传输文件到设备	将文件拖入投屏（非apk）

注：在 macOS 平台上，请使用 cmd 代替 Ctrl。

### 使用 WIFi 无线连接：

Scrcpy 使用 adb 与 Android 设备通讯，而 adb 本身是支持无线连接的。因此除了 USB 数据线之外，我们也能无线使用。前提是需要保证手机和电脑处于同一局域网 (连接到相同的 WiFi 路由器)，步骤如下：

- 查询设备当前的 IP 地址 (设置 →关于手机→状态)
- 启用 adb TCP/IP 连接，执行命令：/snap/bin/scrcpy.adb tcpip 5555，其中 5555 为端口号
- 拔掉你的数据线
- 通过 WiFi 进行连接，执行命令：/snap/bin/scrcpy.adb connect 设备IP地址:5555
- 重新启动 scrcpy 即可
- 如果 WiFi 较慢，可以调整码率：scrcpy -b 3M -m 800，意思是限制 3 Mbps，画面分辨率限制 800，数值可以随意调整。
- 如需切换回 USB 模式，执行：/snap/bin/scrcpy.adb usb

### 参考
- [scrcpy使用](https://www.iplaysoft.com/scrcpy.html)