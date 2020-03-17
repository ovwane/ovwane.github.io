---
date: 2017-02-26 08:27:02
---

批量替换文件名
& 正则匹配的内容，
\1 正则括号内匹配的内容

```bash
ls *.mp4|sed 's/\(^[0-9][0-9]\).*\(day1.*\)/mv "&" "\1.\2"/g'|bash
```





## 参考

 [sed之N和$!N的区别和运用-zooyo-ChinaUnix博客](http://blog.chinaunix.net/uid-10540984-id-1759548.html) 