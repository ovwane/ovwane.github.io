---
title: macOS 10.13.3 配置pipenv
date: 2018-03-26 20:39:00
---

[Python Development Workflow for Humans](https://docs.pipenv.org/)
[pipenv](https://github.com/pypa/pipenv)

```bash
pip install pipenv
```

```
vim ~/.zshrc
#start pipenv
eval "$(pipenv --completion)"
#end
```

```bash
pipenv --python=/Users/jinlong/.pyenv/versions/3.6.4/bin/python
```

```
#显示安装全部过程
pipenv install -v 
```

```
pipenv graph
```

```
pipenv shell
```