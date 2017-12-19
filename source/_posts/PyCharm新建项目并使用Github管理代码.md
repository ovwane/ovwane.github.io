- 在Github上申请token

[Github Personal access tokens](https://github.com/settings/tokens)

- pycharm 设置github token
Preferences->Version Control->GIthub

- pycharm上新建project，名字 python_crawler。

```
cd /Users/jinlong/PycharmProjects/python_crawler

pipenv --python=/Users/jinlong/.pyenv/versions/3.6.3/bin/python

sed -i "" "s/python.org/douban.com/g" Pipfile
```

- 设置pycharm的python解释器
Preferences->Project:python_crawler->Project Interpreter

- 设置pycharm下的github

VCS->Import into Version Control->Share Project On Github

## PyCharm 配置
- Tab键设置成4个空格
Preferences->Editor->Code Style->Python->Use tab character 取消勾选

- 调整字母长度分割线
Preferences->Editor->Code Style->Right margin（columns) 设置为80


[Pycharm安装、设置、优化](https://www.cnblogs.com/hester/p/5466579.html)

[Google 开源项目风格指南-Python 风格指南 - 内容目录](http://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/contents/)

## PyCharm插件
Mongo Plugin

Markdown Navigator
Markdown support


## Python工具

nmap
`pip install python-nmap`