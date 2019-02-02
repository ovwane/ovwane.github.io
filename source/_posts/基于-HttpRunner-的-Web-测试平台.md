---
title: 基于 HttpRunner 的 Web 测试平台
date: 2019-01-24 07:57:23
tags:
---

# [基于 HttpRunner 的 Web 测试平台](https://github.com/HttpRunner/HttpRunnerManager)

### MySQL

安装 

### [RabbitMQ](https://hub.docker.com/_/rabbitmq?tab=tags)

安装 

```shell
docker pull rabbitmq:3.7.8-alpine
```

运行

```shell
docker run -d --hostname rabbit.ovwane.com --name rabbitmq-3.7.8 -p 5672:5672 rabbitmq:3.7.8-alpine
```

```shell
docker run -d --hostname rabbit.ovwane.com --name rabbitmq-management-3.7.8 -p 5672:5672 -p 15672:15672 rabbitmq:3.7.8-management
```

访问

http://127.0.0.1:15672

用户名：`guest`，密码：`guest`



## 本地开发环境部署

1. 安装mysql数据库服务端(推荐5.7+),并设置为utf-8编码，创建相应HttpRunner数据库，设置好相应用户名、密码，启动mysql。
2. 修改:HttpRunnerManager/HttpRunnerManager/settings.py里DATABASES字典和邮件发送账号相关配置

```
     DATABASES = {
         'default': {
         'ENGINE': 'django.db.backends.mysql',
         'NAME': 'HttpRunner',  # 新建数据库名
         'USER': 'root',  # 数据库登录名
         'PASSWORD': 'lcc123456',  # 数据库登录密码
         'HOST': '127.0.0.1',  # 数据库所在服务器ip地址
         'PORT': '3306',  # 监听端口 默认3306即可
     }
 }

 EMAIL_SEND_USERNAME = 'username@163.com'  # 定时任务报告发送邮箱，支持163,qq,sina,企业qq邮箱等，注意需要开通smtp服务
 EMAIL_SEND_PASSWORD = 'password'     # 邮箱密码
```

3. 安装rabbitmq消息中间件，启动服务，访问：<http://host:15672/#/> host即为你部署rabbitmq的服务器ip地址 username：guest、Password：guest, 成功登陆即可

```
    service rabbitmq-server start
```

4. 修改:HttpRunnerManager/HttpRunnerManager/settings.py里worker相关配置

```
    djcelery.setup_loader()
    CELERY_ENABLE_UTC = True
    CELERY_TIMEZONE = 'Asia/Shanghai'
    BROKER_URL = 'amqp://guest:guest@127.0.0.1:5672//'  # 127.0.0.1即为rabbitmq-server所在服务器ip地址
    CELERYBEAT_SCHEDULER = 'djcelery.schedulers.DatabaseScheduler'
    CELERY_RESULT_BACKEND = 'djcelery.backends.database:DatabaseBackend'
    CELERY_ACCEPT_CONTENT = ['application/json']
    CELERY_TASK_SERIALIZER = 'json'
    CELERY_RESULT_SERIALIZER = 'json'

    CELERY_TASK_RESULT_EXPIRES = 7200  # celery任务执行结果的超时时间，
    CELERYD_CONCURRENCY = 10  # celery worker的并发数 也是命令行-c指定的数目 根据服务器配置实际更改 默认10
    CELERYD_MAX_TASKS_PER_CHILD = 100  # 每个worker执行了多少任务就会死掉，我建议数量可以大一些，默认100
```

5. 命令行窗口执行pip install -r requirements.txt 安装工程所依赖的库文件
6. 命令行窗口切换到HttpRunnerManager目录 生成数据库迁移脚本,并生成表结构

```
python manage.py makemigrations ApiManager #生成数据迁移脚本
python manage.py migrate  #应用到db生成数据表
```

7. 创建超级用户，用户后台管理数据库，并按提示输入相应用户名，密码，邮箱。 如不需用，可跳过此步骤

```
python manage.py createsuperuser
```

8. 启动服务,

```
python manage.py runserver 0.0.0.0:8000
```

9. 启动worker, 如果选择同步执行并确保不会使用到定时任务，那么此步骤可忽略

```
python manage.py celery -A HttpRunnerManager worker --loglevel=info  #启动worker
python manage.py celery beat --loglevel=info #启动定时任务监听器
celery flower #启动任务监控后台
```

10. 访问：<http://localhost:5555/dashboard> 即可查看任务列表和状态
11. 浏览器输入：<http://127.0.0.1:8000/api/register/> 注册用户，开始尽情享用平台吧
12. 浏览器输入<http://127.0.0.1:8000/admin/> 输入步骤6设置的用户名、密码，登录后台运维管理系统，可后台管理数据