---
layout: post
title: debian btrfs分区常用操作
tags: [linux]
---


### 1. 基础查询 (查看空间与结构)

```bash
# 查看所有子卷及其 ID (最常用)
sudo btrfs subvolume list /

# 查看磁盘真实空间占用 (比 df -h 准)
sudo btrfs filesystem usage /

# 查看每个子卷的独占空间 (需开启 quota)
sudo btrfs qgroup show -p /

# 查看文件系统的 UUID 等详细信息
sudo btrfs filesystem show

```

### 2. 快照管理 (创建与删除)

```bash
# 创建一个【只读】快照 (推荐用于备份)
sudo btrfs subvolume snapshot -r / /.snapshots/$(date +%Y%m%d)_backup

# 创建一个【可写】快照
sudo btrfs subvolume snapshot / /.snapshots/$(date +%Y%m%d)_writable

# 删除一个子卷或快照 (由于你之前遇到 Directory not empty，建议用 -c)
sudo btrfs subvolume delete -c /.snapshots/snapshot_name

```

### 3. 手动回滚“手术” (在挂载顶层 ID 5 后执行)

```bash
# 假设你已经 mount /dev/sdXY /mnt -o subvolid=5

# 第一步：挪走当前的坏系统
mv /mnt/@ /mnt/@_bad_$(date +%m%d)

# 第二步：从快照克隆出新的根系统
btrfs subvolume snapshot /mnt/@snapshots/target_snapshot /mnt/@

# 第三步：确保克隆出的新根是可写的 (如果是从只读快照克隆的)
btrfs property set -ts /mnt/@ ro false

# 第四步：将其设为默认启动子卷
btrfs subvolume set-default $(btrfs subvolume list /mnt | grep 'path @$' | awk '{print $2}') /mnt

```

### 4. 属性管理 (只读/可写转换)

```bash
# 查看只读属性
sudo btrfs property get /.snapshots/snapshot_name

# 将只读快照转为可写
sudo btrfs property set -ts /.snapshots/snapshot_name ro false

# 将子卷转为只读
sudo btrfs property set -ts /.snapshots/snapshot_name ro true

```

### 5. 维护与修复

```bash
# 开启配额功能 (查看 qgroup 空间前必须执行)
sudo btrfs quota enable /

# 整理碎片 (针对特定目录，如虚拟机或数据库)
sudo btrfs filesystem defragment -r /path/to/dir

# 启动全盘数据校验 (后台运行，检查静默数据损坏)
sudo btrfs scrub start /
# 查看校验进度
sudo btrfs scrub status /

```

---

### 💡 建议

你可以把下面这行加入你的 `~/.bashrc`，以后输入 `blist` 就能看到清晰的子卷列表：
`alias blist='sudo btrfs subvolume list /'`

