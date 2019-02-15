---
title: YAML 学习笔记
date: 2019-02-15 07:12:03
tags:
---

### Python 使用 YAML

**安装 PyYAML**

```shell
pip install pyyaml==3.13
```



## 语法

- `#` 表示注释，从这个字符一直到行尾，都会被解析器忽略。

- `key: value` 对象的一组键值对。
- `- list1` 数组，`key: [value1, value2]`行内表示法。
- 纯量
  - 字符串
  - 布尔值
  - 整数
  - 浮点数
  - Null
  - 时间
  - 日期
- `!! str 123`(整数123转换为字符串)YAML 允许使用两个感叹号，强制转换数据类型。

- 多行字符串可以使用`|`保留换行符，也可以使用`>`折叠换行。
- 锚点`&`和别名`*`，可以用来引用；`&`用来建立锚点，`<<`表示合并到当前数据，`*`用来引用锚点。（类似于变量）
- 函数和正则表达式的转换



## 参考

[The Official YAML Web Site](https://yaml.org/)

[YAML 语言教程 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2016/07/yaml.html)



PyYAML

[https://pyyaml.org](https://pyyaml.org/)

[PyYAML Documentation](https://pyyaml.org/wiki/PyYAMLDocumentation) 查找关键字 `syntax`；`YAML tags and Python types`