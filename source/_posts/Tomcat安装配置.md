---
title: Tomcat安装配置
date: 2016-03-07 20:15:00
categories:
- 技术
- Linux
tags:
- CentOS
- Web
- Tomcat
---
[Tomcat安装配置](http://www.zyops.com/java-tomcat)

## 安装Tomcat
- [JDK安装配置](JDK安装配置.md)

- 下载 [Apache tomcat](http://tomcat.apache.org/)

- 安装Tomcat

```bash
#解压缩tomcat
tar xf apache-tomcat-8.5.23.tar.gz -C /usr/local/
#软链接
ln -s /usr/local/apache-tomcat-8.5.23 /usr/local/tomcat
#添加系统变量
echo 'export TOMCAT_HOME=/usr/local/tomcat'>>/etc/profile
#立即生效系统变量
source /etc/profile
#更改tomcat的权限
chown -R root:root /usr/local/apache-tomcat-8.5.23
```

## 启动Tomcat
```bash
#启动程序
tomcat/bin/startup.sh
#关闭程序
tomcat/bin/shutdown.sh
#查看网络
netstat -tunlp|grep java
#查看进程
ps -ef|grep [j]ava
```

访问网站
网址：http://IP:8080/

Tomcat日志

```bash
#进入日志目录
cd tomcat/logs/
#tomcat实时运行日志
tail -f catalina.out
#访问日志
tail -f 域名_access_log.日期.txt
```

Tomcat配置文件

```bash
cd tomcat/conf

#主配置文件
server.xml
#Tomcat管理用户配置文件
tomcat-users.xml
```

站点目录

```bash
cd tomcat/webapps/ROOT
```





## Tomcat简介
Tomcat是Apache软件基金会（Apache Software Foundation）的Jakarta 项目中的一个核心项目，由Apache、Sun和其他一些公司及个人共同开发而成。

Tomcat服务器是一个免费的开放源代码的Web应用服务器，属于轻量级应用服务器，在中小型系统和并发访问用户不是很多的场合下被普遍使用，是开发和调试JSP程序的首选。

Tomcat和Nginx、Apache(httpd)、lighttpd等Web服务器一样，具有处理HTML页面的功能，另外它还是一个Servlet和JSP容器，独立的Servlet容器是Tomcat的默认模式。不过，Tomcat处理静态HTML的能力不如Nginx/Apache服务器。

>目前Tomcat最新版本为9.0。Java容器应用还有resin、weblogic等。

## Tomcat目录介绍
```
cd /application/tomcat/

tree -L 1
.
├── bin         #→用以启动、关闭Tomcat或者其它功能的脚本（.bat文件和.sh文件）
├── conf        #→用以配置Tomcat的XML及DTD文件
├── lib         #→存放web应用能访问的JAR包
├── LICENSE
├── logs        #→Catalina和其它Web应用程序的日志文件
├── NOTICE
├── RELEASE-NOTES
├── RUNNING.txt
├── temp        #→临时文件
├── webapps     #→Web应用程序根目录
└── work        #→用以产生有JSP编译出的Servlet的.java和.class文件
7 directories, 4 files

cd webapps/

ll

total 20
drwxr-xr-x 14 root root 4096 Oct  5 12:09 docs     #→tomcat帮助文档
drwxr-xr-x  6 root root 4096 Oct  5 12:09 examples #→web应用实例
drwxr-xr-x  5 root root 4096 Oct  5 12:09 host-manager #→管理
drwxr-xr-x  5 root root 4096 Oct  5 12:09 manager  #→管理
drwxr-xr-x  3 root root 4096 Oct  5 12:09 ROOT     #→默认网站根目录
```



## Tomcat管理
测试功能，生产环境不要用。

Tomcat管理功能用于对Tomcat自身以及部署在Tomcat上的应用进行管理的web应用。在默认情况下是处于禁用状态的。如果需要开启这个功能，就需要配置管理用户，即配置前面说过的tomcat-users.xml。

```
[root@tomcat ~]# vim /application/tomcat/conf/tomcat-users.xml
…………
<role rolename="manager-gui"/>
<role rolename="admin-gui"/>
<user username="tomcat" password="tomcat" roles="manager-gui,admin-gui"/>
</tomcat-users>  #→在此行前加入上面三行
[root@tomcat ~]# /application/tomcat/bin/shutdown.sh
[root@tomcat ~]# /application/tomcat/bin/startup.sh
```

## Tomcat主配置文件server.xml详解
- 顶级组件：位于整个配置的顶层，如server。
- 容器类组件：可以包含其它组件的组件，如service、engine、host、context。
- 连接器组件：连接用户请求至tomcat，如connector。
- 被嵌套类组件：位于一个容器当中，不能包含其他组件，如Valve、logger。

组件详解
***

engine：核心容器组件，catalina引擎，负责通过connector接收用户请求，并处理请求，将请求转至对应的虚拟主机host。
host：类似于httpd中的虚拟主机，一般而言支持基于FQDN的虚拟主机。
context：定义一个应用程序，是一个最内层的容器类组件（不能再嵌套）。配置context的主要目的指定对应对的webapp的根目录，类似于httpd的alias，其还能为webapp指定额外的属性，如部署方式等。
connector：接收用户请求，类似于httpd的listen配置监听端口的。
service（服务）：将connector关联至engine，因此一个service内部可以有多个connector，但只能有一个引擎engine。service内部有两个connector，一个engine。因此，一般情况下一个server内部只有一个service，一个service内部只有一个engine，但一个service内部可以有多个connector。
server：表示一个运行于JVM中的tomcat实例。
Valve：阀门，拦截请求并在将其转至对应的webapp前进行某种处理操作，可以用于任何容器中，比如记录日志(access log valve)、基于IP做访问控制(remote address filter valve)。
logger：日志记录器，用于记录组件内部的状态信息，可以用于除context外的任何容器中。
realm：可以用于任意容器类的组件中，关联一个用户认证库，实现认证和授权。可以关联的认证库有两种：UserDatabaseRealm、MemoryRealm和JDBCRealm。
UserDatabaseRealm：使用JNDI自定义的用户认证库。
MemoryRealm：认证信息定义在tomcat-users.xml中。
JDBCRealm：认证信息定义在数据库中，并通过JDBC连接至数据库中查找认证用户。
***

配置文件注释

```xml
<?xml version='1.0' encoding='utf-8'?>
<!--
<Server>元素代表整个容器,是Tomcat实例的顶层元素.由org.apache.catalina.Server接口来定义.它包含一个<Service>元素.并且它不能做为任何元素的子元素.
    port指定Tomcat监听shutdown命令端口.终止服务器运行时,必须在Tomcat服务器所在的机器上发出shutdown命令.该属性是必须的.
    shutdown指定终止Tomcat服务器运行时,发给Tomcat服务器的shutdown监听端口的字符串.该属性必须设置
-->
<Server port="8005" shutdown="SHUTDOWN">
  <Listener className="org.apache.catalina.startup.VersionLoggerListener" />
  <Listener className="org.apache.catalina.core.AprLifecycleListener" SSLEngine="on" />
  <Listener className="org.apache.catalina.core.JreMemoryLeakPreventionListener" />
  <Listener className="org.apache.catalina.mbeans.GlobalResourcesLifecycleListener" />
  <Listener className="org.apache.catalina.core.ThreadLocalLeakPreventionListener" />
  <GlobalNamingResources>
    <Resource name="UserDatabase" auth="Container"
              type="org.apache.catalina.UserDatabase"
              description="User database that can be updated and saved"
              factory="org.apache.catalina.users.MemoryUserDatabaseFactory"
              pathname="conf/tomcat-users.xml" />
  </GlobalNamingResources>
  <!--service服务组件-->
  <Service name="Catalina">
    <!--
    connector：接收用户请求，类似于httpd的listen配置监听端口.
        port指定服务器端要创建的端口号，并在这个端口监听来自客户端的请求。
        address：指定连接器监听的地址，默认为所有地址（即0.0.0.0）
        protocol连接器使用的协议，支持HTTP和AJP。AJP（Apache Jserv Protocol）专用于tomcat与apache建立通信的， 在httpd反向代理用户请求至tomcat时使用（可见Nginx反向代理时不可用AJP协议）。
        minProcessors服务器启动时创建的处理请求的线程数
        maxProcessors最大可以创建的处理请求的线程数
        enableLookups如果为true，则可以通过调用request.getRemoteHost()进行DNS查询来得到远程客户端的实际主机名，若为false则不进行DNS查询，而是返回其ip地址
        redirectPort指定服务器正在处理http请求时收到了一个SSL传输请求后重定向的端口号
        acceptCount指定当所有可以使用的处理请求的线程数都被使用时，可以放到处理队列中的请求数，超过这个数的请求将不予处理
        connectionTimeout指定超时的时间数(以毫秒为单位)
    -->
    <Connector port="8080" protocol="HTTP/1.1"
               connectionTimeout="20000"
               redirectPort="8443" />
    <Connector port="8009" protocol="AJP/1.3" redirectPort="8443" />
    <!--engine,核心容器组件,catalina引擎,负责通过connector接收用户请求,并处理请求,将请求转至对应的虚拟主机host
        defaultHost指定缺省的处理请求的主机名，它至少与其中的一个host元素的name属性值是一样的
    -->
    <Engine name="Catalina" defaultHost="localhost">
      <!--Realm表示存放用户名，密码及role的数据库-->
      <Realm className="org.apache.catalina.realm.LockOutRealm">
        <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
               resourceName="UserDatabase"/>
      </Realm>
      <!--
      host表示一个虚拟主机
        name指定主机名
        appBase应用程序基本目录，即存放应用程序的目录.一般为appBase="webapps" ，相对于CATALINA_HOME而言的，也可以写绝对路径。
        unpackWARs如果为true，则tomcat会自动将WAR文件解压，否则不解压，直接从WAR文件中运行应用程序
        autoDeploy：在tomcat启动时，是否自动部署。
        xmlValidation：是否启动xml的校验功能，一般xmlValidation="false"。
        xmlNamespaceAware：检测名称空间，一般xmlNamespaceAware="false"。
      -->
      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">
        <!--
        Context表示一个web应用程序，通常为WAR文件
            docBase应用程序的路径或者是WAR文件存放的路径,也可以使用相对路径，起始路径为此Context所属Host中appBase定义的路径。
            path表示此web应用程序的url的前缀，这样请求的url为http://localhost:8080/path/****
            reloadable这个属性非常重要，如果为true，则tomcat会自动检测应用程序的/WEB-INF/lib 和/WEB-INF/classes目录的变化，自动装载新的应用程序，可以在不重启tomcat的情况下改变应用程序
        -->
        <Context path="" docBase="" debug=""/>
        <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
               prefix="localhost_access_log" suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />
      </Host>
    </Engine>
  </Service>
</Server>
```

# #WEB站点部署
上线的代码有两种方式，第一种方式是直接将程序目录放在webapps目录下面，这种方式大家已经明白了，就不多说了。第二种方式是使用开发工具将程序打包成war包，然后上传到webapps目录下面。下面让我们见识一下这种方式。

- 使用war包部署web站点

```
[root@tomcat webapps]# pwd
/application/tomcat/webapps
[root@tomcat webapps]# rz  #→上传memtest.war，此文件也在上面的百度网盘里
[root@tomcat webapps]# ls
docs  examples  host-manager  manager  memtest  memtest.war  ROOT
```

浏览器访问：http://10.0.0.3:8080/memtest/meminfo.jsp

- 自定义默认网站目录
上面访问的网址为http://10.0.0.3:8080/memtest/meminfo.jsp 
现在我想访问格式为http://10.0.0.3:8080/meminfo.jsp 
怎么破？

方法一

将meminfo.jsp或其他程序放在tomcat/webapps/ROOT目录下即可。因为默认网站根目录为tomcat/webapps/ROOT

方法二

```
[root@tomcat ~]# vim /application/tomcat/conf/server.xml
      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">
         <Context path="" docBase="/application/tomcat/webapps/memtest" debug="0" reloadable="false" crossContext="true"/>
[root@tomcat ~]# /application/tomcat/bin/shutdown.sh
[root@tomcat ~]# /application/tomcat/bin/startup.sh
```

## Tomcat多实例及集群架构
- Tomcat多实例

1. 复制Tomcat目录

 ```
[root@tomcat ~]# cd /application/
[root@tomcat application]# cp -a apache-tomcat-8.0.27 tomcat8_1
[root@tomcat application]# cp -a apache-tomcat-8.0.27 tomcat8_2
 ```

1. 修改配置文件

```
[root@tomcat application]# mkdir -p /data/www/www/ROOT
[root@tomcat application]# cp /application/tomcat/webapps/memtest/meminfo.jsp /data/www/www/ROOT/
[root@tomcat ~]# sed -i '22s#8005#8011#;69s#8080#8081#;123s#appBase=".*"# appBase="/data/www/www"#' /application/tomcat8_1/conf/server.xml
[root@tomcat ~]# sed -i '22s#8005#8012#;69s#8080#8082#;123s#appBase=".*"# appBase="/data/www/www"#' /application/tomcat8_2/conf/server.xml
[root@tomcat ~]# diff /application/tomcat/conf/server.xml  /application/tomcat8_1/conf/server.xml   
22c22
< <Server port="8005" shutdown="SHUTDOWN">
---
> <Server port="8011" shutdown="SHUTDOWN">
69c69
<     <Connector port="8080" protocol="HTTP/1.1"
---
>     <Connector port="8081" protocol="HTTP/1.1"
123c123
<       <Host name="localhost"  appBase="/application/tomcat/webapps/memtest"
---
>       <Host name="localhost"   appBase="/data/www/www"
[root@tomcat ~]# diff /application/tomcat/conf/server.xml  /application/tomcat8_2/conf/server.xml
22c22
< <Server port="8005" shutdown="SHUTDOWN">
---
> <Server port="8012" shutdown="SHUTDOWN">
69c69
<     <Connector port="8080" protocol="HTTP/1.1"
---
>     <Connector port="8082" protocol="HTTP/1.1"
123c123
<       <Host name="localhost"  appBase="/application/tomcat/webapps/memtest"
---
>       <Host name="localhost"    appBase="/data/www/www"
```

3. 启动多实例

```
for i in {1..2};do /application/tomcat8_$i/bin/startup.sh;done
netstat -tunlp|grep java
```

浏览器可以分别访问http://10.0.0.3:8081/meminfo.jsp 和 http://10.0.0.3:8082/meminfo.jsp

## Tomcat集群
### 使用nginx+Tomcat反向代理集群
```
[root@tomcat ~]# vim /application/nginx/conf/nginx.conf
    upstream web_pools {
        server 127.0.0.1:8081;
        server 127.0.0.1:8082;
        }
    server {
        listen       80;
        server_name  localhost;
        location / {
            root   html;
            index  index.jsp index.html index.htm;
            proxy_pass http://web_pools;
        }
     }
[root@tomcat ~]# /application/nginx/sbin/nginx -t
[root@tomcat ~]# /application/nginx/sbin/nginx
```
浏览器可以访问http://10.0.0.3/meminfo.jsp

# Tomcat监控

# Tomcat安全优化和性能优化
## 安全优化
降权启动
telnet管理端口保护
ajp连接端口保护
禁用管理端

## 性能优化
### 屏蔽dns查询enableLookups="false"
```
    <Connector  port="8081" protocol="HTTP/1.1"
               connectionTimeout="6000" enableLookups="false" acceptCount="800"
               redirectPort="8443" />
```

## jvm调优
Tomcat最吃内存，只要内存足够，这只猫就跑的很快。
如果系统资源有限，那就需要进行调优，提高资源使用率。

```
优化catalina.sh配置文件。在catalina.sh配置文件中添加以下代码：
JAVA_OPTS="-Djava.awt.headless=true -Dfile.encoding=UTF-8 -server -Xms1024m -Xmx1024m -XX:NewSize=512m -XX:MaxNewSize=512m -XX:PermSize=512m -XX:MaxPermSize=512m"
server:一定要作为第一个参数，在多个CPU时性能佳
-Xms：初始堆内存Heap大小，使用的最小内存,cpu性能高时此值应设的大一些
-Xmx：初始堆内存heap最大值，使用的最大内存
上面两个值是分配JVM的最小和最大内存，取决于硬件物理内存的大小，建议均设为物理内存的一半。
-XX:PermSize:设定内存的永久保存区域
-XX:MaxPermSize:设定最大内存的永久保存区域
-XX:MaxNewSize:
-Xss 15120 这使得JBoss每增加一个线程（thread)就会立即消耗15M内存，而最佳值应该是128K,默认值好像是512k.
+XX:AggressiveHeap 会使得 Xms没有意义。这个参数让jvm忽略Xmx参数,疯狂地吃完一个G物理内存,再吃尽一个G的swap。
-Xss：每个线程的Stack大小
-verbose:gc 现实垃圾收集信息
-Xloggc:gc.log 指定垃圾收集日志文件
-Xmn：young generation的heap大小，一般设置为Xmx的3、4分之一
-XX:+UseParNewGC ：缩短minor收集的时间
-XX:+UseConcMarkSweepGC ：缩短major收集的时间
```



## 问题

Parallels Desktop 运行的CentOS 7.4.1708

jdk1.8.151
tomcat 8.5.23

重启tomcat 就打不开了。不知道什么原因。然后用vmware fusion就可以立刻重启。怀疑时paralles的网络有问题。但技术不行暂时找不到问题