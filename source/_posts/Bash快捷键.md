---
date: 2018-08-08 16:52:00
---

>>macOS 10.13.2
>iTerm2 3.1.2
>zsh 5.3 (x86_64-apple-darwin17.0)

>生活在 Bash shell 中，熟记以下快捷键，将极大的提高你的命令行操作效率。

<p font-color="red">macOS下 Alt 被替换为 Esc</p>

### 编辑命令
### 命令补全
```
Tab　　　  用于命令补全

^I 　　　　 可用于命令补全

^[ 　　　　 相当于Esc，也可补全
```

### 删除命令
```
Ctrl + u ：从光标处删除至命令行首
Ctrl + k ：从光标处删除至命令行尾
Ctrl + w ：从光标处删除至字首
Alt + d ：从光标处删除至字尾
Ctrl + d ：删除光标处的字符
Ctrl + h ：删除光标前的字符
^B 　　　　删除光标所在字母
^Y 　　　　粘贴或恢复上次的删除
```

### 移动命令
```
^P 、^N、 ^B、 ^F    　　方向键 上 下 左 右
Ctrl + a ：移到命令行首
Ctrl + e ：移到命令行尾
Ctrl + f ：按字符前移（右向）
Ctrl + b ：按字符后移（左向）
Alt + f ：按单词前移（右向）
Alt + b ：按单词后移（左向）
Ctrl + xx：在命令行首和光标之间移动
```

```
Ctrl + y ：粘贴至光标后
Alt + c ：从光标处更改为首字母大写的单词
Alt + u ：从光标处更改为全部大写的单词
Alt + l ：从光标处更改为全部小写的单词
Ctrl + t ：交换光标处和之前的字符
Alt + t ：交换光标处和之前的单词
Alt + Backspace：与 Ctrl + w ~~相同~~类似，分隔符有些差别 [感谢 rezilla 指正]
Ctrl+Alt+E 　扩展命令行
Esc+T 　　 置换前两个单词
```

### 重新执行命令
```
Ctrl + r：逆向搜索命令历史
Ctrl + g：从历史搜索模式退出
Ctrl + p：历史中的上一条命令
Ctrl + n：历史中的下一条命令
Alt + .：使用上一条命令的最后一个参数
Alt+P 　　   非增量方式反向搜索历史

Alt+> 　　   历史列表中的最后一行命令开始向前
```

### 控制命令
```
Ctrl + l：清屏
Ctrl + o：执行当前命令，并选择上一条命令
Ctrl + s：阻止屏幕输出
Ctrl + q：允许屏幕输出
Ctrl + c：终止命令
Ctrl + z：挂起命令
Bang (!) 命令
^S　　　　锁住屏幕

^Q　　　　恢复屏幕

^C　　　　杀死当前进程 

 ^\　　　　停止当前进程

^D　　　　退出当前shell

& 　　　　 后台执行，（nohup以忽略挂起信号方式运行程序）

^Z　　　　把当前进程转后台运行

jobs　　　 查看当前后台作业状态

fg 　　　　将后台作业拿到前台处理

bg　　　　作业在后台运行
^L　　　　清屏

^M或^J　  回车
```

### !!：执行上一条命令
```
!blah：执行最近的以 blah 开头的命令，如 !ls
!blah:p：仅打印输出，而不执行
!$：上一条命令的最后一个参数，与 Alt + . 相同
!$:p：打印输出 !$ 的内容
!*：上一条命令的所有参数
!*:p：打印输出 !* 的内容
^blah：删除上一条命令中的 blah
^blah^foo：将上一条命令中的 blah 替换为 foo
^blah^foo^：将上一条命令中所有的 blah 都替换为 foo
```


8.一些bash文档

1.GNU文档：http://www.gnu.org/software/bash/manual/

2.鸟哥的私房菜：http://linux.vbird.org/linux_basic/0320bash.php

3.IBM的一些bash教程：http://www.ibm.com/developerworks/cn/views/linux/libraryview.jsp?sort_by=&show_abstract=true&show_all=&search_flag=&contentarea_by=Linux&search_by=bash&topic_by=-1&type_by=%E6%89%80%E6%9C%89%E7%B1%BB%E5%88%AB&ibm-search=%E6%90%9C%E7%B4%A2

