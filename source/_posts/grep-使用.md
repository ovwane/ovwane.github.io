---
title: Grep 使用
date: 2018-08-08 16:39:20
---

# [Grep](https://www.gnu.org/software/grep/)



打印匹配项后内容

```
grep -A 1 "text" file.txt
```

> 1 为显示的行数，可以随意更改。



打印匹配项前内容

```
grep -B 1 "text" file.txt
```



打印匹配项前后内容

```
grep -C 1 "text" file.txt
```



```shell
grep --line-buffered
```

> [grep 动态文件输出](http://www.firefoxbug.com/index.php/archives/2310/)
>
> [输出流缓冲的意义 何时缓冲 Stdout Buffering](https://www.cnblogs.com/liqiuhao/p/7669074.html)

