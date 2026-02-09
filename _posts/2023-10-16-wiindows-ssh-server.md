---
layout: post
title: win10配置ssh服务
tags: [linux]
---

### 下载安装Openssh Server
系统设置->管理可选功能->添加Openssh Server
### 配置防火墙规则(1809以上可跳过)
控制面板->系统和安全->管理工具->高级安全Windows Defender 防火墙->入站规则->新建规则，类型端口，TCP协议，本地端口22->允许连接（全部）
### 启动Openssh Server，并将服务加入开机自启
管理工具->服务->列表中OpenSSH Authentication与OpenSSH SSH Server，分别启动。右键菜单属性中将启动类型改为自动即实现开机自启。
``` shell

```

