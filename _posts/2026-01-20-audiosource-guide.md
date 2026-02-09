---
layout: post
title: Linux 用手机当麦克风Audio Source
tags: [linux]
---



cat > audiosource-guide.txt << 'EOF'
Linux 用手机当麦克风 —— 方案一（Audio Source / USB + ADB）

一、准备工作

1. 手机端
- 安卓手机
- 开启开发者模式
- 开启 USB 调试
- 安装 Audio Source（F-Droid 或 GitHub）

2. Linux 端
安装 ADB 与音频工具：

sudo apt update
sudo apt install -y adb pulseaudio-utils pavucontrol

下载 Audio Source 客户端脚本：

wget https://github.com/gdzx/audiosource/releases/latest/download/audiosource
chmod +x audiosource

二、开始使用

1. 连接手机
用 USB 线连接手机到电脑。
在手机上允许 USB 调试授权。

检查设备是否被识别：

adb devices

2. 启动音频转发

./audiosource run

手机端允许录音权限。
Linux 会出现新的输入设备：Audio Source。

3. 设置为系统默认麦克风

打开 pavucontrol：

pavucontrol

进入“输入设备”，选择“Audio Source”并设为默认。

三、测试麦克风

录制 5 秒：

arecord -d 5 test.wav

播放测试：

aplay test.wav

四、停止使用

按 Ctrl + C 停止脚本。

五、常见问题

1. 没有出现 Audio Source 设备？
重启音频服务：

systemctl --user restart pipewire pipewire-pulse

2. adb 看不到设备？
检查 USB 模式是否为“文件传输（MTP）”。

3. 延迟高？
确保使用原装数据线，USB 连接正常。

EOF