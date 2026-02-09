---
layout: post
title: linux list service and application
tags: [linux]
---
application
``` shell
dpkg -l | grep edge
apt list

```
service
``` shell
  642  systemctl 
  643  systemctl --type=service
  644  systemctl --type=service --state=running
```
uninstall

``` shell
dpkg -r modiri
apt remove modiri
apt autoremove
```
