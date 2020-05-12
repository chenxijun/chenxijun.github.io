---
layout: mypost
title: Android通过ADB备份获取应用数据
categories: [技术,Android]
---

## 起因

一位同学的手机没法root，但是想获取应用数据，于是便有了这个需求。

## 方法

- 首先获取Platform Tools中的ADB：[下载地址](https://developer.android.google.cn/studio/releases/platform-tools)
- 在命令行中输入`adb devices`确认设备连接

> `adb backup [-f <file>] [-apk|-noapk][-shared|-noshared] [-all] [-system|nosystem] [<packages...>]`
> 
> `-f <file>`：用这个来选择备份文件存储在哪里
> 
> `-apk|-noapk`：这个决定是否在备份里包含apk或者仅仅只备份应用数据
> 
> `-shared|-noshared`：这个参数用于决定是否备份设备共享的SD card内容，默认是-noshare
> 
> `-all`：如果你不是备份某个应用，使用-all备份整个系统
> 
> `-system|-nosystem`：这个参数决定-all标签是否包含系统应用，默认的是-system
> 
> `<packages...>`：备份特定应用。

- 按上面规则输入`adb backup`命令备份
- 下载ab文件解包工具（需要~~Jvav~~ Java）：[Github地址](https://github.com/nelenkov/android-backup-extractor)
- 使用`java -jar abe.jar unpack <backup.ab> <backup.tar> [password]`解包
- 即可在输出中获得数据