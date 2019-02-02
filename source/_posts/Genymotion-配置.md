---
title: Genymotion 配置
date: 2019-01-04 07:39:27
tags:
---

## ARM_Translation



## OpenGapps

```
~/.Genymobile/Genymotion/cache/opengapps
```



## GPS

启动 Genymotin Shell

```shell
/Applications/Genymotion\ Shell.app/Contents/MacOS/genyshell
```

```shell
# 查看设备
devices list

# 选择设备，0是设备id。
devices select 0

# 查看 GPS 状态
gps getstatus

# 开启 GPS
gps setstatus enabled

# 关闭 GPS
gps setstatus disabled

# 获取纬度
gps getlatitude

# 南京
gps setlatitude 32.0523428565

# 广州
gps setlatitude 23.1290765766

# 获取经度
gps getlongitude

# 南京
gps setlongitude 118.7847135259

# 广州
gps setlongitude 113.2643446427
```

[经纬度查询](http://www.gpsspg.com/maps.htm)

## 参考

[Starting Genymotion Shell](https://docs.genymotion.com/latest/Content/04_Tools/Genymotion_Shell/Starting_Genymotion_Shell.htm)

[Genymotion安装配置指南](http://ju.outofmemory.cn/entry/317903)