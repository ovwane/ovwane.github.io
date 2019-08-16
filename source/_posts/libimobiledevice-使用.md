---
title: libimobiledevice 使用
date: 2019-08-16 11:45:39
tags:
---

[libimobiledevice](https://github.com/libimobiledevice/libimobiledevice)

[libusbmuxd](https://github.com/libimobiledevice/libusbmuxd) | [usbmuxd](https://github.com/libimobiledevice/usbmuxd)

[ideviceinstaller](https://github.com/libimobiledevice/ideviceinstaller)



## usbmuxd

命令

- icat   
- iproxy

文件

- /var/run/usbmuxd
- `/var/lib/lockdown` (Linux) or /var/db/lockdown (macOS)
- `/System/Library/PrivateFrameworks/MobileDevice.framework/Versions/A/Resources/usbmuxd -launchd`

[Usbmux](https://www.theiphonewiki.com/wiki/Usbmux) 简介



## libimobiledevice

命令

- idevice_id：查看设备 id
- idevicebackup
- idevicebackup2
- idevicecrashreport
- idevicedate
- idevicedebug
- idevicedebugserverproxy
- idevicediagnostics
- ideviceenterrecovery
- ideviceimagemounter
- ideviceinfo：查看设备具体参数
- idevicename：查看设备名字
- idevicenotificationproxy
- idevicepair
- ideviceprovision
- idevicescreenshot：设备当前屏幕截图
- idevicesyslog：查看日志



## ideviceinstaller

命令

- ideviceinstaller：安装软件，ipa包。



## macOS 配置

```shell
# 卸载
brew uninstall --ignore-dependencies libimobiledevice
brew uninstall --ignore-dependencies usbmuxd
brew uninstall ideviceinstaller

# 安装
brew install --HEAD usbmuxd
brew unlink usbmuxd
brew link usbmuxd

# 安装
brew install --HEAD libimobiledevice

# 安装
brew install ideviceinstaller
```



