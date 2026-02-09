---
layout: post
title: debian13优化bash history命令fzf使用方法
tags: [linux]
---

sudo apt update
sudo apt install fzf


在 ~/.bashrc 文件末尾添加：

eval "$(fzf --bash)"


应用更改：
执行 source ~/.bashrc 

你可以通过设置环境变量来自定义搜索菜单的外观和默认命令： 
bash
# 设置菜单高度、边框和反向布局
export FZF_DEFAULT_OPTS='--height 40% --layout=reverse --border'

# 使用 fd 代替 find 以获得更快的搜索速度（需安装 fd-find 并在 Debian 中链接为 fd）
# sudo apt install fd-find && ln -s $(which fdfind) ~/.local/bin/fd
export FZF_DEFAULT_COMMAND='fd --type f'


4. 使用内置搜索菜单快捷键
配置完成后，你可以直接在终端使用以下“搜索菜单”快捷键： 
CTRL-R：调出命令历史搜索菜单。
CTRL-T：调出当前目录下的文件搜索菜单。
ALT-C：调出目录搜索菜单，选中后直接切换（cd）到该目录。
模糊补全：输入命令（如 vim 或 cd）后接 ** 再按 TAB，可触发针对该命令的搜索补全。 
5. 进阶：配置预览窗口
若想在搜索菜单旁实时查看文件内容，可配合 bat 工具： 
bash
# 搜索文件时按预览（需安装 bat）
export FZF_CTRL_T_OPTS="--preview 'batcat --color=always --line-range :500 {}'"
