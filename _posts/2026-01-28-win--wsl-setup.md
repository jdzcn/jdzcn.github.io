---
layout: post
title: win10开启wsl
tags: [win]
---


@echo off
title Win10 LTSC 21H2 WSL2 自动配置脚本
echo ======================================================
echo 正在检查管理员权限...
net session >nul 2>&1
if %errorLevel% neq 0 (
    echo [错误] 请右键点击本脚本，选择“以管理员身份运行”！
    pause
    exit /b
)

echo [1/4] 正在开启 WSL 可选功能...
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

echo [2/4] 正在开启虚拟机平台组件...
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

echo [3/4] 检查 WSL 内核状态...
echo 请在弹出的浏览器窗口中下载并安装最新的 WSL2 MSI 程序。
echo 下载完成后请运行安装包，然后回到此窗口。
start https://github.com

echo ======================================================
echo [4/4] 配置完成！
echo ------------------------------------------------------
echo 重要提示：
echo 1. 您必须【重启电脑】以上功能才会生效。
echo 2. 重启后，请将 Debian 13 的 rootfs 放在 D 盘或 E 盘。
echo 3. 使用以下命令导入：
echo    wsl --import Debian13 C:\WSL_Path  C:\Downloads\rootfs.tar.xz
echo ======================================================
pause
