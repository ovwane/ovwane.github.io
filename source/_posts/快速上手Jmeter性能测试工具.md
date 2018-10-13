---
title: 快速上手Jmeter性能测试工具
date: 2018-10-09 07:56:40
tags:
---

[快速上手Jmeter性能测试工具](http://www.dataguru.cn/article-6808-1.html)

**课程大纲：** 

**第1课：开源的力量--Jmeter **

 1. 阐述开源软件的现状和趋势 

 2. 解析引入和使用Jmeter的原因

 3. 对比多种工具，阐述性能测试工具选型原则

 4. 阐述Jmeter的优势和劣势 

 5. Jmeter的安装和目录解析  

**第2课：初识Jmeter**

1. Jmeter界面结构解析 
2. Jmeter重要的配置文件解析 
3. 实例演示如何使用Jmeter录制页面操作 
4. 代理和反向代理解析 
5. Jmeter与LoadRunner总体对比 

**第3课：搭建骨架--Jmeter重要组件介绍** 

1. Jmeter中的属性和变量 
2. Jmeter中的采样器 
3. Jmeter中的计时器 
4. Jmeter中的前置处理器和后置处理器 
5. 通过实例演示Jmeter组件作用域 
6. Jmeter与LoadRunner骨架对比  

**第4课：往骨架上添肉—Jmeter脚本组成和组件搭配**

1. 阐述在实际使用中Jmeter脚本组成和开发原则 
2. 阐述配置元素和Default元素的区别 
3. 通过实例演示HTTP Cookie Manager的使用  

**第5课：Jmeter中的逻辑控制器（Logic Controller）**

1. Jmeter中有多种逻辑控制器，不仅种类繁多，而且功能丰富，本节课将专门对逻辑控制器进行阐述。 
2. 实例演示循环逻辑控制器
3. 实例演示条件逻辑控制器 
4. 实例演示随机逻辑控制器
5. Jmeter与LoadRunner中该功能的对比

**第6课：采样器（Sampler）详细解析**

1. 采样器作业和配置详细解析 
2. 对重要的采样器的例如HTTP采样器进行详细剖析 

**第7课：Jmeter中的参数化 **

1. 参数化的意义和作用 
2. 通过实例演示Jmeter中的参数化 
3. Jmeter中的参数化与LoadRunner中的参数化的对比 

**第8课：Jmeter中的关联**

1. 关联的意义和作用 
2. 通过实例演示Jmeter中的关联 
3. Jmeter中的关联与LoadRunner中的关联的对比  

**第9课：Jmeter中正则表达式和函数**

1. 基本的正则表达式
2. Jmeter中如何使用正则表达式 
3. Jmeter中函数的概念 
4. Jmeter中函数的应用  

**第10课：Jmeter中的Listener** 

1. 阐述性能测试工具中Listener的意义 
2. Jmeter中常用Listener图表的含义 
3. 如何高效的使用Listener 
4. Jmeter中的Listener与LoadRunner中的Analysis的区别  

**第11课：Jmeter扩展插件的使用** 

1. 阐述可扩展的含义 
2. Jmeter中常用的扩展插件 
3. 通过实例查看扩展插件的使用效果  

**第12课：分布式Jmeter** 

1. 分布式负载生成器的概念和意义 
2. 如何在Jmeter中配置server –client模式 
3. Jmeter与LoadRunner在分布式上的区别和各自特色



## 第1课：开源的力量--Jmeter

### 学习JMeter课程有什么用？

1）多掌握一门性能测试工具，提高职场竞争力。

2）如果仅仅学习工具，那确实有点亏，我们一旦接触到相关知识点，会扩展。以便提升整个计算机体系的理解。听过软件性能测试课程的人都知道，我们的课程特点就是扩充和引导。

3）提升英文水平。



### 性能测试工具选项的原则

1）成本

​	a、工具成本

​	b、学习成本

2）通信协议

​	a、标准协议

​	b、自有协议 （性能测试的原理就是模拟协议）

3）生命力

4）跨平台



### 安装JMeter

[Download Apache JMeter](https://jmeter.apache.org/download_jmeter.cgi)

[apache/jmeter: Mirror of Apache JMeter](https://github.com/apache/jmeter)



JDK6+，作者建议用JDK7

JMeter 2.13 https://archive.apache.org/dist/jmeter/binaries/apache-jmeter-2.13.tgz



解压缩

```shell
tar -xf apache-jmeter-2.13.tgz
```

运行JMeter

```shell
apache-jmeter-2.13/bin/jmeter
```

jmeter配置文件 apache-jmeter-2.13/bin/jmeter.properties

```properties
language=en
```



### JMeter模拟压力的原理

1）性能测试工具-协议（性能测试工具通过什么模拟压力？）

2）自动化测试工具-对象识别技术

3）面试中的一个例子



### JMeter目录结构

1）目录结构解析

docs：

lib：

bin：       

extras：

2）配置文件解析



### 需要注意的是：

强大的工具只是表象，高于工具的，是我们坚实的基础知识和分析问题的能力。

不能做工具的奴隶，我们才是主人。



工具共享：

链接： http://pan.baidu.com/s/1i3yyRel 密码：u55k



## 第2课：初识Jmeter

1）第一印象

2）TestPlan（测试计划）

3）线程组

4）保存



String开发的GUI界面



所有内容都在 Test Plan 下面添加

用户定义变量：User Defined Variables

自定义jar包，放在 lib目录下



Test Plan（测试计划）

​	Thread Group（线程组）



添加线程组，可以添加多个线程组一个测试计划下。

线程组

Number of Threads（并发线程数）

发送请求



5）添加一个Http Sampler 采样器

Sampler -> HTTP Request



**至少到目前为止，我们实现了一个最简单的JMeter的测试计划，虽然还非常不完善，但可以跑起来。**



Implementation: HttpClient4



查看测试结果，需要添加一个Listener，View Results Tree类型。



测试计划保存为.jmx文件



JMeter更新的时候要注意jmx的影响，因为jmx文件没有校监功能。容易出错。

jmx文件是xml文件格式的。



### 使用过程中要注意的

1）查看jmeter日志。bin/jmeter.log

2）log_level.jmeter属性。bin/jmeter.properties

3）Java虚拟机启动配置。HEAP=-Xms512m -Xmx512m

启动JMeter。`java -jar ApacheJMeter.jar`



### JMeter VS LoadRunner

1、界面上

2、安装上

3、协议支持上（Sampler）

4、函数库

5、成本（学习成本，价格成本）

6、开源



### JMeter中的录制-代理讲解

1、录制的原理（Browser->JMeter->Web服务器）

2、代理

3、代理和反向代理



### JMeter中的录制

1、JMeter中的WorkBench（工作台）

2、JMeter录制详解

在JMeter里添加一个录制

WorkBench->Add->Non-Test Elements->HTTP(s) Test Script Recorder

Target Controller：选择测试计划里的线程组



录制HTTPS网站需要安装证书



## 第3课：搭建骨架--Jmeter重要组件介绍

### Jmeter组件（元素）

1）**Jmeter中的Sampler**

2）Jmeter中的计时器

3）Jmeter中的前置处理器和后置处理器

4）Jmeter中的断言

5）**Jmeter中的Controller**

6）Jmeter中的Listener

7）配置原件（配置元素）(服务器地址，变量之类的配置文件)



前置处理器 Pre Processors, Sampler 采样器之前处理

后置处理器 Post Processors，Sampler 采样器之后处理



Logic Controller 控制脚本的结构的。



### 认识了组件，顺序呢？

**本节课的重点不是认识组件，而是组件执行的顺序和组件的作用域。 **

同一级别下Linster是在Sampler之后运行的。（得到的结论）

同一级别下Conf Element在Sampler之前生效。



#### 作用域

作用域问题，同一个级别下作用域相同。

**如果听不明白，请不要听下去。回去再听一遍。**

如果有相同元素，不同级别，不同内容，则本元素同级引用，本元素生效。

参数名字相同，会合并参数。

同一级别，两个元素，按先后顺序生效一个。



Timer 每个元素分别指定时间。并在元素执行之前。

one 5s

two 5s



### Jmeter组件执行顺序

测试计划的元素执行是有序的，通过一下方式执行：

1-配置节点（Conf Element)

2-前置处理器

3-定时器

4-取样器

5-后置处理器（只在有结果可用情况下执行）

6-断言（只在有结果可用情况下执行）

7-监听器（只在有结果可用情况下执行）



### JMeter与LoadRunner骨架对比

1、Jmeter中作用域非常关键

2、Jmeter中需要使用人员介入的部分更多

3、开发一个Jmeter性能测试脚本，实际上，就是根据场景需求，按照一定的作用域拼装组件。

4、LoadRunner中是通过代码的位置和迭代的设置来控制执行的顺序的。



## 第4课：往骨架上添肉—Jmeter脚本组成和组件搭配

### Jmeter脚本开发原则

**简单、正确、高效。**

**简单：**去除无关的组件，同时能复用的尽量复用。

**正确：** 对脚本或者业务正确性进行必要的判断， 不能少也不能多。

**高效：**部分组件仅仅使用在脚本开发模式使用，在真正生产环境下不要使用。Lisener要越少越好。



HttpWatch抓包



Http Request->Embedded Resources from HTML File->对钩 Retrieve All Embedded Resources



### HTTP Cookie Manager

1、HTTP Cookie Manager的应用场景

2、Manager组件和Default组件的区别

Manager不可以有多个，Default可以有多个，然后叠加参数。

HTTP Cookie Manager不能有两个，在同一个作用域下。



3、阅读官方文档



官方文档

[Apache JMeter - User's Manual: Component Reference](https://jmeter.apache.org/usermanual/component_reference.html)



### Jmeter中的属性

1、WorkBench中的属性查看组件

WorkBench->Non-Test Elements->Property Display





Option->Function Helper Dialog



获取变量值

${__P(LoadRunner,test)}



2、属性（Property）

1）什么是属性？

2）如何使用属性？

3）通过实例演示属性



3、属性的特点

1）JMeter属性在测试脚本的任何地方都是可见的（全局）

2）JMeter属性对于整个测试计划都是可见的（全局），因此可以用于在线程间传递信息。



### Jmeter中的变量

1、变量（Variables）

1）回忆一下变量的应用场景

2）LoadRunner中如何实现的？

Test Plan -> User Defined Variables



2、如何使用变量？

${loadrunner}



专门查看变量的采样器

Debug Sampler



User Defined Variables 无论放在哪里，都会被所有线程共享。



3、实例演示变量



4、变量的特性

1）JMeter变量对于测试线程而言是局部变量。这就以为着JMeter变量在不同测试线程中，既可以是完全相同的，也可以是不同的。

2）如果有某个线程更新了变量，那么仅仅是更新了变量在该线程中复制的值。例如，"正则表达式提取器"（后置处理器）会依据它所在线程的采样结果来更新变量值，该变量值可以供相同的线程后续使用。



5、属性和变量都是大小写敏感的。



## 第5课：Jmeter中的逻辑控制器（Logic Controller）

**上次回顾：**属性和参数



**想象一些场景**

1、连续发10个相同的请求

2、其中前两个做特殊处理

**思考**

1、这样做真的好吗？

２、如果脚本非常庞大呢？

３、如果需要做选择操作呢？



### Logic Controller出场

1、首先必须声明的是：Jmeter中的Controller和LoadRunner中的Controller的区别

2、



**Simple Controller**

提供一个块的结构和控制。更方便、更清晰，嵌套其他的Controller



${__threadNum}



**Loop Controller**

1、简单的说就是提供一个循环

 

**Once Only Controller**

1、简介

Thread Group->Once Only Controller只执行一次，线程组多次，也只执行一次。

Thread Group->Loop Controller->Once Only Controller 如果在Loop Controller下，则会执行线程组的次数。

2、例子

3、适用场景



**ForEach Controller**

和User Defined Variables 一起使用



**Transaction Controller**

统计响应时间Load time

Generate parent sample



**If Controller**

解析变量



学什么东西要学以致用。



## 第6课：采样器（Sampler）详细解析

### 采样器

1、那个默默干活的家伙（Sampler）

2、就像每个Controller都有自己的特点一样，每个采样器也都有自己的个性。



### 个性？

1、如何理解个性？

2、学习采样器主要注意哪些地方？

3、有什么方法和技巧？



课程到现在为止，你最熟悉那个采样器？

HTTP Sampler



### 常规设置

1、采样器默认实现-查看jmeter.httpsampler

jmeter.properties

jmeter.httpsampler=HttpClient4



2、文件的上传和下载？

上传：HTTP Sampler -> Send Files With the Request

下载：HTTP Sampler->Save Responses to file



WorkBench->HTTP Mirror Server



3、默认解析器是：htmlparser

通过查看htmlparser.classname



4、通过设置Retrieve All Embedded Resources from HTML Files 和 Use concurrent pool更真实的模拟负载。

HTTP Sampler->Use concurrent pool

HTTP Sampler->Retrieve All Embedded Resources from HTML Files 



5、URLs must match



6、IP欺骗

HTTP Sample -> Source address 



7、file协议

Http Sampler ->protocol->file,除了http还可以用file协议



## 第7课：Jmeter中的参数化 

### 概念的引入

参数化概念解释



### 现实考虑

1、被业务场景所迫：所有用户都输入相同的数据，不能体现出真实的业务环境。（搜索操作）

2、被系统体系所迫：存在缓存，不能体现出真正的性能。

3、被系统业务约束所迫：有些系统禁止同一用户多次登录系统，也就严重到无法测试的地步。



### 哪些需要参数化？

1、登录认证信息

2、一些和时间相关的，违法时间约束的

3、一些受其他字段约束的

4、一些来自于其他数据源（例如数据库的）

5、其他在系统运行过程中需要变动的



### 参数化

参数化后，写死了的代码变活，代码将更加美好！



### 工具中的参数化

1、LoadRunner中的参数化

2、Jmeter中的参数化（本节课的重点）

无论哪种形式或工具的参数化，本质是没有变的。变的只是不同工具提供的不同操作方法。



### Jmeter中的参数化

参数化-我们来了？你在哪？？

变量-变量在哪？

变量是每个线程都有各自一份的，一个线程修改并不影响另一个线程。



Jmeter参数化可以通过多种方式，本节课只讲应用最广泛，最实用的。

通过CSV Data Set Config组件

1、更容易使用和理解

2、适合于大参数量场景

3、设置方便灵活



**1**

conf.csv

```csv
user1,passwd1
user2,passwd2
```



Conf Element->CSV Data Set Config

Filename: conf.csv

Variable Names：username,password



**2**

conf.csv

```csv
username,password
user1,passwd1
user2,passwd2
"us,er3",passwd3 #需要使用 Allow quoted data：True
```



Conf Element->CSV Data Set Config

Filename: conf.csv

Variable Names：



如果recycle option和stopThread都是false，那么当到达文件尾部的时候，变量的数值被设置成EOF,当然，这个可以通过修改属性文件中的csvdataset.eofstring进行修改。 jmeter.properties中修改



## 第8课：Jmeter中的关联

### 什么是关联

1）关联的定义

2）参数化和关联的区别的阐述

关联，从请求的响应中提取数据。参数化，自己做的数据。

3）什么时候需要关联？

服务器返回的动态变化且对业务有影响的。



### 工具中的关联

1、LoadRunner中的关联

2、Jmeter中的关联

**无论哪种形式或工具的关联，本质是没有变的。变的只是不同工具提供的不同操作方法。**



### Jmeter中的关联

关联 -我们来了，你在哪？？

如何获取请求响应中特定的数据或信息？

强大的后置处理器 Post Processors-> Regular Expression Extractor



### 正则表达式

正则表达式，又称正规表示法、常规表示法（英语：Regular Expression，在代码中简称regex、regexp或RE），计算机科学中的一个概念。



**Jmeter的正则规则基于Apache ORO，跟perl语言的正则表达式规则类似，目前学会了解和使用最常用的语法即可。**



具有特殊含义的字符：

（和）：界定期望获取字符串的匹配模式

.(字符点)：匹配任意单个字符

+：一次或者多次

？：找到匹配的结果后立刻停止查找

\：转义字符

[]：匹配符合[]内的字符

[0-9]:匹配所有数字字符

[a-z] 匹配所有小写字母字符

[^0-9]匹配所有非数字字符

[^a-z]匹配所有非小写字母字符

^匹配字符开头的字符

$匹配字符结尾的字符



### 总结

通过例子可以看出，在性能测试工具中进行关联是构建正确的脚本基本要求。而高于工具和关联的，是需要对协议的了解和业务的属性。



抓包查看分析也是一项基本技能。