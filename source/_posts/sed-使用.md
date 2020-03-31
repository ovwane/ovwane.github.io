---
title: sed 使用
date: 2017-02-26 08:27:02
---

# [sed](https://www.gnu.org/software/sed/)

stream editor

> sed 是一个非交互性文本流编辑器。流编辑器是一行一行处理文件内容的，然后对该行进行指定的处理，然后正在处理的内容存放在模式空间（缓冲区）内，并将结果输出到屏幕，接着读入下一行。整个文件像流水一样被逐行处理然后逐行输出。这就是流编辑器。



Sed工作流程：首先读取文本中的第1行，将其放入模式空间内。然后读取第一条编辑指令，使用指令中定义的模式和行号查找、编辑文本（这些操作都是针对读入到模式空间里的文本进行的操作，原文本的内容不受影响）。 如果不匹配，则忽略后续的编辑命令。完成编辑后，将结果输出并读取下一行，重复这个过程直到文本结束。文本内容不会被改变，只是改变了输出。



除了模式空间之外，sed还使用了一个被称为保留空间的临时缓冲区，保留空间通常用于暂存编辑内容。暂存空间里默认存储一个空行。



<!--more-->



pattern space：模式空间

hold space：保留空间



模式空间:初始化为空，处理完一行后会自动输出到屏幕并清除模式空间。

保持空间:初始化为一个空行，也就是默认带一个\n，处理完后不会自动清除。

模式空间：可以想成工程里面的流水线，数据之间在它上面进行处理，用于处理文本行。

保持空间：可以想象成仓库，我们在进行数据处理的时候，作为数据的暂存区域，用于保留文本行，是保存已经处理过的输入行，默认有一个空行。



 模式空间和保持空间(暂存空间)命令

- `n N`    Read/append the next line of input into the pattern space.
- `h H`    Copy/append pattern space to hold space.
- `g G`    Copy/append hold space to pattern space.
- `x`     Exchange the contents of the hold and pattern spaces
- ！ 对所选行以外的所有行应用命令。

打印行（1,12,123）

```
sed 'H;g'
```



反序行（321）

```
sed '1!G;h;$!d'
```



知识点

- Pattern Space
- Address
- {}
- Hold Space
- 



不替换原内容：`&`变量

```bash
sed 's/root/Hello&OK/g' /etc/passwd
```



批量替换文件名

```bash
ls *.mp4|sed 's/\(^[0-9][0-9]\).*\(day1.*\)/mv "&" "\1.\2"/g'|bash
```

- `&` 正则匹配的内容
- `\1` 正则括号内匹配的内容



## 参考

 [sed, a stream editor](https://www.gnu.org/software/sed/manual/sed.html) 

 [SED单行脚本快速参考](http://sed.sourceforge.net/sed1line_zh-CN.html)

 [sed之N和$!N的区别和运用-zooyo-ChinaUnix博客](http://blog.chinaunix.net/uid-10540984-id-1759548.html) 