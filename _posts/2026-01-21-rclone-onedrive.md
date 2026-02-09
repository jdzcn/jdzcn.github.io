---
layout: post
title: rclone操作onedrive
tags: [linux]
---


# 一键创建 OneDrive 远程（按提示完成 OAuth 授权）
rclone config create onedrive onedrive \
  drive_id "your-drive-id" \  # 授权后自动获取，可留空
  drive_type "personal" \     # 商业版填 business，世纪互联填 onedrivebusiness
  client_id "" \              # 可选，用官方客户端ID
  client_secret ""

rclone config create onedrive onedrive drive_id " drive_type "personal" client_id "" client_secret ""

# 测试连接
rclone lsd onedrive:

# 单向同步（本地→云端，删除云端多余）
rclone sync /local/path onedrive:/remote/path --progress --delete-after

# 双向同步（首次 --resync，后续不加）
rclone bisync onedrive:/remote/path /local/path --resync --size-only
rclone bisync onedrive:/remote/path /local/path --size-only

# 挂载（需 fuse，国内建议加代理）
rclone mount onedrive:/remote/path /mnt/onedrive \
  --allow-non-empty \
  --vfs-cache-mode writes \
  --vfs-cache-max-age 24h \
  --proxy socks5://127.0.0.1:1080  # 国内必加