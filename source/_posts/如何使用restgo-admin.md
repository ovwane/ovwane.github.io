date: 2018-05-08 11:34

# 如何使用[restgo-admin](https://github.com/winlion/restgo-admin/)

## 使用如下指令克隆

```
cd $GOPATH/src 
```

```
git clone https://github.com/winlion/restgo-admin.git 
```

你将得到restgo-admin 目录 进入目录 

```
cd restgo-admin
```

## 建立数据库

新建数据库名称为restgo-admin,编码为utf-8
将restgo-admin.sql导入到数据库中

## 初始化依赖包

使用前先使用如下指令安装指令安装文件

```shell
$ go get github.com/go-sql-driver/mysql
$ go get -v -u github.com/alecthomas/log4go
$ go get github.com/gin-gonic/gin
$ go get github.com/go-xorm/xorm
$ go get github.com/tommy351/gin-sessions
```

## 启动

使用前先使用如下指令启动应用
`go run main.go `
使用前先使用如下指令打包应用
`build.bat`

# 

 