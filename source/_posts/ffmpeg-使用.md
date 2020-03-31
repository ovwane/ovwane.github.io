---
title: FFmpeg 使用
date: 2018-10-14 09:24:42
tags:
---

# [FFmpeg](https://www.ffmpeg.org/)

下载源码

```shell
git clone https://git.ffmpeg.org/ffmpeg.git
```



编译

```shell
./configure --help
```

```shell
./configure --list-decoders
./configure --list-filters
```

```shell
./configure --prefix=/usr/local/ffmpeg/4.1.1 --enable-shared --enable-pthreads --enable-version3 --enable-hardcoded-tables --enable-avresample --cc=clang --host-cflags='-I/Library/Java/JavaVirtualMachines/openjdk-11.0.2.jdk/Contents/Home/include -I/Library/Java/JavaVirtualMachines/openjdk-11.0.2.jdk/Contents/Home/include/darwin' --host-ldflags= --enable-ffplay --enable-gnutls --enable-gpl --enable-libaom --enable-libbluray --enable-libmp3lame --enable-libopus --enable-librubberband --enable-libsnappy --enable-libtesseract --enable-libtheora --enable-libvorbis --enable-libvpx --enable-libx264 --enable-libx265 --enable-libxvid --enable-lzma --enable-libfontconfig --enable-libfreetype --enable-frei0r --enable-libass --enable-libopencore-amrnb --enable-libopencore-amrwb --enable-libopenjpeg --enable-librtmp --enable-libspeex --enable-videotoolbox --disable-libjack --disable-indev=jack --enable-libaom --enable-libsoxr
```

>ffmpeg n3.3.5-2-g9992aceefe



帮助信息

```shell
ffmpeg --help
```



## 常用命令

### 基本信息查询命令

```shell
-version
-demuxers
-muxers
-devices
-codecs
-decoders
-encoders
-bsfs # 显示比特流 filter
-formats
-protocols
-filters
-pix_fmts
-sample_fmts
-layouts # 显示 channel 名称（声道）
-colors
```



### 录制命令

#### 录视频命令

```shell
ffmpeg -f avfoundation -i 1 -r 30 out.yuv
```

>-f：指定使用 avfoundation 采集数据
>
>-i：指定从哪儿采集数据，它是一个文件索引号 `ffmpeg -f avfoundation -list_devices true -i ""`
>
>-`r`：指定帧率（15帧）



播放

```shell
ffplay -s 3360x2100 -pix_fmt uyvy422 out.yuv
```

> -s：视频大小
>
> -pix_fmt：视频格式



#### 录音命令

```shell
ffmpeg -f avfoundation -i :0 out.wav
```

> :0 代表音频设备 `[0] Built-in Microphone`
>
> 1: 代表视频设备 `[1] Capture screen 0`



播放

```shell
ffplay out.wav
```



### 分解与复用

#### 多媒体格式转换

```shell
ffmpeg -i out.mp4 -vcodec copy -acodec copy out.flv
```

>-i：输入文件
>
>-vcodec copy：视频编码处理方式
>
>-acodec copy：音频编码处理方式



```shell
ffmpeg -i out.mp4 -an -vcodec copy out.h264
```

> -an 不要音频



```shell
ffmpeg -i out.mp4 -acodec copy -vn out.aac
```

> 不能使用



### 处理原始数据

#### 提取 YUV 数据

```shell
ffmpeg -i input.mp4 -an -c:v rawvideo -pix_fmt yuv420p out.yuv
```

>-pix_fmt yuv420p ：像素格式



播放

```shell
ffplay -s 1680x1050 out.yuv
```



#### 提取 PCM 数据

```shell
ffmpeg -i out.mp4 -vn -ar 44100 -ac 2 -f s16le out.pcm
```

>-ar ：rate 采样率
>
>-ac 2：channel 2
>
>-f s16le：存储方式



播放

```shell
ffplay -ar 44100 -ac 2 -f s16le out.pcm
```



### 滤镜命令

> 加水印，去水印，画中画

解压缩 decoder，解码后的数据帧。所有的滤镜处理都是对解码后的数据帧进行处理。



```shell
ffmpeg -i in.mov -vf crop=in_w-200:in_h-200 -c:v libx264 -c:a copy out.mp4
```

> -vf crop=in_w-200:in_h-200 视频滤镜，宽度减少200，高度减少200
>
> -c:v libx264：视频编码
>
> -c:a copy：音频
>
> https://www.cnblogs.com/yongfengnice/p/7095846.html



### 裁剪与合并

> 音视频的裁剪与合并



#### 裁剪

```shell
ffmpeg -i in.mp4 -ss 00:00:00 -t 10 out.ts
```

> -ss 开始裁剪
>
> -t 10 裁剪10秒



#### 合并

```shell
ffmpeg -f concat -i input.txt out.flv
```

>-f concat：对后面的文件进行拼接
>
>-i input.txt：内容为`file filename.ts`格式

file.txt

```file
file '1.mp4'
file '2.mp4'
file '3.mp4'
```

```shell
ffmpeg -f concat -i file.txt -c copy output.mp4
```



降低分辨率

```bash
ffmpeg -i input.mp4 -strict -2 -vf scale=-1:720 output.mp4
```

>720p
>
>使用FFmpeg自带的aac音频编码要带上-strict -2 
>
> [[3][使用] FFmpeg调整文件尺寸 | Sanchi](https://sanchi.forkroad.xyz/post/2018-03-27-ffmpeg-3-%E4%BD%BF%E7%94%A8-FFmpeg%E8%B0%83%E6%95%B4%E6%96%87%E4%BB%B6%E5%B0%BA%E5%AF%B8.html) 



```
ffmpeg -i "input.mp4" -crf 24 -vf scale=-1:720 output.mp4
```

>-crf 指定刷新率



### 图片/视频互转

#### 视频转图片

```shell
ffmpeg -i in.flv -r 1 -f image2 image-%3d.jpeg
```

> `-r` 1:帧率，每秒钟转出一张图片
>
> -f image2：转换的格式
>
> %3d：三个数字



#### 图片转视频

```shell
ffmpeg -i image-%3d.jpeg out.mp4
```



### 获取视频信息

```shell
ffmpeg -i vedio.mp4
```



### 从视频中提取音频

> 从视频文件中提取音频并保存为 mp3

```
ffmpeg -i input.mp4 -f mp3 output.mp3
ffmpeg -i 1.mp4 -vn -vcodec copy 1.m4a
```

> 如果需要可以在中间加上 `-ar 44100 -ac 2 -ab 192` 系数，表示采样率 44100 ，通道 2 立体声，码率 192 kb/s.
>
> -vn 跳过视频。



重新编码

[Non-monotonous DTS in output stream previous current changing to This may result in incorrect timestamps in the output file](https://stackoverflow.com/questions/53021266/non-monotonous-dts-in-output-stream-previous-current-changing-to-this-may-result)

```shell
ffmpeg -safe 0 -f concat -segment_time_metadata 1 -i file.txt -vf select=concatdec_select -af aselect=concatdec_select,aresample=async=1 out.mp4
```



比特率不一样，转码再合并

```bash
ffmpeg -i input1.mp4 -c copy -bsf:v h264_mp4toannexb -f mpegts input1.ts
ffmpeg -i input2.flv -c copy -bsf:v h264_mp4toannexb -f mpegts input2.ts
ffmpeg -i input3.flv -c copy -bsf:v h264_mp4toannexb -f mpegts input3.ts
ffmpeg -i "concat:input1.ts|input2.ts|input3.ts" -c copy -bsf:a aac_adtstoasc -movflags +faststart output.mp4
```



## 处理流程

输入文件-(demuxer)解复用->编码数据包-(decoder)->解码后数据帧-(encoder)->编码数据包-(muxer)复用->输出文件



## 参考

[ffmpeg Documentation](https://ffmpeg.org/ffmpeg.html) 

[ffmpeg合并视频文件 官方文档](https://moejj.com/ffmpeghe-bing-shi-pin-wen-jian-guan-fang-wen-dang/)

[video不能播放mp4的问题（一）](<https://www.jianshu.com/p/aa5ba6967f46>)

[ffmpeg 入门笔记 ](<http://einverne.github.io/post/2015/12/ffmpeg-first.html>)