---
layout: post
title: Hyper-V虚拟机桌面快捷方式创建方法
tags: [win]
---


# Hyper-V虚拟机桌面快捷方式创建方法1（纯文本版）
## 核心操作步骤
1. 桌面空白处右键 → 新建 → 快捷方式
2. 输入目标路径（替换「你的虚拟机名称」，名称含空格必须加英文双引号）
3. 下一步，自定义快捷方式名称（如“XX虚拟机”），点击完成
4. （可选，推荐）右键生成的快捷方式 → 属性
   - 切换到「快捷方式」标签 → 高级 → 勾选「以管理员身份运行」→ 确定
   - （可选）点击「更改图标」，可选择系统图标或自定义ico文件

## 目标路径模板（直接复制替换）
```
C:\Windows\System32\vmconnect.exe localhost "你的虚拟机名称"
```

## 示例（虚拟机名称：Debian13-HyperV）
```
C:\Windows\System32\vmconnect.exe localhost "Debian13-HyperV"
```

## 进阶：双击一键启动+连接（目标路径模板）
### CMD版
```
cmd /c "start vmconnect.exe localhost "你的虚拟机名称""
```
### PowerShell版（更稳定）
```
powershell -Command "Start-Process vmconnect.exe -ArgumentList 'localhost', '你的虚拟机名称'"
```

## 关键注意事项
1. 虚拟机名称需与Hyper-V管理器中完全一致（含大小写、空格、特殊字符）
2. 本地Hyper-V用localhost，远程Hyper-V将localhost替换为远程主机名/IP
3. 必须以管理员身份运行，避免连接/启动权限报错
4. 快捷方式创建后，可直接双击打开虚拟机控制台（未开机可在控制台内点启动）