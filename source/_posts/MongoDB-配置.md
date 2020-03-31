---
title: MongoDB 配置
date: 2018-08-08 16:47:48
---

# [MongoDB](https://github.com/mongodb/mongo)



## 安装

 [mongo](https://hub.docker.com/_/mongo)

拉取

```shell
docker pull mongo:4.2.0
```



运行

```
docker run -d --name mongodb-4.2.0 -p 27017:27017 -v ~/docker/mongodb:/data/db mongo:4.2.0
```



## 安装 Mongo shell

### macOS

> mongodb-community-shell 提供 mongo 命令。
>
> mongodb-database-tools 提供数据库导入导出命令。
>
> https://github.com/mongodb/homebrew-brew/blob/HEAD/Formula/mongodb-database-tools.rb

```bash
brew tap mongodb/brew && brew install mongodb-community-shell && brew install mongodb-database-tools
```



### CentOS

```
yum install mongodb-org-shell-4.4.0
```

> https://docs.mongodb.com/manual/tutorial/install-mongodb-on-red-hat/



## 配置多副本集

https://docs.mongodb.com/manual/tutorial/deploy-replica-set/

https://blog.yowko.com/docker-compose-mongodb-replica-set-with-auth/

```
openssl rand -base64 756 > keyfile
chmod 400 keyfile
# 更改用户为999
chown 999:999 keyfile
```

.env

```
work_dir=.
username=hogwarts
password=5Po5yaoXWAbu
```



## 用户管理

Mongodb中角色的定义：
角色说明：

- read：允许用户读取指定数据库 
- readWrite：允许用户读写指定数据库 
- dbAdmin：允许用户在指定数据库中执行管理函数，如索引创建、删除，查看统计或访问system.profile 
- userAdmin：允许用户向system.users集合写入，可以找指定数据库里创建、删除和管理用户 
- clusterAdmin：只在admin数据库中可用，赋予用户所有分片和复制集相关函数的管理权限。 
- readAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读权限 
- readWriteAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读写权限 
- userAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的userAdmin权限 
- dbAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的dbAdmin权限。 
- root：只在admin数据库中可用。超级账号，超级权限




添加管理员账号

```mongo
db.createUser(    {
        user: "root",
        pwd: "UbM43zUgecBnb8v8MLiu",
        roles: [ { role: "root", db: "admin" } ]
   }
  ) 
```



查看用户

```
show users
db.system.users.find()
```



登录

```shell
mongo -u "root" -p "admin" --authenticationDatabase "admin"
```



数据库中登陆

```mongo
use admin
db.auth("root", "admin")
```



修改用户密码

```mongo
db.changeUserPassword('root','密码')
```



## 常用操作

show dbs  #显示数据库列表 
show collections  #显示当前数据库中的集合（类似关系数据库中的表）
use <db name>  #切换当前数据库，如果数据库不存在则创建数据库。 
db.help()  #显示数据库操作命令，里面有很多的命令 
db.foo.help()  #显示集合操作命令，同样有很多的命令，foo指的是当前数据库下，一个叫foo的集合，并非真正意义上的命令 
db.foo.find()  #对于当前数据库中的foo集合进行数据查找（由于没有条件，会列出所有数据） 
db.foo.find( { a : 1 } )  #对于当前数据库中的foo集合进行查找，条件是数据中有一个属性叫a，且a的值为1
db.dropDatabase()  #删除当前使用数据库
db.cloneDatabase("127.0.0.1")   #将指定机器上的数据库的数据克隆到当前数据库
db.copyDatabase("mydb", "temp", "127.0.0.1")  #将本机的mydb的数据复制到temp数据库中
db.repairDatabase()  #修复当前数据库
db.getName()  #查看当前使用的数据库，也可以直接用db
db.stats()  #显示当前db状态
db.version()  #当前db版本
db.getMongo()  ＃查看当前db的链接机器地址
db.serverStatus()  #查看数据库服务器的状态



## 数据备份

```
brew install mongodb/brew/mongodb-database-tools
```



备份所有数据

```
mongodump --host 127.0.0.1:27017 -d stu_info -o ${PWD}
```

>-d 指定数据库
>
>-o 导出路径



恢复数据

```
mongorestore -h 127.0.0.1:27017 -u admin -p 123 --authenticationDatabase "admin" -d stu_info ${PWD}/stu_info
```



## 增删改查

https://docs.mongodb.com/manual/reference/program/mongo/#cmdoption-mongo-eval

https://docs.mongodb.com/manual/crud/

插入数据

```
mongo 127.0.0.1:27017/test --eval 'db.p12.insertOne({"_id":2,name:"1",phone:"13555002520"})'
```



查询所有数据

```
mongo --quiet 127.0.0.1:27017/test --eval 'db.p12.find().forEach(printjson)'
```



查询数据

```
mongo --quiet 127.0.0.1:27017/test --eval 'db.p12.find({"email" : "1212@qq.com"})'
```



更新数据

```
table_name=p12
mongo --quiet 127.0.0.1:27017/test --eval 'db.'$table_name'.update({"email" : "123@qq.com"},{$set:{"emails":"123@qq.com"}})'
```



## 参考

[Docker MongoDB 部署](https://www.jianshu.com/p/6fdb2bcb4b43)

[Install MongoDB Community Edition on Red Hat Enterprise or CentOS Linux[¶]](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-red-hat/)

[MongoDB Configuration File Options](https://docs.mongodb.com/manual/reference/configuration-options/)

[安全部署MongoDB最佳实践](http://www.mongoing.com/archives/631)

[Mongodb之权限认证管理](http://blog.51cto.com/hld1992/2074619)

[Enable Auth](https://docs.mongodb.com/manual/tutorial/enable-authentication/)

[如何对MongoDB 3.2.7进行用户权限管理配置](https://www.jianshu.com/p/a4e94bb8a052)

[MongoDB 用户的创建、删除、密码修改，角色分配](http://www.itttl.com/blog/mongodb_user_roles.html)

[mongodb 修改用户密码 2种方法](http://blog.51yip.com/nosql/1576.html)

[mongodb启动不了：child process failed, exited with error number 100](https://blog.csdn.net/sinat_30397435/article/details/50774175)

https://blog.yowko.com/docker-compose-mongodb-replica-set/