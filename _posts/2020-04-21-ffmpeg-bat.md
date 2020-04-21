---
layout: mypost
title: FFmpeg相关批处理
categories: [技术,ffmpeg,网课,批处理]
---

## 方便地压缩视频文件

```bat
@ECHO OFF
ECHO ============================================================
ECHO 欢迎使用ffmpeg视频压缩批处理工具
ECHO 您有两种使用方式:
ECHO 1) 直接将待压缩的视频拖放到批处理上
ECHO 2) 在下面输入待压缩视频地址
ECHO.
ECHO 由 尘息Dust 编写
ECHO ============================================================
IF EXIST ffmpeg.exe SET FFMPEG_PATH=ffmpeg.exe
IF EXIST bin/ffmpeg.exe SET FFMPEG_PATH=bin/ffmpeg.exe
IF NOT DEFINED FFMPEG_PATH GOTO NO_PATH_ERR
ECHO 已找到ffmpeg于:%FFMPEG_PATH%
SET RUN_COM=%FFMPEG_PATH%
IF [%1] NEQ [] SET TARGET_PATH=%1
IF NOT DEFINED TARGET_PATH SET /P TARGET_PATH=请输入待压缩视频地址:
SET "RUN_COM="%RUN_COM%" -i "%TARGET_PATH%""
SET /P RES=请输入输出分辨率(如854x480,不输入则保持默认):
IF DEFINED RES SET "RUN_COM=%RUN_COM% -s %RES%"
SET /P FPS=请输入输出帧率(如15,不输入则保持默认):
IF DEFINED FPS SET "RUN_COM=%RUN_COM% -r %FPS%"
SET /P BIT=请输入输出码率(如128k,不输入则保持默认):
IF DEFINED BIT SET "RUN_COM=%RUN_COM% -b %BIT%"
SET /P OUT=请输入输出文件(如output.mp4,不输入则输出output.mp4):
IF NOT DEFINED OUT SET OUT=output.mp4
SET "RUN_COM=%RUN_COM% "%OUT%""
%RUN_COM%
ECHO.
ECHO 命令已完成,正在打开输出文件
%OUT%
PAUSE
EXIT
:NO_PATH_ERR
ECHO 找不到ffmpeg.exe,请检查文件目录
PAUSE
EXIT
```

## 方便地下载腾讯课堂视频

```bat
@ECHO OFF
ECHO ============================================================
ECHO 欢迎使用m3u8转码批处理,可用于下载腾讯课堂视频
ECHO.
ECHO 由 尘息Dust 编写
ECHO ============================================================
ECHO.
ECHO 如果您需要下载腾讯课堂回放视频，请按照下面的步骤执行:
ECHO 1) 在Chrome内核浏览器(如360浏览器等极速浏览器)中打开您的回放视频
ECHO 2) 按下键盘上的F12,会弹出开发者控制台
ECHO 3) 选择标签中的 网络 或者叫 Network
ECHO 4) 按下F5或者直接刷新
ECHO 5) 在一个漏斗图标(可能没有但没关系)附近找到有 Filter 或是 筛选器 字样的输入框
ECHO 6) 在框中输入m3u8
ECHO 7) 正常情况下,下面原来满的列表会只剩两个到四个选项
ECHO 8) 右键任意一个选项,选择 Copy (复制) 然后选择 Copy link address (复制链接地址)
ECHO 9) 在这里下方粘贴(可能是右键)然后回车
ECHO 10) 填写输出视频地址(不填也行)
ECHO 11) 静候完成
ECHO.
:REDO
SET /P ADDR=视频地址:
IF NOT DEFINED ADDR GOTO REDO
SET /P OUTPUT=输出视频地址(默认output.mp4):
IF NOT DEFINED OUTPUT SET OUTPUT=output.mp4
FFMPEG -i "%ADDR%" "%OUTPUT%"
"%OUTPUT%"
ECHO 命令已完成
PAUSE
EXIT
```