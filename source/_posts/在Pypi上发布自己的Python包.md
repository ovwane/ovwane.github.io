---
title: 在Pypi上发布自己的Python包
date: 2018-10-14 08:58:59
tags:
---



**源码打包**

**打包成一个源代码的包**

```shell
python setup.py sdist build
```

这样会在dist文件夹下生成一个tar.gz结尾文件。

**打包成一个wheels格式**

```shell
python setup.py bdist_wheel --universal
```

这样会在dist文件夹下生成一个whl文件。



## 参考

[在Pypi上发布自己的Python包 - 简书](https://www.jianshu.com/p/e9ec8666decc)

[在Pypi上发布自己的Python包 - 孤独的居士 - 博客园](http://www.cnblogs.com/sting2me/p/6550897.html)

[Packaging Python Projects — Python Packaging User Guide](https://packaging.python.org/tutorials/packaging-projects/)