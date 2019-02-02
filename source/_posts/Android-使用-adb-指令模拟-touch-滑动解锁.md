---
title: Android 使用 adb 指令模拟 touch 滑动解锁
date: 2019-01-09 19:02:59
tags:
---

**查找手机触摸屏事件 event 的位置，寻找包含touch字段的内容。**

```
adb shell cat /proc/bus/input/devices
```

```verilog
I: Bus=0018 Vendor=0000 Product=0000 Version=0000
N: Name="atmel-maxtouch"
P: Phys=
S: Sysfs=/devices/f9924000.i2c/i2c-2/2-004b/input/input2
U: Uniq=
E: Enabled=0
H: Handlers=kgsl event2 cpufreq
```

**录制解锁步骤，监听event**

```
adb shell getevent /dev/input/event2 > unlock.txt
```

**手机上滑动解锁一次**

**unlock.txt里面的十六进制数转换为十进制数。**

hex_to_dec.py

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
__date__ = "2019-01-09 15:22:46"
__author__ = "ovwane"

with open("unlock.txt", "r") as f:
    for file in f:
        num_list = []

        line = file
        for i in line.split(" "):
            i_num = int(i.rstrip("\n"), 16)
            num_list.append(i_num)

        print(num_list)
        with open("adb_unlock.sh", "a+", encoding="utf-8") as ff:
            ff.write("adb shell sendevent /dev/input/event2 {} {} {} \n".format(num_list[0], num_list[1], num_list[2]))
```

```python
# adb shell getevent //dev/input/event0 >> getevent.log

with open('./getevent2.sh', 'w') as f2:
    f2.write("adb shell input keyevent 26\n")
    # f2.write("adb shell input keyevent 82\r")
    with open('./getevent.log', 'r') as f:
        for line in f.readlines():
            new_line = " ".join([str(int(it, 16)) for it in line.strip().split(" ")])
            f2.write("adb shell sendevent //dev/input/event0 " + new_line + "\r")
```

```shell
adb shell getevent /dev/input/event2 | awk '{print "adb shell sendevent /dev/input/event2  "strtonum("0x"$1)" "strtonum("0x"$2)" "strtonum("0x"$3); fflush()}'  >> unlock.sh
```

**写到shell script中执行，就可以实现自动解锁了。**

```
adb shell sendevent /dev/input/event2 
```

模拟发送电源键事件，点亮屏幕。

```
adb shell input keyevent 26
```

## 参考

[Android通过指令模拟touch滑动解锁](https://blog.csdn.net/xiaobaiing/article/details/51363835)

[ADB获取手机屏幕的状态(点亮与否)以及ADB点击事件基本操作](https://www.jianshu.com/p/630c5026279c)

[awk 中 对于tail f 的文件重定向](https://blog.csdn.net/fcc7619666/article/details/52022015)