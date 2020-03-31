---
title: macOS 制作U盘安装盘
date: 2017-03-15 10:27:21
---

命令格式

```shell
sudo createinstallmedia --volume /Volumes/Install --
applicationpath -- nointeraction
```

```shell
sudo /Volumes/Install\ macOS\ Sierra\ 10.12.3/Install\ macOS\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/Install --applicationpath /Volumes/Install\ macOS\ Sierra\ 10.12.3/Install\ macOS\ Sierra.app --nointeraction
```



## macOS 10.14+

```shell
sudo /Applications/Install\ macOS\ Catalina.app/Contents/Resources/createinstallmedia --volume /Volumes/Install --nointeraction
```

