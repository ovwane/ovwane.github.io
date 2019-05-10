---
title: 利用 Python 的 CGIHTTPServer 测试 CGI 脚本程序
date: 2019-04-04 16:54:38
tags:
---



## Python 3

步骤： 

1. 新建HttpServer目录 

   ```shell
   mkdir HttpServer
   cd HttpServer
   ```

2. 在HttpServer目录下新建cgi-bin目录 

   ```shell
   mkdir cgi-bin
   cd cgi-bin
   ```

3. 在cgi-bin目录下新建helloworld.sh文件，内容如下：

```shell
vim helloworld.sh
```

```shell
#!/bin/bash
echo "Content-Type:text/html"
echo ""

echo "hello world!"
```

4. 在cgi-bin目录下打开命令行，执行

```shell
chmod +x helloworld.sh
```

赋予helloworld.sh可执行的权限

5. 在HttpServer目录下打开命令行，执行命令

```shell
python -m http.server --cgi --bind 0.0.0.0 8088
```

> 就会把HttpServer目录以后台服务的方式作为CGIHTTPServer启动，运行log可在当前目录的nohup.out文件中查看。

6. 打开浏览器，地址栏出入：

http://localhost:8088/cgi-bin/helloworld.sh

即可看到输出的Hello World! 



## Python 2

步骤： 

1. 新建HttpServer目录 

2. 在HttpServer目录下新建cgi-bin目录 

3. 在cgi-bin目录下新建helloworld.sh文件，内容如下：

```shell
#!/bin/bash
echo "Content-Type:text/html"
echo ""

echo "hello world!"
```

4. 在cgi-bin目录下打开命令行，执行

```shell
chmod +x helloworld.sh
```

赋予helloworld.sh可执行的权限

5. 在HttpServer目录下打开命令行，执行命令

```shell
nohup python -m CGIHTTPServer 8088 &
```

> 就会把HttpServer目录以后台服务的方式作为CGIHTTPServer启动，运行log可在当前目录的nohup.out文件中查看。

6. 打开浏览器，地址栏出入：

http://localhost:8088/cgi-bin/helloworld.sh

即可看到输出的Hello World! 



## 参考

[利用Python的CGIHTTPServer测试CGI脚本程序](<https://blog.csdn.net/yzpbright/article/details/82626451>)