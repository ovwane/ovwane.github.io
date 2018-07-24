---
title: Markdown学习
date: 2017-07-26 18:24:20
---

#### 参考内容

Markdown是什么？

谁发明了这么个东西？

为什么要使用它？

怎么使用？

谁在用？

感觉有意思？不怕你看见，就怕你试试

=
1. Markdown是什么？

Markdown是一种轻量级标记语言，它以纯文本形式(易读、易写、易更改)编写文档，并最终以HTML格式发布。
Markdown也可以理解为将以MARKDOWN语言编写的语言转换成HTML内容的工具，最初是一个perl脚本Markdown.pl。

2. 谁发明了这么个东西？

它由Aaron Swartz和John Gruber共同设计，Aaron Swartz就是那位于去年（2013年1月11日）自杀,有着开挂一般人生经历的程序员。维基百科对他的介绍是：软件工程师、作家、政治组织者、互联网活动家、维基百科人。

他有着足以让你跪拜的人生经历：

14岁参与RSS 1.0规格标准的制订。

2004年入读斯坦福，之后退学。

2005年创建Infogami，之后与Reddit合并成为其合伙人。

2010年创立求进会（Demand Progress），积极参与禁止网络盗版法案（SOPA）活动，最终该提案居然被撤回。

2011年7月19日，因被控从MIT和JSTOR下载480万篇学术论文并以免费形式上传于网络被捕。

2013年1月自杀身亡。

天才都有早逝的归途（又是一位犹太人）。

3. 为什么要使用它？

它是易读（看起开舒服）、易写（语法简单）、易更改纯文本。处处体现着极简主义的影子。
兼容HTML，可以转换为HTML格式发布。
跨平台使用。
越来越多的网站支持Markdown。
更方便清晰的组织你的电子邮件。（Markdown-here, Airmail）
摆脱Word（我不是认真的）。
4. 怎么使用？

如果不算扩展，Markdown的语法绝对简单到让你爱不释手。

废话太多，下面正文，Markdown语法主要分为如下几大部分： 标题，段落，区块引用，代码区块，强调，列表，分割线，链接，图片，反斜杠 \，符号'`'。

4.1 标题

两种形式：
1）使用=和-标记一级和二级标题。

一级标题
=========
二级标题
---------
效果：

一级标题

二级标题
2）使用#，可表示1-6级标题。

# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
效果：

一级标题

二级标题

三级标题

四级标题

五级标题

六级标题
4.2 段落

段落的前后要有空行，所谓的空行是指没有文字内容。若想在段内强制换行的方式是使用两个以上空格加上回车（引用中换行省略回车）。

4.3 区块引用

在段落的每行或者只在第一行使用符号>,还可使用多个嵌套引用，如：

> 区块引用
>> 嵌套引用
效果：

区块引用

嵌套引用
4.4 代码区块

代码区块的建立是在每行加上4个空格或者一个制表符（如同写代码一样）。如
普通段落：

void main()
{
printf("Hello, Markdown.");
}

代码区块：

void main()
{
    printf("Hello, Markdown.");
}
注意:需要和普通段落之间存在空行。

4.5 强调

在强调内容两侧分别加上*或者_，如：

*斜体*，_斜体_
**粗体**，__粗体__
效果：

斜体，斜体
粗体，粗体
4.6 列表

使用·、+、或-标记无序列表，如：

-（+*） 第一项 -（+*） 第二项 - （+*）第三项
注意：标记后面最少有一个_空格_或_制表符_。若不在引用区块中，必须和前方段落之间存在空行。

效果：

第一项
第二项
第三项
有序列表的标记方式是将上述的符号换成数字,并辅以.，如：

1 . 第一项
2 . 第二项
3 . 第三项
效果：

第一项
第二项
第三项
4.7 分割线

分割线最常使用就是三个或以上*，还可以使用-和_。

4.8 链接

链接可以由两种形式生成：行内式和参考式。
行内式：

[younghz的Markdown库](https:://github.com/younghz/Markdown "Markdown")。
效果：

younghz的Markdown库。
参考式：

[younghz的Markdown库1][1]
[younghz的Markdown库2][2]
[1]:https:://github.com/younghz/Markdown "Markdown"
[2]:https:://github.com/younghz/Markdown "Markdown"
效果：

younghz的Markdown库1
younghz的Markdown库2
注意：上述的[1]:https:://github.com/younghz/Markdown "Markdown"不出现在区块中。

4.9 图片

添加图片的形式和链接相似，只需在链接的基础上前方加一个！。

4.10 反斜杠\

相当于反转义作用。使符号成为普通符号。

4.11 符号'`'

起到标记作用。如：

`ctrl+a`
效果：

ctrl+a
5. 谁在用？

Markdown的使用者：

GitHub
简书
Stack Overflow
Apollo
Moodle
Reddit
等等
6. 感觉有意思？趁热打铁，推荐几个_工具_。

Chrome下的stackedit插件可以离线使用，很爽。也不用担心平台受限。 在线的dillinger.io算是评价好的了，可是不能离线使用。
Windowns下的MarkdownPad也用过，不过免费版的体验不是很好。
Mac下的Mou是国人贡献的，口碑很好。推荐。
Linux下的ReText不错。
其实在对语法了如于心的话，直接用编辑器就可以了，脑子里满满的都是格式化好的文本啊。 我现在使用马克飞象 + Markdown-here，先编辑好，然后一键格式化，挺方便。

注意：不同的Markdown解释器或工具对相应语法（扩展语法）的解释效果不尽相同，具体可参见工具的使用说明。 虽然有人想出面搞一个所谓的标准化的Markdown，[没想到还惹怒了健在的创始人John Gruber] (http://blog.codinghorror.com/standard-markdown-is-now-common-markdown/)。

以上基本是所有traditonal markdown的语法。

其它：

列表的使用(非traditonal markdown)：

用|表示表格纵向边界，表头和表内容用-隔开，并可用:进行对齐设置，两边都有:则表示居中，若不加:则默认左对齐。

代码库	链接
MarkDown	https://github.com/younghz/Markdown
moos-young	https://github.com/younghz/moos-young
关于其它扩展语法可参见具体工具的使用说明。


# Makrdown help

# MacDown

![MacDown logo](http://macdown.uranusjr.com/static/images/logo-160.png)

Hello there! I’m **MacDown**, the open source Markdown editor for OS X.

Let me introduce myself.



## Markdown and I

**Markdown** is a plain text formatting syntax created by John Gruber, aiming to provide a easy-to-read and feasible markup. The original Markdown syntax specification can be found [here](http://daringfireball.net/projects/markdown/syntax).

**MacDown** is created as a simple-to-use editor for Markdown documents. I render your Markdown contents real-time into HTML, and display them in a preview panel.

![MacDown Screenshot](http://d.pr/i/10UGP+)

I support all the original Markdown syntaxes. But I can do so much more! Various popular but non-standard syntaxes can be turned on/off from the [**Markdown** preference pane](#markdown-pane).

You can specify extra HTML rendering options through the [**Rendering** preference pane](#rendering-pane).

You can customize the editor window to you liking in the [**Editor** preferences pane](#editor-pane):

You can configure various application (that's me!) behaviors in the [**General** preference pane](#general-pane).

## The Basics
Before I tell you about all the extra syntaxes and capabilities I have, I'll introduce you to the basics of standard markdown. If you already know markdown, and want to jump straight to learning about the fancier things I can do, I suggest you skip to the [**Markdown** preference pane](#markdown-pane). Lets jump right in.  

### Line Breaks
To force a line break, put two spaces and a newline (return) at the end of the line.

* This two-line bullet 
won't break

* This two-line bullet  
will break

Here is the code:

```
* This two-line bullet 
won't break

* This two-line bullet  
will break
```

### Strong and Emphasize

**Strong**: `**Strong**` or `__Strong__` (Command-B)  
*Emphasize*: `*Emphasize*` or `_Emphasize_`[^emphasize] (Command-I)

### Headers (like this one!)

	Header 1
	========
	
	Header 2
	--------

or

	# Header 1
	## Header 2
	### Header 3
	#### Header 4
	##### Header 5
	###### Header 6



### Links and Email
#### Inline
Just put angle brackets around an email and it becomes clickable: <uranusjr@gmail.com>  
`<uranusjr@gmail.com>`  

Same thing with urls: <http://macdown.uranusjr.com>  
` <http://macdown.uranusjr.com>`  

Perhaps you want to some link text like this: [Macdown Website](http://macdown.uranusjr.com "Title")  
`[Macdown Website](http://macdown.uranusjr.com "Title")` (The title is optional)  


#### Reference style
Sometimes it looks too messy to include big long urls inline, or you want to keep all your urls together.  

Make [a link][arbitrary_id] `[a link][arbitrary_id]` then on it's own line anywhere else in the file:  
`[arbitrary_id]: http://macdown.uranusjr.com "Title"`

If the link text itself would make a good id, you can link [like this][] `[like this][]`, then on it's own line anywhere else in the file:  
`[like this]: http://macdown.uranusjr.com`  

[arbitrary_id]: http://macdown.uranusjr.com "Title"
[like this]: http://macdown.uranusjr.com


### Images
#### Inline
`![Alt Image Text](path/or/url/to.jpg "Optional Title")`
#### Reference style
`![Alt Image Text][image-id]`  
on it's own line elsewhere:  
`[image-id]: path/or/url/to.jpg "Optional Title"`


### Lists

* Lists must be preceded by a blank line (or block element)
* Unordered lists start each item with a `*`
- `-` works too
	* Indent a level to make a nested list
		1. Ordered lists are supported.
		2. Start each item (number-period-space) like `1. `
		42. It doesn't matter what number you use, I will render them sequentially
		1. So you might want to start each line with `1.` and let me sort it out

Here is the code:

```
* Lists must be preceded by a blank line (or block element)
* Unordered lists start each item with a `*`
- `-` works too
	* Indent a level to make a nested list
		1. Ordered lists are supported.
		2. Start each item (number-period-space) like `1. `
		42. It doesn't matter what number you use, I will render them sequentially
		1. So you might want to start each line with `1.` and let me sort it out
```



### Block Quote

> Angle brackets `>` are used for block quotes.  
Technically not every line needs to start with a `>` as long as
there are no empty lines between paragraphs.  
> Looks kinda ugly though.
> > Block quotes can be nested.  
> > > Multiple Levels
>
> Most markdown syntaxes work inside block quotes.
>
> * Lists
> * [Links][arbitrary_id]
> * Etc.

Here is the code:

```
> Angle brackets `>` are used for block quotes.  
Technically not every line needs to start with a `>` as long as
there are no empty lines between paragraphs.  
> Looks kinda ugly though.
> > Block quotes can be nested.  
> > > Multiple Levels
>
> Most markdown syntaxes work inside block quotes.
>
> * Lists
> * [Links][arbitrary_id]
> * Etc.
```


### Inline Code
`Inline code` is indicated by surrounding it with backticks:  
`` `Inline code` ``

If your ``code has `backticks` `` that need to be displayed, you can use double backticks:  
```` ``Code with `backticks` `` ````  (mind the spaces preceding the final set of backticks)


### Block Code
If you indent at least four spaces or one tab, I'll display a code block.

	print('This is a code block')
	print('The block must be preceded by a blank line')
	print('Then indent at least 4 spaces or 1 tab')
		print('Nesting does nothing. Your code is displayed Literally')

I also know how to do something called [Fenced Code Blocks](#fenced-code-block) which I will tell you about later.

### Horizontal Rules
If you type three asterisks `***` or three dashes `---` on a line, I'll display a horizontal rule:

***


## <a name="markdown-pane"></a>The Markdown Preference Pane
This is where I keep all preferences related to how I parse markdown into html.  
![Markdown preferences pane](http://d.pr/i/RQEi+)

### Document Formatting
The ***Smartypants*** extension automatically transforms straight quotes (`"` and `'`) in your text into typographer’s quotes (`“`, `”`, `‘`, and `’`) according to the context. Very useful if you’re a typography freak like I am. Quote and Smartypants are syntactically incompatible. If both are enabled, Quote takes precedence.


### Block Formatting

#### Table

This is a table:

First Header  | Second Header
------------- | -------------
Content Cell  | Content Cell
Content Cell  | Content Cell

You can align cell contents with syntax like this:

| Left Aligned  | Center Aligned  | Right Aligned |
|:------------- |:---------------:| -------------:|
| col 3 is      | some wordy text |         $1600 |
| col 2 is      | centered        |           $12 |
| zebra stripes | are neat        |            $1 |

The left- and right-most pipes (`|`) are only aesthetic, and can be omitted. The spaces don’t matter, either. Alignment depends solely on `:` marks.

#### <a name="fenced-code-block">Fenced Code Block</a>

This is a fenced code block:

```
print('Hello world!')
```

You can also use waves (`~`) instead of back ticks (`` ` ``):

~~~
print('Hello world!')
~~~


You can add an optional language ID at the end of the first line. The language ID will only be used to highlight the code inside if you tick the ***Enable highlighting in code blocks*** option. This is what happens if you enable it:

![Syntax highlighting example](http://d.pr/i/9HM6+)

I support many popular languages as well as some generic syntax descriptions that can be used if your language of choice is not supported. See [relevant sections on the official site](http://macdown.uranusjr.com/features/) for a full list of supported syntaxes.


### Inline Formatting

The following is a list of optional inline markups supported:

Option name         | Markup           | Result if enabled     |
--------------------|------------------|-----------------------|
Intra-word emphasis | So A\*maz\*ing   | So A<em>maz</em>ing   |
Strikethrough       | \~~Much wow\~~   | <del>Much wow</del>   |
Underline [^under]  | \_So doge\_      | <u>So doge</u>        |
Quote [^quote]      | \"Such editor\"  | <q>Such editor</q>    |
Highlight           | \==So good\==    | <mark>So good</mark>  |
Superscript         | hoge\^(fuga)     | hoge<sup>fuga</sup>   |
Autolink            | http://t.co      | <http://t.co>         |
Footnotes           | [\^4] and [\^4]: | [^4] and footnote 4   |

[^4]: You don't have to use a number. Arbitrary things like `[^footy note4]` and `[^footy note4]:` will also work. But they will *render* as numbered footnotes. Also, no need to keep your footnotes in order, I will sort out the order for you so they appear in the same order they were referenced in the text body. You can even keep some footnotes near where you referenced them, and collect others at the bottom of the file in the traditional place for footnotes. 




## <a name="rendering-pane"></a>The Rendering Preference Pane
This is where I keep preferences relating to how I render and style the parsed markdown in the preview window.  
![Rendering preferences pane](http://d.pr/i/rT4d+)

### CSS
You can choose different css files for me to use to render your html. You can even customize or add your own custom css files.

### Syntax Highlighting
You have already seen how I can syntax highlight your fenced code blocks. See the [Fenced Code Block](#fenced-code-block) section if you haven’t! You can also choose different themes for syntax highlighting.

### TeX-like Math Syntax
I can also render TeX-like math syntaxes, if you allow me to.[^math] I can do inline math like this: \\( 1 + 1 \\) or this (in MathML): <math><mn>1</mn><mo>+</mo><mn>1</mn></math>, and block math:

\\[
    A^T_S = B
\\]

or (in MathML)

<math display="block">
    <msubsup><mi>A</mi> <mi>S</mi> <mi>T</mi></msubsup>
    <mo>=</mo>
    <mi>B</mi>
</math>



### Task List Syntax
1. [x] I can render checkbox list syntax
	* [x] I support nesting
	* [x] I support ordered *and* unordered lists
2. [ ] I don't support clicking checkboxes directly in the html window


### Jekyll front-matter
If you like, I can display Jekyll front-matter in a nice table. Just make sure you put the front-matter at the very beginning of the file, and fence it with `---`. For example:

```
---
title: "Macdown is my friend"
date: 2014-06-06 20:00:00
---
```

### Render newline literally
Normally I require you to put two spaces and a newline (aka return) at the end of a line in order to create a line break. If you like, I can render a newline any time you end a line with a newline. However, if you enable this, markdown that looks lovely when I render it might look pretty funky when you let some *other* program render it.





## <a name="general-pane"></a>The General Preferences Pane

This is where I keep preferences related to application behavior.  
![General preferences pane](http://d.pr/i/rvwu+)

The General Preferences Pane allows you to tell me how you want me to behave. For example, do you want me to make sure there is a document open when I launch? You can also tell me if I should constantly update the preview window as you type, or wait for you to hit `command-R` instead. Maybe you prefer your editor window on the right? Or to see the word-count as you type. This is also the place to tell me if you are interested in pre-releases of me, or just want to stick to better-tested official releases.  

## <a name="editor-pane"></a>The Editor Preference Pane
This is where I keep preferences related to the behavior and styling of the editing window.  
![Editor preferences pane](http://d.pr/i/6OL5+)


### Styling

My editor provides syntax highlighting. You can edit the base font and the coloring/sizing theme. I provided some default themes (courtesy of [Mou](http://mouapp.com)’s creator, Chen Luo) if you don’t know where to start.

You can also edit, or even add new themes if you want to! Just click the ***Reveal*** button, and start moving things around. Remember to use the correct file extension (`.styles`), though. I’m picky about that.

I offer auto-completion and other functions to ease your editing experience. If you don’t like it, however, you can turn them off.





## Hack On

That’s about it. Thanks for listening. I’ll be quiet from now on (unless there’s an update about the app—I’ll remind you for that!).

Happy writing!


[^emphasize]: If **Underlines** is turned on, `_this notation_` will render as underlined instead of emphasized 

[^under]: If **Underline** is disabled `_this_` will be rendered as *emphasized* instead of being underlined.

[^quote]: **Quote** replaces literal `"` characters with html `<q>` tags. **Quote** and **Smartypants** are syntactically incompatible. If both are enabled, **Quote** takes precedence. Note that **Quote** is different from *blockquote*, which is part of standard Markdown.

[^math]: Internet connection required.

## 参考

[Markdown官网](https://daringfireball.net/projects/markdown/)
[Markdown维基百科](https://zh.wikipedia.org/wiki/Markdown)

[Learning-Markdown (Markdown 入门参考)](http://xianbai.me/learn-md/)

[献给写作者的 Markdown 新手指南](http://www.jianshu.com/p/q81RER)

[参考](https://github.com/younghz/Markdown)