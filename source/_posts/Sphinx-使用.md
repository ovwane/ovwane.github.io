---
ctitle: Sphinx 使用
date: 2019-09-02 13:55:01
tags:
---

# [Sphinx](https://github.com/sphinx-doc/sphinx)

## 安装

```
pip install Sphinx
```



## 使用

创建 Sphinx 项目

```
mkdir docs; cd docs
```

```
sphinx-quickstart
```

```
Welcome to the Sphinx 2.2.0 quickstart utility.

Please enter values for the following settings (just press Enter to
accept a default value, if one is given in brackets).

Selected root path: .

You have two options for placing the build directory for Sphinx output.
Either, you use a directory "_build" within the root path, or you separate
"source" and "build" directories within the root path.
> Separate source and build directories (y/n) [n]: y

The project name will occur in several places in the built documentation.
> Project name: test
> Author name(s): test
> Project release []: 1.0.0

If the documents are to be written in a language other than English,
you can select a language here by its language code. Sphinx will then
translate text that it generates into that language.

For a list of supported codes, see
https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-language.
> Project language [en]: zh_CN

Creating file ./source/conf.py.
Creating file ./source/index.rst.
Creating file ./Makefile.
Creating file ./make.bat.

Finished: An initial directory structure has been created.

You should now populate your master file ./source/index.rst and create other documentation
source files. Use the Makefile to build the docs, like so:
   make builder
where "builder" is one of the supported builders, e.g. html, latex or linkcheck.
```



生成文档

```
make html
```

> 生成的文档位于build/html文件夹。



## 主题



## 参考

[编写《Redis 设计与实现》时用到的工具](https://blog.huangz.me/diary/2013/tools-for-writing-redisbook.html)

[Sampledoc](https://matplotlib.org/sampledoc/)

[用 Sphinx + reStructuredText 构建笔记系统](https://tech.silverrainz.me/2017/03/29/use-sphinx-and-rst-to-manage-your-notes.html)

[用Sphinx编写技术文档](http://zhengkun.info/2014/02/10/sphinx-doc/)

[rst语法进阶之一](http://www.modernfig.cn/blog/sphinx/blog_2)

[用Python做科学计算](http://bigsec.net/b52/scipydoc/pydoc_write_tools.html)

[写最好的文档：Sphinx + Read the Docs](https://avnpc.com/p/170)