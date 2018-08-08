---
titlie: Django企业开发实战
date: 2017-10-17 22:54:00
---

[PYTHON 3 WALL OF SUPERPOWERS](https://python3wos.appspot.com/)
> 是否有人能够掌控开始使用框架之后遇到的所有问题。
> 你必须要做那个能解决所有问题的人。

>Too Choices Means No Choices

>不要担心，就是因为不会。才要学习。

两种提供Web服务的方式，分别是
一：直接通过socket来处理http请求; 
二：通过实现WSGI Application部分的协议。

WSGI，全称是Web Server Gateway Interface（Web服务网关接口）。

gunicorn
Gunicorn可以通过gevent/greenlet/gthread等来实现协程或者异步IO

Werkzeug

我们要做的还是带着需求去找框架，找技术栈，而不是拿着框架来做需求。

所以拿着需求去找框架，选择合适的框架，而不是功能最全的框架或者最先进的框架。


# 开发流程
1. 需求分析
2. 技术选型
3. 开发

>一起读一下文档

无论是MVC模式还是MTV模式，甚至是其他的什么模式，都是为了解耦。把一个软件系统划分为一层一层的结构，让每一层的逻辑更加纯粹，便于开发人员维护。

Django

Model在整个项目结构中是直接同数据库打交道的层，所以数据处理的部分都在这一层。在业务开发中，关于纯数据操作的部分，建议都放到这一层来做。

在View中，我们通过操作Model拿到数据，做一些业务上调整，然后把数据传递到模板中，最终渲染出来页面。

Template 这是Django声称对设计师友好的部分，因为它提供的语法很简单，任何人都可以很快上手，即便是不同编程的人，也可以很容易学习和使用。

Forms 对于传统的，需要通过form来提交数据的页面，Form还是挺好用的。Form是对html中Form表单的抽象。

>无论是Django文档，还是其他问题，甚至是这本书，都是仅供参考。正确的答案始终是在你电脑上的代码里。无论是因为，框架版本的原因，还是文档上的书写错误，或者是本书的书写错误，你电脑上的代码始终不会骗你。

创建项目

配置python环境

````
mkdir student_house && cd student_house
pipenv --python=/Users/jinlong/.pyenv/versions/2.7.14/bin/python

pipenv shell
pip install django==1.11.2
​```

​````bash
#创建项目
django-admin startproject student_sys

cd student_sys #所有操作都基于这个目录

#创建APP
python manage.py startapp student

# 1.Model
vim student/models.py

# 2.Admin
vim student/admin.py

# 3.student app放到settings.py中
vim student_sys/settings.py

INSTALLED_APPS = [
    'student',

# 4. 迁移
python manage.py makemigrations 创建迁移文件
python manage.py migrate 创建表
python manage.py createsuperuser 根据提示，输出用户名，邮箱，密码

# 5.基础配置

#开发首页
# 6.
vim student/views.py 

# 7.
mkdir student/templates
vim student/templates/index.html

# 8.
vim student_sys/urls.py

# 9.
vim student/forms.py 
​```
 
启动项目: python manage.py runserver

基础配置
vim student_sys/student_sys/settings.py

​```
LANGUAGE_CODE = 'zh-hans'  # 语言

TIME_ZONE = 'Asia/Shanghai'  # 时区

USE_I18N = True  # 语言

USE_L10N = True  # 数据和时间格式

USE_TZ = True  # 启用时区
​```

在admin中，展示带有choices属性的字段时，Django会自动帮我们调用get_status_display方法，所以我们不用配置。







````