---
layout: post
title: rclone同步坚果云命令格式
tags: [linux]
---



rclone sync d:/posts jgy:posts/
  --dry-run \         
  --tpslimit 1 \
  --transfers 2 \
  --checkers 2
  --bwlimit 1M \
  --sanitize \
  --max-size 400M \
  --log-file jgy_sync.log  # 记录日志，排查流量超限/失败文件
  
  rclone check /本地路径 123pan:目标路径 --one-way --log-file check.log
  
  # 查找并删除0字节文件
find /本地路径 -type f -size 0 -delete
# 查找含违规字符的文件并输出（手动改名）
find /本地路径 -type f -path "*[\\/:*?\"<>|]*" -print > bad_files.txt
# 检查长路径（建议≤400字符）
find /本地路径 -type f | awk '{if(length($0)>400) print $0}' > long_paths.txt

检查文件夹文件数量
# Linux / macOS
find /path/to/folder -type f | wc -l

# Windows PowerShell
(Get-ChildItem -File -Recurse "D:\path\to\folder").Count


rclone sync /你的文件夹 云盘标识:目标路径 \
  --progress \          # 显示同步进度，直观看到文件传输状态
  --transfers 4 \       # 并发传输数（默认4，降为2-4更稳）
  --checkers 8 \        # 校验文件数（降为8，减少API请求）
  --tpslimit 3 \        # 每秒最多3个API请求（避免429限流）
  --bwlimit 5M \        # 限速5MB/s，防止带宽跑满触发风控
  --webdav-list-chunk 700 \  # 仅WebDAV云盘（如坚果云）：单次列表请求700个文件，适配750个上限
  --retries 3 \         # 失败重试3次，避免网络波动导致中断
  --low-level-retries 10 \  # 底层请求重试10次，提升容错
  --log-file full_sync.log \ # 记录所有日志，方便排查失败文件
  --sanitize \          # 自动清理特殊字符，避免文件名报错
  --checksum            # 基于校验和同步，避免时间戳不一致导致重复传输