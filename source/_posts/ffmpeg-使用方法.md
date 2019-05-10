---
title: ffmpeg 使用方法
date: 2018-10-14 09:24:42
tags:
---

### 获取视频信息

```shell
ffmpeg -i vedio.mp4
```



### 从视频中提取音频

> 从视频文件中提取音频并保存为 mp3

```
ffmpeg -i input.mp4 -f mp3 output.mp3
```

> 如果需要可以在中间加上 `-ar 44100 -ac 2 -ab 192` 系数，表示采样率 44100 ，通道 2 立体声，码率 192 kb/s.



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



重新编码

[Non-monotonous DTS in output stream previous current changing to This may result in incorrect timestamps in the output file](https://stackoverflow.com/questions/53021266/non-monotonous-dts-in-output-stream-previous-current-changing-to-this-may-result)

```shell
ffmpeg -safe 0 -f concat -segment_time_metadata 1 -i file.txt -vf select=concatdec_select -af aselect=concatdec_select,aresample=async=1 out.mp4
```



## 参考

[FFmpeg](https://www.ffmpeg.org/)

[ffmpeg合并视频文件 官方文档](https://moejj.com/ffmpeghe-bing-shi-pin-wen-jian-guan-fang-wen-dang/)

[video不能播放mp4的问题（一）](<https://www.jianshu.com/p/aa5ba6967f46>)

[ffmpeg 入门笔记 ](<http://einverne.github.io/post/2015/12/ffmpeg-first.html>)