---
title: FastMonkey 配置
date: 2018-10-22 21:57:05
tags:
---

下载工程代码
```shell
cd ~/Xcode

git clone https://github.com/zhangzhao4444/Fastmonkey.git
```

Cartfile

```
github "SwiftyJSON/SwiftyJSON"  ==  3.1.4
github "tadija/AEXML" == 4.1.0
```

下载工程所依赖的包

```shell
carthage update
```





## 参考

[FastMonkey](https://github.com/zhangzhao4444/Fastmonkey)

[基于 XCTestWD，swiftmonkey 二次开发，实现无需插桩的 iOS monkey 自动化工具 fastmonkey](https://testerhome.com/topics/9524)

[使用Fastmonkey进行Monkey测试实践](https://www.jianshu.com/p/2cbdb50411ae)

