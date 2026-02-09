---
layout: post
title: imagemagick常用命令
tags: [linux]
---

当前目录文件生成缩略图
``` shell
mogrify  -format gif -path thumbs -thumbnail 100x100 *.jpg
```

旋转图片
``` shell
convert 1.jpg -rotate 90 1.jpg
#改变大小
 convert 1.jpg -resize 300x300 2.jpg
```

