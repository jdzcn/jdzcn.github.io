---
layout: post
title: ffmpeg提取音频
tags: [linux]
---

### 提取为MP3

```
for f in *.mp4; do
    ffmpeg -i "$f" -vn -c:a libmp3lame -q:a 4 "${f%.mp4}.mp3"
done

```

### 快速提取无损音频

```
for f in *.mp4; do
    ffmpeg -i "$f" -vn -c:a copy "${f%.mp4}.m4a"
done
```
