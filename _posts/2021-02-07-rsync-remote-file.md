---
layout: post
title: Linux增量同步工具rsync常用方法
tags: [linux]
---

rsync是linux中的文件同步工具，用于本地之间或本地远程文件夹增量同步，在更新网站和备份数据时非常方便。

### 一、安装

```shell
sudo apt install rsync
```

### 二、用法

#### 基本用法

```shell
$ rsync -av source [user@host:]destination
```

[user@host:】为可选远程主机，默认使用ssh协议。目标目录destination如果不存在，rsync 会自动创建。执行上面的命令后，源目录source被完整地复制到了目标目录destination下面，即形成了destination/source的目录结构。

如果只想同步源目录source里面的内容到目标目录destination，则需要在源目录后面加上斜杠。

```shell
$ rsync -av source/ [user@host:]destination
```

#### --delete 参数

```shell
rsync -av --delete source/ destination
```

谨慎使用。--delete参数将删除源目录没有、而目标目录存在的文件，使目标目录成为源目录的镜像副本。

#### --exclude参数

有时，我们希望同步时排除某些文件或目录，这时可以用--exclude参数指定。

```shell
$ rsync -av --exclude '.*' source/ destination
```

#### --include参数

```shell
$ rsync -av --include="*.txt" --exclude='*' source/ destination
```
####模拟运行（Dry Run）
在执行涉及大量删除或复杂路径的操作前，先看看会发生什么，而不实际修改文件。
用法：加入 -n 或 --dry-run。
建议：每次使用 --delete 前都跑一次这个，防止误删。


####快速检查文件差异
如果你只想知道哪些文件不同，而不想实际传输，可以配合 -i (itemize-changes) 参数。
用法：rsync -avin ...
输出含义：
>f+++++++：新文件。
>f..t......：时间戳不同。
>f.s......：文件大小不同。



#### sync dir to termux(android phone)
rsync -avzP --inplace --no-perms --no-owner --no-group -e "ssh -p 8022" posts/ u0_a228@192.168.1.53:/sdcard/Download/posts/ 
####从手机拉取照片到电脑，跳过已经存在的文件
rsync -avzP --ignore-existing -e "ssh -p 8022" u0_a228@192.168.1.53:/sdcard/DCIM/Camera/ ~/Pictures/PhoneBackup/

上面命令指定同步时，排除所有文件，仅同步txt文件。

### 三、参考

- [rsync用法教程](http://www.ruanyifeng.com/blog/2020/08/rsync.html)