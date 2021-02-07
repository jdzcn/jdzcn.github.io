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

上面命令指定同步时，排除所有文件，仅同步txt文件。

### 三、参考

- [rsync用法教程](http://www.ruanyifeng.com/blog/2020/08/rsync.html)