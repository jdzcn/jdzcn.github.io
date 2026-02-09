---
layout: post
title: rclone 管理云盘
tags: [linux]
---

### Install
``` shell
sudo apt install rclone
or
curl https://rclone.org/install.sh | sudo bash
```
### Config
``` shell
rclone config
```
### Sync

``` shell
#test sync
rclone sync --dry-run md:paint/ ~/paint/
#show progress
rclone sync -P md:paint/ ~/paint/
```
### Mount
``` shell
rclone mount md: ~/onedrive/ --vfs-cache-mode full
rclone mount ali:/ W: --vfs-cache-mode writes --cache-dir E:\aliyun --network-mode --no-check-certificate --default-permissions --header "Referer:https://www.aliyundrive.com/" --vfs-read-chunk-size-limit 1G --vfs-read-chunk-size 64M --dir-cache-time 12h --buffer-size 32M
```

### Reference
- [rclone.org](https://rclone.org/)