---
layout: post
title: rclone配置sftp免密同步
tags: [linux]
---

# Rclone SFTP 与 Windows SSH 密钥配置全指南

本指南涵盖了在 Windows 10 上生成密钥并将其配置到 Rclone 的完整流程。

---

## 第一部分：在 Windows 10 生成 SSH 密钥

打开 **PowerShell** 并执行以下步骤：

### 1. 生成密钥对
```powershell
ssh-keygen -t rsa -b 4096 -C "备注名称"
请谨慎使用此类代码。

保存路径：按回车接受默认路径 (C:\Users\用户名\.ssh\id_rsa)。
密码(Passphrase)：若追求自动化，可直接按两次回车留空。
2. 获取公钥内容
用于部署到服务器：
powershell
cat ~/.ssh/id_rsa.pub
请谨慎使用此类代码。

注意：请将输出的以 ssh-rsa 开头的长字符串复制，并添加到服务器的 ~/.ssh/authorized_keys 文件中。
第二部分：Rclone SFTP 配置
1. 交互式配置流程
运行 rclone config，按照以下逻辑输入：
配置项	输入内容

n) New remote	输入名称（如 myserver）
Storage Type	输入 sftp
host	服务器 IP 地址
user	SSH 用户名
key_file	C:\Users\你的用户名\.ssh\id_rsa
pubkey_auth	true (如果使用密钥认证)
2. 配置文件直接编辑 (rclone.conf)
你可以直接将以下代码块粘贴到 rclone.conf 中：
```
[myserver]
type = sftp
host = 1.2.3.4
user = root
port = 22
key_file = C:\Users\你的用户名\.ssh\id_rsa
```
# 区分大小写。如果私钥没有设置密码，则无需 pass 字段
请谨慎使用此类代码。

第三部分：常用管理命令
常用操作
bash
# 测试连接：列出根目录
rclone lsd myserver:

# 同步本地文件夹到服务器
rclone sync C:\mydata myserver:/backup --progress

# 挂载为本地磁盘 (需安装 WinFSP)
rclone mount myserver:/ X: --vfs-cache-mode writes
请谨慎使用此类代码。

相关资源：Rclone 官方文档 | Microsoft OpenSSH 指南