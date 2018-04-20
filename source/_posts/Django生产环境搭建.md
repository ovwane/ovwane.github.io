```shell
pipenv install

pipenv install uwsgi

pipenv install django

pipenv install gunicorn
```

django 2.0.4

pip show django

uwsgi 2.0.17

pip show uwsgi

```
django-admin startproject teacher
```

使用的

mkdir -p /data/uwsgi/

```
vim /data/uwsgi/teacher.ini

[uwsgi]
# 项目目录
chdir=/root/py3/teacher
# 指定项目的application
module=teacher.wsgi:application
# 进程个数
workers=2
pidfile = /data/uwsgi/uwsgi_teacher.pid
# 指定IP端口
http=127.0.0.1:8001
# 指定静态文件
# static-map=/root/py3/teacher/static
# 启动uwsgi的用户名和用户组
uid=root
gid=root
# 启用主进程
master=true
# 自动移除unix Socket和pid文件当服务停止的时候
vacuum=true
# 序列化接受的内容，如果可能的话
thunder-lock=true
# 启用线程
enable-threads=true
# 设置自中断时间
harakiri=30
# 设置缓冲
post-buffering=4096
# 设置日志目录
daemonize=/data/uwsgi/teacher.log
# 指定sock的文件路径
socket=/data/uwsgi/teacher.sock
```

```
unix:/data/uwsgi.sock;
```

启动

```
# 启动uwsgi配置
uwsgi --ini /data/uwsgi/teacher.ini   

# 关闭uwsgi
uwsgi --stop uwsgi.pid  
killall -s INT /root/.virtualenv/py3-xzcW3AwM/bin/uwsgi

#重新加载配置
uwsgi --reload uwsgi.pid  
```

nginx配置

```
 # 指定项目路径uwsgi
        location /django/ {        # 这个location就和咱们Django的url(r'^admin/', admin.site.urls),
            include uwsgi_params;  # 导入一个Nginx模块他是用来和uWSGI进行通讯的
            uwsgi_connect_timeout 30;  # 设置连接uWSGI超时时间
            # 指定uwsgi的sock文件所有动态请求就会直接丢给他
            uwsgi_pass unix:/opt/project_teacher/script/uwsgi.sock;  
        }

        # 指定静态文件路径
        location /static/ {
            alias  /opt/project_teacher/teacher/static/;
            index  index.html index.htm;
        }
```

```


[uwsgi]
socket = 127.0.0.1:9090
master = true         //主进程
vhost = true          //多站模式
no-site = true        //多站模式时不设置入口模块和文件
workers = 2           //子进程数
reload-mercy = 10     
vacuum = true         //退出、重启时清理文件
max-requests = 1000   
limit-as = 512
buffer-size = 30000
pidfile = /var/run/uwsgi9090.pid    //pid文件，用于下面的脚本启动、停止该进程
daemonize = /data/log/uwsgi9090.log
```

```
[uwsgi]
chdir=/root/py3/teacher
module=teacher.wsgi:application
master=True
pidfile=/var/run/uwsgi9090.pid
vacuum=True
max-requests=5000
daemonize=/data/log/uwsgi9090.logi
```

```
[uwsgi]
chdir=/root/py3/teacher
uid=nobody
gid=nobody
module=teacher.wsgi:application
socket=/data/uwsgi.sock
master=true
workers=5
pidfile=/data/uwsgi.pid
vacuum=true
thunder-lock=true
enable-threads=true
harakiri=30
post-buffering=4096
daemonize=/data/uwsgi.log
```

### **django静态文件收集**

​    1.setting.py设置

```
DEBUG = False
STATIC_ROOT = os.path.join(BASE_DIR, 'statics')
```

​    2. 执行collectstatic命令：

```
python manage.py collectstatic
```

好处：django把静态文件拷贝到你设置的statics目录下（这样可以更方便的和nignx集成，权限管理也更方便）

### 参考

[Liunx之Ubuntu下Django+uWSGI+nginx部署](http://www.chenxm.cc/post/275.html)

[[Django + Uwsgi + Nginx 的生产环境部署](http://www.cnblogs.com/chenice/p/6921727.html)](https://www.cnblogs.com/chenice/p/6921727.html)

[如何配置nginx+uwsgi+django?](https://www.zhihu.com/question/27295854)˜

