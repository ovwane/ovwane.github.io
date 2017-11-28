title: Jenkins+Ansible+Gitlab自动化部署
date: 2016-03-15 15:21:20
categories:
- 技术
- Linux
tags:
- CentOS
- Jenkins
- Ansible
- Gitlab
---
[Jenkins+Ansible+Gitlab自动化部署](http://www.showerlee.com/archives/1880)

# 简介
- Ansible 首先给大家介绍的是Ansible, 恩, 重要的问题说三遍, 不是Saltstack, Ansible作为一个python写的自动化部署工具, 确实较之前我所接触的Chef, saltstack, puppet更有自己的一些优势, 首先就是agentless, 无需在Linux client安装任何服务即可无缝连接Linux default ssh端口进行部署(windows需要安装winrm 开启ssh服务), 这点其实我觉得非常重要, 可以想象很多公司本身是对network管理非常严格的, 在部署一个产品的同时你需要考虑很多时间成本, 使用其他部署工具本身非常棘手的问题就是去申请开端口, client量少的话, 我们可以去等, 多的话本身你去request, waiting, unblock port等等long long process.... 最后会耗费很长时间. 这个对很多产品本身就是很致命的. 不推荐Saltstack的原因也是因为其需要在每台agent逐一去安装client service并测试, 这本身就会耗费一些时间成本。
其他呢? 其实我觉得就是容易上手, 语法简单, 有现成模板让你去学习, 加之是我们非常喜爱的python语法, why not?

- Jenkins不用我多说, 估计懂行的人都在用它, 开源, 轻量级, 兼容性和扩展性强, 直观的GUI管理这都是它的优势, 配合Ansible我觉得用起来会非常easy going.

- Gitlab 最后提一下Gitlab, 为什么要用Gitlab? 他作为一个代码版本控制系统和部署有什么关系呢? 其实这里就涉及一个我们Ansible playbook管理问题, 试想我们需要维护一个公司庞大的server集群, 我们所有需要部署的机器或者产品会对应我们相对的部署脚本, 我们使用的Ansible playbook如果只是保存在Ansible Server的具体某个目录, 这本身就不便于我们进行编写维护更新(想想每次都跑到远程去编写playbook或者每次在本地编写好后再upload到远程我都会脑补数以万计的草泥马从我眼前呼啸而来).
这里Gitlab就给我们提供一个非常方便以及直观的Playbook management. 我们需要做的其实就是在Gitlab去建立一个对应产品或者server的playbook仓库, 然后我们在本地写好后直接commit到这个仓库, 最后在部署的时候, 去让Jenkins pull这个playbook到其workspace, 并作为一个Job去run这个playbook, 这样是不是很规范, 而且便于管理?
当然Ansible本身企业版Tower也会提供一个类似管理并维护playbook以及监控ansible本身running process的GUI管理系统, 用起来也很不错, 但作为收费版本, 我们在这里就不做过多阐述了.

# 环境
这里我推荐Jenkins和Ansible可以安装到同一个环境作为部署server, 
Gitlab作为版本控制系统可单独部署在另一台server.

总结:
Jenkins首先从Gitlab去抓取我们写好的具体产品的playbook, 并使用virtualenv下的Ansible相关命令, 保证我们在一个clean的环境下使用stable version去批量部署我们的产品到远程client。

# 安装环境
System: CentOS 6.7 x64 (deploy.example.com)
Jenkins: Jenkins ver. 1.650
Ansible: Ansible 2.1.0
Gitlab: GitLab 7.14.3

# 配置
## Jenkins配置
我们创建deploy用户作为jenkins_user, workspace为deploy家目录下的jenkins目录.

```
# adduser deploy
# vi /et
c/sysconfig/jenkins
 ...
JENKINS_HOME="/home/deploy/jenkins"
JENKINS_USER="deploy"
```

浏览器访问Jenkins页面
http://deploy.example.com:8080
安装完成。

这里我们使用一个国内PHP网站模板phpcms作为我们需要部署的产品进行本次范例演示, 在进行最终的Build前我们需要做一些准备工作, 稍后我们会回到这个界面。

## Ansible配置
配置[virtualenv](http://www.showerlee.com/archives/1862)
[Ansible-playbook范例]( http://www.showerlee.com/archives/1649)

## Gitlab配置

我们最终会创建一个ansible playbook仓库git@git.example.cn:showerlee/Ansible-showerlee.git, 并在本地编写好我们的规则, 最终commit到这个仓库, 以便Jenkins去调用我们的部署规则。

[部署phpcms的playbook仓库实例](https://git.showerlee.com/showerlee/leon-playbook-phpcms1.1)