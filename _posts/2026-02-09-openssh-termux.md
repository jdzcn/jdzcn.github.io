---
layout: post
title: Termux配置sshd
tags: [linux]
---

Termux 配置 sshd 完整步骤

一、安装 openssh（Termux 官方包）

执行以下命令更新包并安装 openssh，包含 sshd 服务端、ssh 客户端及相关工具：

pkg update

pkg install openssh -y

二、启动 sshd 服务

Termux 的 sshd 默认监听 8022 端口（非标准 22 端口，无需root权限）

启动命令：sshd

停止命令：pkill sshd

三、设置登录密码（必做步骤）

Termux  sshd 需先设置密码才能登录，执行命令后输入两次密码即可：

passwd

四、查看设备 IP（用于外部连接）

执行以下任一命令，在输出结果中找到 wlan0 网卡对应的局域网 IP（如 192.168.1.xxx）：

ifconfig

或

ip a

五、从其他设备（电脑/手机）连接 Termux

在目标设备的终端中执行命令，格式如下（替换对应参数）：

ssh -p 8022 用户名@设备IP

说明：

- 用户名：执行 whoami 命令可查看 Termux 用户名（通常为 u0_axxx 格式）

- 设备IP：第四步获取的局域网 IP

- 端口：固定为 8022，不可省略 -p 参数

示例：ssh -p 8022 u0_a228@192.168.1.53

六、自定义配置 sshd（可选，增强安全性）

1. 配置文件路径：~/.ssh/sshd_config（若不存在，手动创建即可）

2. 常用配置示例（写入配置文件后保存）：

Port 8022          # 监听端口，可修改为其他未占用端口（如 8888）

PasswordAuthentication yes  # 允许密码登录（默认开启）

PubkeyAuthentication yes    # 允许密钥登录（推荐开启）

PermitRootLogin no          # 禁止root登录（Termux 默认无root，仅作兼容）

3. 配置生效：修改后需重启 sshd 服务

pkill sshd

sshd

七、设置密钥登录（更安全，推荐）

1. 在客户端（如电脑）生成密钥对：

ssh-keygen  # 按提示回车即可，无需设置密码（或按需设置）

2. 将公钥复制到 Termux：

ssh-copy-id -p 8022 u0_a228@192.168.1.53

3. 验证成功后，可关闭密码登录（修改 sshd_config）：

PasswordAuthentication no



4. 重启 sshd 使配置生效

八、设置 sshd 开机自启动（Termux）

1. 安装 termux-boot 插件：

pkg install termux-boot -y

2. 创建启动脚本：

mkdir -p ~/.termux/boot/  # 若目录不存在则创建

nano ~/.termux/boot/start-sshd

3. 在脚本中写入以下内容并保存（按 Ctrl+O 保存，Ctrl+X 退出）：

sshd

4. 赋予脚本执行权限：

chmod +x ~/.termux/boot/start-sshd

九、常见问题排查

1. 连接被拒绝：

- 确认 sshd 已启动：pgrep sshd（有输出则表示运行中）

- 确认连接端口为 8022（或自定义端口）

- 确保客户端与 Termux 设备在同一局域网

2. 密码正确但无法登录：

- 检查 sshd_config 中 PasswordAuthentication 是否为 yes

- 重新设置密码：passwd

#配置服务器简称ssh redmi
#nano ~/.ssh/config
Host redmi
    HostName 192.168.1.53
    Port 8022
    User u0_a228
    IdentityFile ~/.ssh/id_ed25519
