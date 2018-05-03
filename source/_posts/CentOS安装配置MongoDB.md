```shell
rpm --import https://www.mongodb.org/static/pgp/server-3.6.asc
```

```
cat >/etc/yum.repos.d/mongodb-org-3.6.repo<<EOF
[mongodb-org-3.6]
name=MongoDB Repository
baseurl=https://repo.mongodb.org/yum/redhat/$releasever/mongodb-org/3.6/x86_64/
gpgcheck=1
enabled=1
gpgkey=https://www.mongodb.org/static/pgp/server-3.6.asc
EOF
```

```shell
yum -y install mongodb-org-3.6.4 mongodb-org-server-3.6.4 mongodb-org-shell-3.6.4 mongodb-org-tools-3.6.4 mongodb-org-mongos-3.6.4
```

配置

```shell
mkdir -p /data/mongodb/db
chown mongod:mongod /data/mongodb/db
```

/etc/mongod.conf

```
systemLog:
  destination: file
  logAppend: true
  path: /data/mongodb/mongod.log

storage:
  dbPath: /data/mongodb/db
  journal:
    enabled: true

processManagement:
  fork: true  # fork and run in background
  pidFilePath: /data/mongodb/mongod.pid  # location of pidfile
  timeZoneInfo: /usr/share/zoneinfo
  
net:
  port: 27017
  bindIp: 127.0.0.1
```

```shell
systemctl enable mongod.service
systemctl start mongod.service
systemctl restart mongod.service
systemctl status mongod.service
systemctl stop mongod.service
```

### 开启权限验证

下Mongodb中角色的定义：
角色说明：

```
read：允许用户读取指定数据库 
readWrite：允许用户读写指定数据库 
dbAdmin：允许用户在指定数据库中执行管理函数，如索引创建、删除，查看统计或访问system.profile 
userAdmin：允许用户向system.users集合写入，可以找指定数据库里创建、删除和管理用户 
clusterAdmin：只在admin数据库中可用，赋予用户所有分片和复制集相关函数的管理权限。 
readAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读权限 
readWriteAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读写权限 
userAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的userAdmin权限 
dbAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的dbAdmin权限。 
root：只在admin数据库中可用。超级账号，超级权限
```

添加管理员账号

```mongo
db.createUser(    {
        user: "root",
        pwd: "UbM43zUgecBnb8v8MLiu",
        roles: [ { role: "root", db: "admin" } ]
   }
  ) 

#查看用户
show users
db.system.users.find()
```



```mongo
db.createUser(
   {
     user: "admin",
     pwd: "mongodb:passok",
     roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
   }
)
```

### 添加普通用户

```
db.createUser(
   {
     user: "test",
     pwd: "123456",
     roles: [ { role: "readWrite", db: "test" }],
   }
 )
```

```shell
vim /etc/mongod.conf

security:
  authorization: enabled
```

```shell
#重启
systemctl restart mongod.service

#登陆
mongo -u "admin" -p "密码" --authenticationDatabase "admin"
```

```mongo
#数据库中登陆
use admin
db.auth("admin", "密码")
```

修改用户密码

```mongo
db.changeUserPassword('root','密码')
```



操作

```mongo
> show dbs  #显示数据库列表 
> show collections  #显示当前数据库中的集合（类似关系数据库中的表）
> show users  #显示用户
> use <db name>  #切换当前数据库，如果数据库不存在则创建数据库。 
> db.help()  #显示数据库操作命令，里面有很多的命令 
> db.foo.help()  #显示集合操作命令，同样有很多的命令，foo指的是当前数据库下，一个叫foo的集合，并非真正意义上的命令 
> db.foo.find()  #对于当前数据库中的foo集合进行数据查找（由于没有条件，会列出所有数据） 
> db.foo.find( { a : 1 } )  #对于当前数据库中的foo集合进行查找，条件是数据中有一个属性叫a，且a的值为1
> db.dropDatabase()  #删除当前使用数据库
> db.cloneDatabase("127.0.0.1")   #将指定机器上的数据库的数据克隆到当前数据库
> db.copyDatabase("mydb", "temp", "127.0.0.1")  #将本机的mydb的数据复制到temp数据库中
> db.repairDatabase()  #修复当前数据库
> db.getName()  #查看当前使用的数据库，也可以直接用db
> db.stats()  #显示当前db状态
> db.version()  #当前db版本
> db.getMongo()  ＃查看当前db的链接机器地址
> db.serverStatus()  #查看数据库服务器的状态
```

### 那么如何正常关闭mongodb？

可以去看官方文档：
<http://docs.mongodb.org/manual/tutorial/manage-mongodb-processes/>

先通过shell连上服务器：

```
mongo

use admin

db.shutdownServer()
```

错误

```
WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.

We suggest setting it to 'never'
```



参考

[Install MongoDB Community Edition on Red Hat Enterprise or CentOS Linux[¶]](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-red-hat/)

[MongoDB Configuration File Options](https://docs.mongodb.com/manual/reference/configuration-options/)

[安全部署MongoDB最佳实践](http://www.mongoing.com/archives/631)

[Mongodb之权限认证管理](http://blog.51cto.com/hld1992/2074619)

[Enable Auth](https://docs.mongodb.com/manual/tutorial/enable-authentication/)

[如何对MongoDB 3.2.7进行用户权限管理配置](https://www.jianshu.com/p/a4e94bb8a052)

[[MongoDB 用户的创建、删除、密码修改，角色分配](http://www.itttl.com/blog/mongodb_user_roles.html)]

[mongodb 修改用户密码 2种方法](http://blog.51yip.com/nosql/1576.html)

[mongodb启动不了：child process failed, exited with error number 100](https://blog.csdn.net/sinat_30397435/article/details/50774175)