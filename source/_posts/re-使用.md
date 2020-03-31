---
title: Regular Expression 使用
date: 2020-04-02 09:21:38
tags:
---

# Regular Expression

正则表达式



正则表达式引擎：DFA （Deterministic Final Automata 确定型有穷自动机）和 NFA 自动机（Non deterministic Finite Automaton 不确定型有穷自动机）



回溯陷阱（Catastrophic Backtracking）



贪婪模式、懒惰模式、独占模式



<!--more-->



正则语法检查网站： [Online regex tester and debugger: PHP, PCRE, Python, Golang and JavaScript](https://regex101.com/) 

常用正则表达式： [正则大全](https://any86.github.io/any-rule/) 



只匹配中文

```
[一-龥]{2,8}
```

> 2 到 8 个字符。



## 参考

 [正则表达式30分钟入门教程](https://deerchao.cn/tutorials/regex/regex.htm) 

 [正则表达式“派别”简述 | Keep Coding](https://liujiacai.net/blog/2014/12/07/regexp-favors/) 

 [不同环境下的正则表达式](https://rgb-24bit.github.io/blog/2019/regular-expressions-in-different-environments.html)

 [Shell正则表达式 列表](https://man.linuxde.net/docs/shell_regex.html) 

 [Linux 的使用 - 正则表达式 - LINOTES](https://linotes.imliloli.com/tools/re/#%E5%87%A0%E7%A7%8D-posix-%E6%B5%81%E6%B4%BE) 

 [一个正则表达式引发的血案，让线上CPU100%异常！ - 知乎](https://zhuanlan.zhihu.com/p/38229530)

 [一个由正则表达式引发的血案 - 明志健致远 - 博客园](https://www.cnblogs.com/study-everyday/p/7426862.html) 

