---
title: jq 使用
date: 2020-04-30 15:24:55
tags:
---

# [jq](https://stedolan.github.io/jq/)

Shell 下 JSON 解析工具。



<!--more-->



选取多个值

```
jq '.terms[0].chapter_info[0] | .typeText'
```

> `|`



选取所有值

```
jq '.terms[]'
```

> `[]`



筛选

```
jq '.iterm[] | select(.rating<5)'
```

>判断 rating 的值小于5 的显示 iterm。



修改

```bash
cat data.json | jq '.data.items[0].quota.name="aa"'
```

> name赋值为aa。



参数字符串转换数字：https://stedolan.github.io/jq/manual/#tonumber

```
jq --arg ARG1 1 '.[$ARG1|tonumber]'
```



切片

```
jq '.item[0:10]'
```



添加字段

```bash
timestamp=$(date +%s) && echo '{"s":1,"k":2}'|jq '. += {timestamp:'$timestamp'}'
```



判断

```
  # if .[6] == "1" then .[6] else [.[6],.[1]] end
```



## 参考

 [jq 1.6 Manual](https://stedolan.github.io/jq/manual/v1.6/) 

 [JSON格式化输出和解析工具 - jq - 散尽浮华 - 博客园](https://www.cnblogs.com/kevingrace/p/7565371.html) 

 [Shell：无比强大的shell之json解析工具jq , Linux命令行解析json, jq解析 json 实例](https://justcode.ikeepstudying.com/2018/02/shell%EF%BC%9A%E6%97%A0%E6%AF%94%E5%BC%BA%E5%A4%A7%E7%9A%84shell%E4%B9%8Bjson%E8%A7%A3%E6%9E%90%E5%B7%A5%E5%85%B7jq-linux%E5%91%BD%E4%BB%A4%E8%A1%8C%E8%A7%A3%E6%9E%90json-jq%E8%A7%A3%E6%9E%90-json/) 

