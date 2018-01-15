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