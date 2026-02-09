---
layout: post
title: debian挂载windows共享目录
tags: [linux]
---

在 Debian 13 上挂载 Windows 10 的共享目录（通常使用 SMB/CIFS 协议），最稳妥且现代的方法是使用 cifs-utils。

以下是分步指南，包括临时挂载和开机自动挂载。

1. 准备工作
首先，确保你的 Windows 10 文件夹已经开启了“共享”，并且你知道 Windows 的 IP 地址、用户名和密码。

在 Debian 13 上安装必要的工具：

Bash
sudo apt update
sudo apt install cifs-utils
2. 临时挂载（测试用）
先手动挂载一次，确认连接是否通畅。
io
创建挂载点：

Bash
sudo mkdir -p /mnt/win_share
执行挂载命令：

Bash
sudo mount -t cifs //192.168.x.x/共享文件夹名 /mnt/win_share -o username=你的私有用户名,domain=WORKGROUP
输入命令后，系统会提示你输入 Windows 密码。

3. 开机自动挂载（推荐）
为了安全起见，我们不把密码直接写在配置文件里，而是使用一个凭据文件。

第一步：创建凭据文件
Bash
nano ~/.smbcredentials
输入以下内容：

Plaintext
username=你的Windows用户名
password=你的Windows密码
domain=WORKGROUP
保存退出后，限制权限（保护你的密码）：

Bash
chmod 600 ~/.smbcredentials
第二步：修改 /etc/fstab
Bash
sudo nano /etc/fstab
在文件末尾添加一行：

Plaintext
//192.168.x.x/shared  /mnt/win_share  cifs  credentials=/root/.smbcreds,uid=1000,gid=1000,file_mode=0775,dir_mode=0775,iocharset=utf8,_netdev,x-systemd.automount,x-systemd.idle-timeout=60  0  0

第三步：测试生效
Bash
sudo mount -a
如果没有报错，你的共享目录就永久挂载好了。