---
title: VS Code 配置
date: 2019-05-10 11:03:14
tags:
---



#### Text Editor

Font Size: `20`

- editor cursor， 是跟光标渲染和多光标相关的设置；
- editor find， 是与编辑器内搜索相关的设置；
- editor font， 是与字体有关的设置；
- editor format， 是代码格式化；
- editor suggest， 是和自动补全、建议窗口等相关的配置。



#### Breadcrumbs

> 面包屑导航(Breadcrumbs)

Enable



#### 多文件搜索视图的位置

search.location: `panel`



#### 空格符

editor.renderWhitespace: `all`



#### 缩进参考线

`editor.renderIndentGuides`



#### 垂直标尺

`"editor.rulers": [120]`



#### 光标的样式

editor.cursorBlinking

editor.cursorStyle:`block`

editor.cursorWidth:`0`

editor.renderLineHighlight: "all"



#### 空格

editor.detectIndentation

editor.insertSpaces

editor.tabSize



#### 代码风格

editor.formatOnSave: true

editor.formatOnType: true



#### 小地图 Minimap

`editor.minimap.enabled`：取消



#### 新的空文件默认语言类型

files.defaultLanguage：`markdown`

> `javascript`



## 快捷键

自定义快捷键：`先按 cmd+k 然后再按 cmd+s`

命令面板： `cmd+shift+p`

文件资源管理器：`cmd+shift+e`

跨文件搜索：`cmd+shift+f`

源代码管理：`cmd+shift+g`

启动和调试：`cmd+shift+d`

管理扩展：`cmd+shift+x`

光标移动：`cmd` 行跳转，`option` 单词间跳转，`cmd+shfit+\`代码块间跳转。

合并代码行：`ctrl+j`

多光标操作：`shift+方向键`

复制一行：`option+shift+上下键`

多行操作`cmd+option+上下键`

打开文件之间跳转：`ctrl+tab`

最近打开文件列表：`cmd+p`

符号（Symbols）跳转：`cmd+shift+o`

多个文件内符号跳转：`cmd+t`

定义实现查看：`cmd+f12`

引用 (Reference) 跳转：`shift+f12`

代码折叠：`cmd+option+[`

代码展开：`cmd+option+]`

递归折叠：`cmd+k cmd+[`

递归展开：`cmd+k cmd+]`

所有可折叠的都折叠：`cmd+k cmd+0`

所有课展开的都展开：`cmd+k cmd+j`

面板：`cmd+j`

打开终端：ctrl+`

新建终端：ctrl+shitf+`

多窗口：`cmd+\`

窗口间切换：`cmd+1`，`cmd+2`，`cmd+3`

窗口纵向切换：`cmd+option+0`

grid 2 x 2 窗口布局

专注模式：`cmd+b`

窗口缩放：`cmd + +`，`cmd + -`



## 代码片段（code snippet）

## 文件搜索

单文件搜索：`cmd+f`

搜索结果向下跳转：`cmd+g`

搜索结果向上跳转：`cmd+shift+g`

全单词匹配：`cmd+option+w`

正则表达式：`cmd+option+r`

单文件替换：`cmd+option+f`

多文件搜索：`cmd+shift+f`

多文件替换：`cmd+shift+h`



## 任务系统

单任务

多任务



# debugger

快捷键：`cmd+shif+d`

运行：`f5`



## 插件

### Rainbow Brackets

括号颜色



### Indent Rainbow

缩进颜色



### Import Cost

JavaScript 经常被吐槽的一个地方，就是大家对 npm 库的使用程度非常高，经常为了一个简单的功能，引入了几兆甚至十几兆的 npm 包。[Import Cost](https://marketplace.visualstudio.com/items?itemName=wix.vscode-import-cost)这个插件，很好地在代码中给我们以提示，告诉我们引入的某个包，它最终会导致整个项目的大小增加多少。



### Debugger for Chrome



### Docker



## 基础入门

1-13讲

```

```



