---
title: Django学习
date: 2017-10-12 09:31:51
---

>>macOS 10.13
>Python 3.6.3
>Django 1.11.6
>PyMySQL 0.7.11
>mysqlclient 1.3.12

```
mkvirtualenv django_demo
workon django_demo

pip install Django
pip install mysqlclient
```

#### 设置数据库
##### settings.py
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': "django_demo",
        'USER': "root",
        'PASSWORD': 'root',
        'HOST': "127.0.0.1"
    }
}
```

#### 配置和迁移项目
```
manage.py makemigrations
manage.py migrate
```

#### 创建 app
manage.py startapp 