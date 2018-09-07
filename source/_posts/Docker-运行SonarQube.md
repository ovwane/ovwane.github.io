---
title: Docker 运行SonarQube
date: 2018-09-07 09:03:05
tags:
---

```shell
docker pull sonarqube:7.1
```

```shell
docker run -d --name sonarqube-7.1 -p 9000:9000 -p 9092:9092 sonarqube:7.1
```

启动后访问[http://localhost:9000](http://localhost:9000/)就可以进入sonar了, 默认管理员用户和密码是admin/admin

#### 安装插件

举个栗子，我们安装一个汉化插件：Chinese Pack

进入Administration->Marketplace

搜索`Chinese Pack`，点击install。

### 使用 Sonar maven插件进行代码解析

使用前提：

- 需要maven版本3.0.2及以上。
- 安装好SonarQube。
- 使用了已安装的SonarQube支持的最低的JDK。
- 已经安装好了你要分析的语言的插件。

## 参考

[使用 Sonar 进行代码质量管理](https://www.jianshu.com/p/4e6fd254045c)

[SonarQube代码质量检查工具](https://my.oschina.net/zzuqiang/blog/843406)

[Sonar入门学习](https://blog.csdn.net/wangjunjun2008/article/details/9407221)

