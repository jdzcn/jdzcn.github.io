---
layout: post
title: micro常用操作
tags: [linux]
---

### 1. 基础快捷键 (同 Windows 习惯)

* **保存并退出**
* `Ctrl + S` : 保存
* `Ctrl + Q` : 退出 (若未保存会提示)


* **剪切板操作**
* `Ctrl + C` : 复制选中文字
* `Ctrl + X` : 剪切选中文字
* `Ctrl + V` : 粘贴


* **撤销与搜索**
* `Ctrl + Z` : 撤销 (Undo)
* `Ctrl + Y` : 重做 (Redo)
* `Ctrl + F` : 查找内容
* `Ctrl + N` : 查找下一个
* `Ctrl + P` : 查找上一个


* **选择与跳转**
* `Ctrl + A` : 全选
* `Ctrl + G` : 跳转到指定行 (输入行号后回车)



---

### 2. 进阶指令 (按 Ctrl + E 进入命令模式)

* `vsplit` : 垂直分屏
* `hsplit` : 水平分屏
* `set mouse off` : 关闭鼠标支持（方便终端选中文本）
* `set tabsize 4` : 设置缩进为 4 格
* `help` : 查看官方详细文档

---

### 3. 一键设置：让 micro 适配 Sway/Wayland

在终端执行以下命令，可以让 micro 自动支持系统剪贴板、开启行号显示、并优化缩进：

```bash
micro -set clipboard terminal
micro -set mouse on
micro -set tabsize 4
micro -set autoindent true
micro -set cursorline true

```

---

### 4. 将 micro 设为系统默认编辑器

如果你希望在执行 `crontab -e` 或其他系统命令时默认打开 micro，请在终端执行：

```bash
echo 'export EDITOR=micro' >> ~/.bashrc
source ~/.bashrc

```

---

