---
title: Xposed 教程
date: 2019-01-04 09:23:41
tags:
---

## Xposed

[Xposed Module Repository](https://repo.xposed.info/)

### **下载Xposed Installer**

https://repo.xposed.info/module/de.robv.android.xposed.installer

### 安装Xposed

> Android 6.0

**刷机安装 xposed.zip**

 `xposed-v89-sdk23-x86`

**安装apk**

`XposedInstaller_3.1.5.apk`



## 模块

- XPrivacy



## 模拟位置

- 模拟位置 (FakeLocation)](https://repo.xposed.info/module/com.rong.xposed.fakelocation)
- [Fake My GPS](https://repo.xposed.info/module/com.fakemygps.android)
- https://github.com/hilarycheng/xposed-gps
- https://github.com/qdk0901/FakeXX
- https://github.com/mark-ypq/GPSHook

[Android开发 ：详细讲解如何使用Xposed拦截修改地理位置信息](https://blog.csdn.net/qq_34149335/article/details/81329007) ok，看这篇文章搞定了高德地图。

[Android之基于Xposed的模拟位置模块实现](https://www.jianshu.com/p/796e94d8af31)

[Hook实现Android 微信，陌陌 ，探探位置模拟](https://blog.csdn.net/u012889434/article/details/61921933)

[用Xposed框架拦截微信、人人、QQ等LBS应用的当前位置](https://blog.csdn.net/u011000290/article/details/46925713)

参考这篇[基于Xposed框架Hook定位功能来破解QQ的LBS红包](http://littlerich.top/2017/01/17/%E5%9F%BA%E4%BA%8EXposed%E6%A1%86%E6%9E%B6Hook%E5%AE%9A%E4%BD%8D%E5%8A%9F%E8%83%BD%E6%9D%A5%E7%A0%B4%E8%A7%A3QQ%E7%9A%84LBS%E7%BA%A2%E5%8C%85/)



## Xiaomi Mi4

[小米4 Android 6.0 版本 Root 并安装 Xposed 框架攻略](http://prototypez.github.io/2016/05/16/root-and-install-Xposed-framework-on-XiaoMi4-with-Android-M/)

系统版本

> cancro_global_images_V9.5.7.0.MXDMIFA_20180815.0000.00_6.0_global_bbfc0f3951.tgz
>
> miui_MI3WMI4WGlobal_8.3.1_b992dfbc34_6.0.zip



[TWRP](https://twrp.me/xiaomi/xiaomimi3.html)

>twrp-3.3.0-0-cancro.img

```shell
fastboot flash recovery twrp-3.3.0-0-cancro.img
```



[[OFFICIAL] Xposed for Lollipop/Marshmallow/Nougat/Oreo [v90-beta3, 2018/01/29]](https://forum.xda-developers.com/showthread.php?t=3034811)

> XposedInstaller_3.1.5.apk
>
> xposed-v89-sdk23-arm.zip

1. 使用 twrp 安装 xposed-v89-sdk23-arm.zip。

2. 安装 XposedInstaller_3.1.5.apk



## 参考

[Android 神器 xposed 框架使用指南](https://blog.csdn.net/fuchaosz/article/details/53143216)

[Xposed 使用手册](https://www.jianshu.com/p/655a07b687ec)

[不需要 Root，也能用上强大的 Xposed 框架：VirtualXposed](https://sspai.com/post/44447)

[Android 系统上的 Xposed 框架中都有哪些值得推荐的模块？](https://www.zhihu.com/question/22063862)

[新手不要再被误导！这是一篇最新的Xposed模块编写教程](https://www.freebuf.com/articles/terminal/189021.html)

[【Xposed模块开发入门】真·第一课](https://www.52pojie.cn/thread-688466-1-1.html)

[萌新跟我来入门Hook技术](https://www.52pojie.cn/thread-682743-1-1.html)

[[OFFICIAL] Xposed for Lollipop/Marshmallow/Nougat/Oreo [v90-beta3, 2018/01/29]](https://forum.xda-developers.com/showthread.php?t=3034811)