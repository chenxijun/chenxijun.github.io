---
layout: mypost
title: 你好，世界！
categories: [技术,ffmpeg]
---

## 绪言

近日在上网课，便想尝试把网课回放扒下来备用，结果是不好下载的m3u8格式。网上看了很多关于m3u8下载的文章，大多比较麻烦，我今日试验出一种较为方便的方式，运用FFmpeg直接转码m3u8视频为mp4格式视频，简单易用。

下载安装ffmpeg、获取m3u8链接或文件的方式不再赘述，见他人博客。

![一个典型的m3u8文件](classic-m3u8.png)

## 链接下载

如果有指向m3u8的链接，则有最为简单的下载方式：

```bash
ffmpeg -i [视频链接] [保存文件]
```

例如，我需要保存抓包下来的网课至`test.mp4`，则进行命令

```bash
ffmpeg -i 'http://link.to.your/video.m3u8' test.mp4
```

![shell](shell.png)