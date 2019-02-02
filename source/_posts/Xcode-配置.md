---
title: Xcode 配置
date: 2012-09-07 15:27:44
tags:
---

### 移除对旧设备的支持

影响：可重新生成；再连接旧设备调试时，会重新自动生成。我移除了4.3.2, 5.0, 5.1等版本的设备支持。

路径：`~/Library/Developer/Xcode/iOS\ DeviceSupport`

### 移除DerivedData

影响：可重新生成；会删除build生成的项目索引、build输出以及日志。重新打开项目时会重新生成，大的项目会耗费一些时间。

路径：`~/Library/Developer/Xcode/DerivedData`

路径：`~/Library/Developer/Xcode/iOS Device Logs`

### 快速删除 Xcode 中的 Components

```
/Library/Developer/CoreSimulator/Profiles/Runtimes
```

```
~/Library/Caches/com.apple.dt.Xcode/Downloads
```



## 参考

[xcode文件过大，手动删除无用文件](https://blog.csdn.net/a787188834/article/details/78813030)

[快速删除 Xcode 中的 Components](https://fang2hou.com/development/remove-xcode-components/)