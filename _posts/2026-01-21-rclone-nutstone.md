---
layout: post
title: rclone操作坚果云
tags: [linux]
---


# 一键创建坚果云远程
rclone config create jianguoyun webdav \
  url "https://dav.jianguoyun.com/dav/" \
  vendor other \
  user "你的坚果云账号" \
  pass "坚果云应用密码"

# 测试连接
rclone lsd jianguoyun:

# 单向同步（国内直连稳）
rclone sync /local/path jianguoyun:/remote/path --progress --delete-after

# 挂载（无需代理，流畅）
rclone mount jianguoyun:/remote/path /mnt/jianguoyun \
  --allow-non-empty \
  --vfs-cache-mode writes