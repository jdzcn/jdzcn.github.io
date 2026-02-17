---
layout: post
title: redmi k30 5G手机刷Pixel Experience
tags: [android]
---

这份指南为你整理了从零开始到进入系统的完整流程。针对 **Redmi K30 5G (picasso)**，这是一条最稳健的路径。

---

## 🚀 Redmi K30 5G 刷入 Pixel Experience 全流程手册

### 第一阶段：环境准备

1. **解锁 Bootloader (BL)：** 手机绑定小米账号满 168 小时，使用官方解锁工具解锁。
2. **电脑环境：** 安装好 ADB/Fastboot 驱动，下载官方平台工具 (Platform Tools)。
3. **准备文件：**
* 小米官方刷机工具以安装小米驱动
https://xiaomiflashtool.com/
* **Recovery:** OrangeFox (OFRP) 镜像文件。
https://orangefox.download/
* **底包 (Firmware):** 对应版本的最新底层固件（通常为 MIUI 官方提取版）。
https://xmfirmwareupdater.com/#download
* **ROM:** Pixel Experience 官方或稳定版 `.zip` 包。
https://get.pixelexperience.org/
* **工具:** v2rayNG (arm64-v8a) 用于后续激活网络。
https://github.com/2dust/v2rayNG/releases



---

### 第二阶段：刷入 Recovery

1. 手机进入 **Fastboot 模式**（音量下 + 电源键）。
2. 电脑端执行选择执行：
```bash
fastboot devices  # 确认看到序列号
fastboot boot recovery.img  # 临时启动 OrangeFox
fastboot flash recovery d:\recovery.img #正式刷入orangefox recovery
fastboot reboot recovery #重启到recovery模式
```

---

### 第三阶段：核心刷机步骤

进入 OrangeFox 后，严格按以下顺序操作：

1. **清除数据 (Wipe)：**
* 进入 Wipe 菜单，勾选 `Dalvik / ART Cache`、`Cache`、`System`、`Data`，滑动清理。


2. **刷入底包 (Install Firmware)：**
* 找到下载好的底包 `.zip`，点击并刷入。**（不可省略，否则会导致基带不匹配或无法开机）**。


3. **刷入系统 (Install ROM)：**
* 直接找到 `PixelExperience_picasso-xxx.zip` 刷入。


4. **格式化 DATA (Format Data)：**
* 刷完后不要重启！回到 Wipe 菜单。
* 点击 **Format Data**，手动输入 **`yes`** 并确认。这是解决“卡米”和分区加密的关键。


5. **重启：**
* 点击 **Reboot System**。



---

### 第四阶段：初始化与联网

1. **跳过开机向导：**
* 初次启动较慢（约 5-10 分钟）。
* **重要：** 拔掉 SIM 卡，不连 Wi-Fi，一直点“跳过”或“以后设置”，直到进入桌面。


2. **安装联网工具：**
* 电脑端执行：`adb install v2rayNG_2.0.9_arm64-v8a.apk`。


3. **修复 Wi-Fi 感叹号：**
* 打开 v2rayNG，配置好订阅并开启。
* 电脑端执行以下 ADB 命令（解决系统认为无法上网的问题）：


```bash
adb shell settings put global captive_portal_http_url http://connectivitycheck.platform.hicloud.com/generate_204
adb shell settings put global captive_portal_https_url https://connectivitycheck.platform.hicloud.com/generate_204

```



---

### 第五阶段：后续优化建议

* **开启 120Hz：** 设置 -> 显示 -> 刷新率。
* **拍照增强：** 安装 **GCam (谷歌相机)** 移植版。
* **指纹支付：** 若无法使用，在 Magisk 中刷入对应的“指纹支付修复模块”。
* **网络分流：** 在 v2rayNG 设置中开启“分应用代理”，让国内 App 走直连。

---

### ⚠️ 注意事项

* **不要在系统设置内 OTA：** 除非你确信 Recovery 和系统脚本完全匹配。
* **回退 MIUI：** 必须使用小米官方线刷工具 (MiFlash)，不要在 PE 的 Recovery 里直接刷 MIUI。
