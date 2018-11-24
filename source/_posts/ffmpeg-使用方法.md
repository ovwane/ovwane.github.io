---
title: ffmpeg 使用方法
date: 2018-10-14 09:24:42
tags:
---



file.txt

```file
file '1.mp4'
file '2.mp4'
file '3.mp4'
```

合并视频

```shell
ffmpeg -f concat -i file.txt -c copy output.mp4
```



## 参考

[FFmpeg](https://www.ffmpeg.org/)

[ffmpeg合并视频文件 官方文档](https://moejj.com/ffmpeghe-bing-shi-pin-wen-jian-guan-fang-wen-dang/)