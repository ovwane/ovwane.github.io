---
title: IntelliJ IDEA 配置
date: 2018-05-11 12:00:40
tags:
---

> IntelliJ IDEA 2018.1.2

## 设置

Apperance & BBehavior->Apperance**

Theme Darcula



**Keymap**

Mac OS X 10.5+



**Editor**

Font: Menlo

Size: 20

## File and Code Templates

### Class

```java
#if (${PACKAGE_NAME} && ${PACKAGE_NAME} != "")package ${PACKAGE_NAME};#end
#parse("File Header.java")
/**
* @author: ovwane
*
* @create: ${YEAR}-${MONTH}-${DAY} ${HOUR}:${MINUTE}:${SECOND}
**/

public class ${NAME} {
}
```



## 插件

### Maven Helper

一旦安装了Maven Helper插件，只要打开pom文件，就可以打开该pom文件的Dependency Analyzer视图（在文件打开之后，文件下面会多出这样一个tab）。

### FindBugs-IDEA

FindBugs很多人都并不陌生，Eclipse中有插件可以帮助查找代码中隐藏的bug，IDEA中也有这款插件。

使用方法很简单，就是可以对多种级别的内容进行finbugs

### .ignore

git提交时过滤掉不需要提交的文件，很方便，有些本地文件是不需要提交到Git上的。

### CamelCase

将不是驼峰格式的名称，快速转成驼峰格式，安装好后，选中要修改的名称，按快捷键shift+alt+u。

### Lombok plugin

开发神器，可以简化你的实体类，让你i不再写get/set方法，还能快速的实现builder模式，以及链式调用方法，总之就是为了简化实体类而生的插件。

### codehelper.generator

可以让你在创建一个对象并赋值的时候，快速的生成代码，不需要一个一个属性的向里面set,根据new关键字，自动生成掉用set方法的代码，还可以一键填入默认值。

### GsonFormat

一键根据json文本生成java类 非常方便

### String Manipulation

字符串日常开发中经常用到的，但是不同的字符串类型在不同的地方可能有一些不同的规则，比如类名要用驼峰形式、常量需要全部大写等，有时候还需要进行编码解码等。这里推荐一款强大的字符串转换工具——String Manipulation

### Key promoter X

Key Promoter X 是一个提示插件，当你在IDEA里面使用鼠标的时候，如果这个鼠标操作是能够用快捷键替代的，那么Key Promoter X会弹出一个提示框，告知你这个鼠标操作可以用什么快捷键替代。

### GenerateAllSetter

一键调用一个对象的所有set方法并且赋予默认值 在对象字段多的时候非常方便，在做项目时，每层都有各自的实体对象需要相互转换，但是考虑BeanUtil.copyProperties()等这些工具的弊端，有些地方就需要手动的赋值时，有这个插件就会很方便，创建完对象后在变量名上面按Alt+Enter就会出来 generate all setter选项。

### AceJump

前面介绍了一款可以通过使用快捷键来代替鼠标操作的插件，这里再介绍一款可以彻底摆脱鼠标的插件，即AceJump

AceJump允许您快速将光标导航到编辑器中可见的任何位置，只需点击“ctrl +;”，然后输入一个你想要跳转到的字符，之后键入匹配的字符就跳转到你想要挑战的地方了。

### CodeGlance

在编辑区的右侧显示的代码地图。

### Background image Plus

这是一款可以设置idea背景图片的插件，不但可以设置固体的图片，还可以设置一段时间后随机变化背景图片，以及设置图片的透明度等等。

### active-power-mode

这是一款让你在编码的时候，整个屏幕都为之颤抖的插件。

### Nyan progress bar

这是一个将你idea中的所有的进度条都变成萌新动画的小插件。

### Rainbow Brackets

彩虹颜色的括号 看着很舒服 敲代码效率变高。

### Alibaba Java Coding Guidelines

Alibaba Java Coding Guidelines pmd implements

## Git

- GitLab Projects （Share on GitLab）

### [ChineseTypography](https://github.com/judasn/ChineseTypography-IDEA-Plugin)

此插件可以快速对文章内容的中文、英文、符号之间增加空格，增加文章可读性。

**Usage**：

- **1.Shortcut Key：** Ctrl + Shift + Y（Windows/Linux）、Command + Shift + Y（Mac）
- **2.Toolbar：** Code -- ChineseTypography

> 好的开发工具可以提高开发效率，所以的能让自己提高效率，把时间节省出来去学习，去提升自己。这些插件只是日常开发当中用到的一些，等到以后再发现了新的好玩的有意思，和提高工作效率的插件，继续分享出来。

## 参考

[Java程序员你们想要代码写的溜，这几个IDEA插件了解下](https://c.m.163.com/news/a/E38OGR040529KUS3.html?spss=newsapp&from=timeline&spssid=304b698c77a53e1e7920117a9769c58c&spsw=1)

[如何在IntelliJ IDEA中使用.ignore插件忽略不必要提交的文件](https://blog.csdn.net/qq_34590097/article/details/56284935)

[idea生成类注释和方法注释的正确方法](https://blog.csdn.net/qq_34581118/article/details/78409782)

[IntelliJ IDEA 简体中文专题教程](https://github.com/judasn/IntelliJ-IDEA-Tutorial)

[IntelliJ IDEA 18 周岁，吐血推进珍藏已久的必装插件](https://mp.weixin.qq.com/s?__biz=MzI3NzE0NjcwMg==&mid=2650123120&idx=1&sn=563ae7e8fd66e0d7c311ea9a8616d558&chksm=f36bb651c41c3f4700bcf053340a0b9e52e8ad07399d9758d677c426116b2e876b0a4378977f&mpshare=1&scene=23&srcid=#rd)