---
title: Python 学习笔记 第三版学习笔记
date: 2019-02-28 17:16:49
tags:
---

> Python 3.6.1



## 软件

### MessagePack

> 简单来讲，它的数据格式与json类似，但是在存储时对数字、多字节字符、数组等都做了很多优化，减少了无用的字符，二进制格式，也保证不用字符化带来额外的存储空间的增加。

```shell
pip install msgpack
```

[MessagePack: It's like JSON. but fast and small.](https://msgpack.org/)



### psutil

>psutil = process and system utilities，它不仅可以通过一两行代码实现系统监控，还可以跨平台使用，支持Linux／UNIX／OSX／Windows等，是系统管理员和运维小伙伴不可或缺的必备模块。

```shell
pip install psutil
```

[psutil - 廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001511052957192bb91a56a2339485c8a8c79812b400d49000)

[giampaolo/psutil: Cross-platform lib for process and system monitoring in Python](https://github.com/giampaolo/psutil)



## 性能测试



### Pympler

> pympler 则被⽤来统计对象实例的内存使⽤。

```shell
pip install pympler
```



## 调试器

> iPDB ⽀持语法⾼亮，且输出更加友好。

```shell
pip install ipdb
```

[使用 ipdb 调试 Python - 小沐枫 - 博客园](https://www.cnblogs.com/zimufeng/p/6188229.html)