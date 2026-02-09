---
layout: post
title: rclone同步infini命令格式
tags: [linux]
---

rclone sync d:\paint\ infini:paint /
  --dry-run \          # 先测试，确认无误后删除
  --tpslimit 2 \
  --transfers 2 \
  --bwlimit 5M \
  --sanitize \
  --timeout 30s \      # 适配海外服务器延迟
  --log-file infini_sync.log
  
  rclone check /本地路径 123pan:目标路径 --one-way --log-file check.log