date: 2018.04.05 05：50

01.就业指导-简历制作.mp4 16：35
02.就业指导-简历制作2.mp4 1：05：29
03.就业指导-简历制作3.mp4 35：42
04.就业指导-简历制作4.mp4 16：59
06.就业指导-如何面试2.mp4 55：59
07.就业指导-如何面试3.mp4 30：53
08.就业指导-最后的鸡汤.mp4 36：59
09.天帅分享：Nginx+uWSGI+Django部署.mp4 9：52

开始日期 2017.10.20 0：15


## 09
结束时间 2017.10.20 07：49

自我介绍

天帅 运维开发

1、我们开发项目是为了什么
	给其他人使用
2、怎么给其他人使用
	runserver
3、我想给其他人使用怎么办？
	1、买台服务器
	2、Python环境
	3、把代码上传到服务器上
	4、runserver
	这一套流程，我们称之为"部署"
	

Django 内部有socket server 但是性能比较差

使用uWSGI

uWSGI + Django
安装 uwsgi
pip install uwsgi

使用命令启动
uwsgi --http --file django/wsgi.py --static-map=/static=static



视频不完整 