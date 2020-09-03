---
title: Alfred 配置
date: 2018-05-06 06:09:54
---

 # Alfred



<!--more-->



## 问题

### 如何解决 alfred 每次开机都弹出 alfred 想访问你的通讯录

```shell
sudo codesign -f -d -s - /Applications/Alfred\ 3.app/Contents/Frameworks/Alfred\ Framework.framework/Versions/A/Alfred\ Framework
```

成功返回:

```shell
/Applications/Alfred 3.app/Contents/Frameworks/Alfred Framework.framework/Versions/A/Alfred Framework: replacing existing signature
```



## 参考

[【玩转MAC】解决Alfred每次开机后，都会提示“是否允许访问通讯录”](https://www.jianshu.com/p/cc206abae6e5)