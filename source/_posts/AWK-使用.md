---
title: AWK 使用
date: 2018-08-08 16:52:07
---

# [AWK](https://www.gnu.org/software/gawk/)

样式扫描和处理语言



## 概念

- RS：Record Separator，记录分隔符。

- ORS：Output Record Separate，输出当前记录分隔符。

- FS：Field Separator，字段分隔符。

- OFS：Out of Field Separator，输出字段分隔符。
- NF：Number of Field，行中字段的个数。
- NR：Number of Row，当前处理的行数。



## 参数

-F 字段分隔符
`awk -F":" '{print $0}' /etc/passwd`

$0 整行
$1 以“：”冒号分割的第一列
$2 以“：”冒号分割的第二列

NF==7 字段数量
NR==2 纪录数量





用 awk 中查看服务器连接状态并汇总

```bash
netstat -an|awk '/^tcp/{++s[$NF]}END{for(a in s)print a,s[a]}'
```



统计 web 日志访问流量，要求输出访问次数，请求页面或图片，每个请求的总大小， 总访问流量的大小汇总

```bash
awk '{a[$7]+=$10;++b[$7];total+=$10}END{for(x in a)print b[x],x,a[x]|"sort -rn -k1";print "total size is :"total}' /app/log/access_log
```



数值转换，16进制转换魏10进制

```
adb shell getevent /dev/input/event2 | awk '{print "adb shell sendevent /dev/input/event2  "strtonum("0x"$1)" "strtonum("0x"$2)" "strtonum("0x"$3); fflush()}'  >> unlock.sh
```

```shell
awk '{print "adb shell sendevent /dev/input/event2  "strtonum("0x"$1)" "strtonum("0x"$2)" "strtonum("0x"$3); fflush()}'
```

>fflush()立刻刷新缓存到文件。



查看 NF 数量

```shell
ifconfig eth0 | awk -F [" "]+ '{print "NF =", NF, $0}'
```

>[awk -F "[ :]+"](https://blog.51cto.com/meiling/2307401)
>
>[" "]+这个是正则表达式，+表示一个或多个，这里就表示一个或多个空格。



拆分行

```shell
curl -s https://testerhome.com/api/v3/topics.json | awk 'BEGIN{RS="}},{"}{print $0}'
```

> RS="}},{" `}},{`



合并行

```bash
awk '{if(NR%4!=0)ORS=" ";else ORS="\n"}1'
```

> 每4行合并为1行。



从file文件中找出长度大于80的行

```bash
awk 'length>80' file
```



按连接数查看客户端IP

```bash
netstat -ntu | awk '{print $5}' | cut -d: -f1 | sort | uniq -c | sort -nr
```



 打印99乘法表

```bash
seq 9 | sed 'H;g' | awk -v RS='' '{for(i=1;i<=NF;i++)printf("%dx%d=%d%s", i, NR, i*NR, i==NR?"\n":"\t")}'
```



## 参考

 [The GNU Awk User’s Guide](https://www.gnu.org/software/gawk/manual/gawk.html) 

[AWK单行脚本快速参考](http://www.pement.org/awk/awk1line_zh-CN.txt)
[awk之RS、ORS与FS、OFS](https://www.cnblogs.com/fhefh/archive/2011/11/16/2251656.html)

 [AWK 简明教程 | | 酷 壳 - CoolShell](https://coolshell.cn/articles/9070.html) 