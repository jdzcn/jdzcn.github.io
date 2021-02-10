---
layout: post
title: debian10网络配置
---

debian10安装完成后，网络配置过程如下：
### 获取网卡信息
```shell
ip a
```
获取网卡名称，如enp2s0
### 配置
激活网卡
```shell
allow-hotplug enp2s0
```
DHCP配置
```shell
iface enp2s0 inet dhcp
```
静态网址
```shell
iface enp2s0 inet static
address 192.168.1.2
network 192.168.1.0
netmask 255.255.255.0
broadcast 192.168.1.255
gateway 192.168.1.1
dns-nameservers 192.168.1.1
```
### 重置网卡服务
```shell
systemctl restart networking ifup@enp2s0

ifdown enp2s0
ifup enp2s0
```