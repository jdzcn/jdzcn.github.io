---
layout: post
title: qt应用程序在linux中打包部署
tags: [linux]
---


1.将项目（比如store）在linux中用qt creator编译运行Release版本；

2.新建文件夹store,将上述可执行文件拷入；

3.创建两个空的脚本pack.sh和Cpack.sh,内容如下：

``` shell

#!/bin/sh  
exe="ChineseChess" #你需要发布的程序名称
des="/home/xmuli/Desktop/temp/qwer" #创建文件夹的位置
deplist=$(ldd $exe | awk  '{if (match($3,"/")){ printf("%s "),$3 } }')  
cp $deplist $des

```
4.再创建一个store.sh文件（名称和可执行文件一样），内容如下：

``` shell


#!/bin/sh  
appname=`basename $0 | sed s,\.sh$,,`  
dirname=`dirname $0`  
tmp="${dirname#?}"  
if [ "${dirname%$tmp}" != "/" ]; then  
dirname=$PWD/$dirname  
fi  
LD_LIBRARY_PATH=$dirname  
export LD_LIBRARY_PATH  
$dirname/$appname "$@"
```
5.打开终端，执行如下：

./pack.sh
会自动将所有依赖复制到文件夹

6.将此目录打包即可。



