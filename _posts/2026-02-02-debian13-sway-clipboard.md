---
layout: post
title: debian sway共享剪贴板
tags: [linux]
---

### 1. 确保已安装依赖

在终端执行：

```bash
sudo apt update && sudo apt install cliphist wl-clipboard wofi -y

```

---

### 2. 复制以下内容到你的 Sway 配置文件

通常路径为 `~/.config/sway/config`。建议直接粘贴到文件末尾：

```text
### --- 剪贴板历史记录配置 (Win+V) --- ###

# 1. 启动监听守护进程 (支持文本和图片)
exec wl-paste --type text --watch cliphist store
exec wl-paste --type image --watch cliphist store

# 2. 绑定 Win+V 键 (Super + V)
# 执行逻辑：列出历史 -> wofi 搜索选择 -> 解码 -> 存入当前剪贴板
bindsym $mod+v exec cliphist list | wofi --dmenu --display-column 2 | cliphist decode | wl-copy

# 3. 设置 wofi 窗口样式（可选：使其居中浮动）
for_window [app_id="wofi"] floating enable, move position center

```

---

### 3. 如何立即生效

保存文件后，在 Sway 中按下：
`$mod + Shift + c` （默认重新加载配置快捷键）

### 常用管理命令（在终端直接用）：

* **查看所有历史：** `cliphist list`
* **清空所有记录：** `cliphist wipe`
* **删除单条记录：** `cliphist list | wofi -dmenu | cliphist delete`

**需要我帮你把“删除历史记录”也绑定一个快捷键（比如 Win+Delete）吗？**