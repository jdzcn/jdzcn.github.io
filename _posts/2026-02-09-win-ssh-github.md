---
layout: post
title: Windows 10 上生成 SSH 密钥并关联 GitHub
tags: [win]
---

在 Windows 10 上生成 SSH 密钥并关联 GitHub 是实现代码免密推送的标准操作。整个过程分为“生成、获取、上传”三个步骤。

---

### 第一步：检查并生成 SSH 密钥

Windows 10 已经内置了 OpenSSH 客户端，你可以直接通过 **CMD** 或 **PowerShell** 完成。

1. 按下 `Win + R`，输入 `cmd` 并回车。
2. 输入以下命令（请将 `your_email@example.com` 替换为你的 GitHub 注册邮箱）：
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"

```


*注：ed25519 是目前最推荐的加密算法。如果系统不支持，可改用 `ssh-keygen -t rsa -b 4096`。*
3. **一路回车**即可（它会询问保存路径和密码，默认不设密码直接按回车）。

---

### 第二步：获取公钥内容

密钥生成后，会保存在 `C:\Users\你的用户名\.ssh` 目录下。你需要复制 **公钥（.pub 文件）** 的内容。

1. 在终端输入以下命令直接查看内容：
```bash
type %userprofile%\.ssh\id_ed25519.pub

```


*如果你生成的是 rsa 密钥，文件名则是 `id_rsa.pub`。*
2. **选中并复制**出现的以 `ssh-ed25519` 开头的那一长串字符串。

---

### 第三步：将公钥上传至 GitHub

1. 登录你的 [GitHub](https://github.com/) 账号。
2. 点击右上角头像 -> **Settings**。
3. 在左侧菜单栏找到 **SSH and GPG keys**。
4. 点击右上角的 **New SSH key**。
5. 在 **Title** 处随便起个名字（如 "Win10-Laptop"），在 **Key** 处粘贴你刚才复制的内容。
6. 点击 **Add SSH key**。

---

### 第四步：验证连接

返回 CMD 或 PowerShell，输入：

```bash
ssh -T git@github.com

```

* 如果系统提示 `Are you sure you want to continue connecting (yes/no/[fingerprint])?`，输入 **`yes`** 并回车。
* 如果看到 `Hi [你的用户名]! You've successfully authenticated...`，说明配置成功！

---

### 常见问题小贴士

* **权限问题：** 如果提示找不到 `ssh-keygen`，请确保 Windows 10 已开启“OpenSSH 客户端”功能（设置 -> 应用 -> 可选功能 -> 添加功能）。
* **HTTPS 转 SSH：** 如果你之前的项目是用 HTTPS 克隆的，记得用 `git remote set-url origin git@github.com:用户名/仓库名.git` 切换成 SSH 协议，这样才能享受免密待遇。

**你的 Git 已经配置好全局用户名和邮箱了吗？如果还没做，我可以提供对应的配置命令。**