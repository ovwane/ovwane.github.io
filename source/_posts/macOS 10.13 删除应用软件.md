---
date: 2018-05-05 20:22:27
---

### 目录

Mac和Windows操作系统有一个很大的不同，大部分App是没有安装程序的，一般下载下来就是一个dmg文件，解开之后直接将App拖到`应用程序`目录下就可以了，所以给人感觉卸载也就是将App拖到`废纸篓`然后清空。如果真这样做就大错特错，即使一个最简单的App都会在下面几个目录中或多或少留下纪念，这些目录一般有：

- ~/Library
- ~/Library/Application Support
- ~/Library/Application Support/CrashReporter
- ~/Library/Caches
- ~/Library/Containers
- ~/Library/LaunchAgents
- ~/Library/Preferences
- ~/Library/PreferencePanes

如果一个程序是通过`pkg`方式安装，或者是在第一次运行时请求管理员权限，那一般还会在如下几个目录中留点纪念：

- /Library
- /Library/Application Support
- /Library/Extensions
- /Library/LaunchAgents
- /Library/LaunchDaemons
- /Library/PreferencePanes
- /Library/Preferences

### 参考

[如何彻底删除不需要的App](https://segmentfault.com/a/1190000005035742)

 

