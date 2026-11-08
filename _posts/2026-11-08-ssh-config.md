---
layout: post
title: 配置SSH主机简称
tags: [linux]
---

### 配置主机简称

```
Host mi
    HostName 192.168.1.53
    Port 8022
    User u0_a228
    IdentityFile ~/.ssh/id_ed25519

Host arch
    HostName 192.168.1.75
    Port 22
    User sb
    IdentityFile ~/.ssh/id_ed25519
```
