---
title: Pytest Allure Jenkins 持续集成
date: 2018-11-27 09:40:43
tags:
---

**安装Jenkins插件**：

- Allure
  - [Allure Jenkins Plugin](http://wiki.jenkins-ci.org/display/JENKINS/Allure+Plugin)
- GitLab
  - [GitLab Logo Plugin](https://wiki.jenkins-ci.org/display/JENKINS/Gitlab+Logo+Plugin)
  - [GitLab Plugin](https://wiki.jenkins-ci.org/display/JENKINS/GitLab+Plugin)
  - [Gitlab Authentication](https://plugins.jenkins.io/gitlab-oauth)
  - [Gitlab Hook](https://plugins.jenkins.io/gitlab-hook)

访问gitlab API

```
https://${DOMAIN}/api/v4/projects?private_token=${API_token}
```

**新建job**

New Item->example_pytest_allure(Freestyle project)



Source Code Management

- Git
  - Repository URL
  - Credentials
    - Kind: SSH Username with private key
    - Username: git
    - Private Key: 



## 参考

[pytest自动化测试框架（6）-pytest+Allure+jenkins集成](https://qizhenjun.com/blog/37ea8921/)

[使用gitlab API](https://www.jianshu.com/p/50d58fa8bdc6)

[搭建GitLab+Jenkins持续集成环境图文教程](https://blog.csdn.net/ruangong1203/article/details/73065410)

[python+allure+jenkins](https://testerhome.com/topics/10422)