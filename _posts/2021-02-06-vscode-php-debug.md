---
layout: post
title: vscode调试php代码
tags: [linux]
---

vscode和php都是我常用的开发工具，下面介绍如何在vscode中调试php代码。

## 1. PHP Debug extension

在vscode中安装php debug扩展

![](https://i.loli.net/2021/02/06/HipavsINjgotBEe.png)

## 2. Xdebug Installation

linux中安装php-xdebug

```shell
sudo apt install php-xdebug
```

## 3. Debugging configuration in VS Code

在vscode在运行菜单添加配置，选择php环境。

### 参考

- [How  to debug php using vscode](https://dev.to/vidamrr/how-to-debug-php-using-visual-studio-code-1gna)