---
layout: post
title: win10生成SSH 密钥
tags: [win]
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