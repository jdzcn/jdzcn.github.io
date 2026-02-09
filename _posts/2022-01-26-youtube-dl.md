---
layout: post
title: How to use youtube-dl 
tags: [linux]
---


### Use
``` shell
#List all format
youtube-dl -F url
youtube-dl -f best url

#use proxy
youtube-dl --proxy socks5://127.0.0.1:1080 url

#select playlist
youtube-dl --playlist-start 2 --playlist-end 5 url
youtube-dl --playlist-items 1,3-5 url

#only audio mp3
youtube-dl -x --audio-format mp3 url

#指定长度
youtube-dl -x --audio-format mp3 --postprocessor-args "-ss 00:00:53 -to 00:01:20" https://youtu.be/NjTT5_RSkw4
or
ffmpeg -i input_video.mp4 -ss 00:01:00 -to 00:03:00 -c copy output_video.mp4
```
### Config
~/.config/youtube-dl/config
``` shell
# Use this proxy
--proxy 127.0.0.1:3128

# Save all videos under Movies directory in your home directory
-o ~/Movies/%(title)s.%(ext)s
```


### Reference
- [youtube-dl.org](https://github.com/ytdl-org/youtube-dl)