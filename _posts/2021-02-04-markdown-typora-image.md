---
layout: post
title: typora图片自动上传
tags: [linux]
---

typora是所见即所得的markdown编辑器，受到开发人员的普遍欢迎。

#### 偏好设置

打开文件偏好设置，按下面图片设置



下载或更新

#### 获取token

登录或注册https://sm.ms/home/获取token

#### 编辑配置文件

在指定位置输入token

~/.picgo/config.json

```
{
	"picBed":{
		"uploader":"smms",
		"smms":{
			"token":"your token"
		}
	},
	"picgoPlugins":{}
}
```

#### 验证上传