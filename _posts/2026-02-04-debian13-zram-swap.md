---
layout: post
title: debian 配置zram和交换文件
tags: [linux]
---

在 Debian 13 上配置 **zram**（高性能压缩内存）+ **Btrfs Swapfile**（磁盘保底）的组合是目前 Linux 桌面和服务器的最佳实践。

以下是完整的配置流程，我们将确保 zram 的优先级高于磁盘上的 Swap，从而兼顾速度与稳定性。

---

## 第一步：安装并配置 zram-tools

虽然可以手动写脚本，但 Debian 推荐使用 `zram-tools`，它简单且集成度高。

1. **安装工具：**
```bash
sudo apt update
sudo apt install zram-tools

```


2. **修改配置文件：**
编辑 `/etc/default/zramswap`：
```bash
sudo nano /etc/default/zramswap

```


建议配置如下（取消相关行的注释并修改）：
* `ALLOCATION_STRATEGY=adaptive` (智能分配)
* `PERCENTAGE=50` (占用物理内存的 50% 作为压缩池)
* `PRIORITY=100` (**关键：** 确保这个值比普通 swap 高，建议设为 100)


3. **重启服务：**
```bash
sudo systemctl restart zramswap

```



---

## 第二步：在 Btrfs 上创建 Swap File

在 Btrfs 上不能直接用 `dd` 创建 swap，必须使用 Btrfs 专用的命令来禁用写时复制（CoW）。

1. **创建 swap 文件：**
这里以创建一个 **4G** 的文件为例（可以根据磁盘空间调整）：
```bash
sudo btrfs filesystem mkswapfile --size 4G /swapfile

```


2. **启用 swap：**
```bash
sudo swapon /swapfile

```


3. **永久挂载：**
编辑 `/etc/fstab`：
```bash
sudo nano /etc/fstab

```


在文件末尾添加一行（**注意 prio=-2，确保它优先级最低**）：
```text
/swapfile  none  swap  defaults,prio=-2  0  0

```



---

## 第三步：验证优先级

这是最重要的一步，我们要确认系统会先压榨 zram，只有 zram 满了才会往硬盘里写数据。

运行命令：

```bash
swapon --show

```

**预期的输出结果应该是这样的：**
| NAME | TYPE | SIZE | USED | PRIO |
| :--- | :--- | :--- | :--- | :--- |
| /dev/zram0 | partition | 8G (举例) | 0B | 100 |
| /swapfile | file | 4G | 0B | -2 |

* **PRIO 为正数 (100)** 的 zram 会被优先使用。
* **PRIO 为负数 (-2)** 的磁盘文件只负责“兜底”。

---

## 进阶微调：调整 Swappiness

为了让系统更积极地使用 zram 而不是过早触发磁盘 IO，可以调整内核参数：

1. 编辑配置文件：
```bash
sudo nano /etc/sysctl.d/99-zram.conf

```


2. 添加以下内容：
```text
# 增加 swappiness，让内核更倾向于将不常用的内存压缩进 zram，而不是直接删缓存
vm.swappiness = 100

```


3. 应用配置：
```bash
sudo sysctl --system

```



---

### 总结

你现在的系统拥有了一个**两级交换机制**：

1. **第一级 (zram)**：极速压缩，CPU 换空间。
2. **第二级 (swapfile)**：低速保底，防止内存溢出导致程序崩溃。

