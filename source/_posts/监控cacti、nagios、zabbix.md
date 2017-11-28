title: 监控cacti、nagios、zabbix
date: 2016-12-02 18:28:23
categories:
- 技术
- Linux
tags:
- cacti
- nagios
- zabbix
---
监控cacti、nagios、zabbix

- cacti 重图形，有数据历史，需用到数据库支持，支持web配置，默认不支持告警，可以加插件； 
- nagios重状态和结果，没有数据历史，不成图像，不支持web配置，可以自己开发脚本定制个性化监控，支持多种插件； 
- zabbix有数据历史，可成图像，支持web配置，可以自动发现； 

# Zabbix
## Zabbix简介
　　Zabbix是一个企业级的开源分布式监控解决方案，由一个国外的团队持续维护更新，软件可以自由下载使用，运作团队靠提供收费的技术支持赢利。官方网站：http://www.zabbix.com官方文档：http://www.zabbix.com/documentation/2.0/manual/quickstart。Zabbix通过C/S模式采集数据，通过B/S模式在web端展示和配置。

## Zabbix运行条件：
- Server：Zabbix Server需运行在LAMP（Linux+Apache+Mysql+PHP）环境下，对硬件要求低。
- Agent：目前已有的agent基本支持市面常见的OS，包含Linux、HPUX、Solaris、Sun、windows。
- IPMI：Agent的另一种方式，主要应用于设备的物理性能监控，例如设备的温度、风扇的转速等。
- SNMP：支持各类常见的网络设备。

## Zabbix功能
　　具备常见的商业监控软件所具备的功能（主机的性能监控、网络设备性能监控、数据库性能监控、FTP等通用协议监控、多种告警方式、详细的报表图表绘制）支持自动发现网络设备和服务器；支持分布式，能集中展示、管理分布式的监控点；扩展性强，server提供通用接口，可以自己开发完善各类监控。
　　优点：开源，无软件成本投入；Server对设备性能要求低（实际测试环境：虚拟机CentOS5，2GCPU 1G内存，监控5台设备，CPU使用率基本保持在10%以下，内存剩余400M以上）；支持设备多；支持分布式集中管理；开放式接口，扩展性强。

## Zabbix架构
　　Zabbix支持多种网络方式下的监控，可通过分布式的方式部署和安装监控代理。
　　
　　
## 网络设备时间同步配置
　　公司现有网络设备为cisco E4506和cisco c2960。各相关设备的配置如下。 

Cisco E4506配置 
　　Cisco 4506这类设备有硬件时钟，相关设定如下所示。 

```
clock timezone Beijing 8 0  #时区 
clock calendar-valid        #设硬件时钟 
ntp source Vlan19           #时间服务器源 
ntp master 3 
ntp server 192.168.0.189    #时间服务器地址 
show ntp status             #查看状态   
show ntp associations  
```

Cisco 2960配置

```
clock timezone Beijing 8 0  #时区  
ntp source Vlan19           #时间服务器源  
ntp master 3  
ntp server 192.168.0.189    #时间服务器地址  
show ntp status             #查看状态  
show ntp associations  
```

## [网络设备监控](http://waringid.blog.51cto.com/65148/1104627)
Cisco 2960配置 
　　自从升级了网络设备后就一直在想办法把这些网络设备的性能也监控起来，在网上找了很多的资料也下载了一些模板，使过后才发现不是很靠谱，看来只能靠自已了。这些仅描写一些需要注意的地方，如果看不明白的可以参考一下以前的文档。 
　　SNMP协议在cisco设备中是可以支持查看CPU和RAM性能的，这方面的内容cisco的网站上的内容很丰富。需要注意的是同一型号的设备如果IOS版本不一样它的OID值和相关的设置指令是不一样的。请参考[How to Collect CPU Utilization on Cisco IOS Devices Using SNMP](https://www.cisco.com/c/en/us/support/docs/ip/simple-network-management-protocol-snmp/15215-collect-cpu-util-snmp.html) 
　　 
首先在需要监控的设备上启用snmp协议。 
　　
```
snmp-server community public RO   
snmp-server enable traps  
```

# Cacti与Nagios进行网络监控的区别
Cacti和Nagios是现在使用比较多的网络监控软件了，对于这两款监控软件的区别，应该说是侧重点的不同。

- Cacti比较着重于直观数据的监控，易于生成图形，用来监控网络流量、cpu使用率、硬盘使用率等可以说很在合适不过。
- Nagios则比较注重于主机和服务的监控，并且有很强大的发送报警信息的功能。

把两者结合起来，既可以使报警机制高效及时，又可以很容易的查看各项数据的情况。
由于工作的关系，我在前一家公司主要是用FreeBSD来架构网络监控程序，最早使用的是MRTG，然后开始用RRDTOOL，后来发现了Cacti，爱不释手啊。
而现在的公司，一开始是老板要求用Nagios来进行主机和服务监控，但是后来觉得Nagios设置起来实在不方便，所以改用了Cacti，并且使用Plugin来构建报警机制，但是效果不甚理想。
于是就在找一个比较合适的解决办法，前一段在网上看到Nagios For Cacti的Plugin终于有了更新，决定试一下看看。
1. 安装必须的软件
2. 安装Cacti
3. 安装Cacti Plugins Arch
4.安装NPC，Settings和Thold
5. 安装Nagios
6. 安装NDoutils
如果，你管理的系统是一个30台服务器规模以下的小公司，那么也许你自己写的监控脚本是最好的解决办法，但是，如果，服务器达到30台以上的，而且分布到各个地域，那么使用一些开源的监控工具就非常合适了。
这里只说自己用过的两种监控工具，这两种工具可以配合使用，一个是cacti,另一个是nagios。
这两个工具最好是都装在linux系统上，cacti需要通过snmp协议收集被监控服务器的信息，nagios 则有自己的agent去收集信息。cacti虽然可以安装在windows上，其实那也是模拟了一个linux的类环境。

cacti偏重于网络流量，系统负载方面的监控。而 nagios偏重于系统服务方面的监控，你可以在被监控的机器上写自己的程序(shell,c 或 perl都可以) 。nagios则通过这些脚本来对服务进行监控。nagios可以和短信发送机配合用来监控规模较大的网站。

cacti 是一个用 rrdtool 来画图的网络监控系统, 通常一说到网络管理, 大家首先想到的经常是 mrtg, 但是 mrtg 画的图简单且难看, rrdtool 虽然画图本领一流, 画出来的图也漂亮, 但是他也就是一个画图工具, 不像 mrtg 那样本身还集成了数据收集功能. cacti 则是集成了各种数据收集功能,然后用 rrdtool 画出监控图形. 其本身界面比起同类系统要漂亮不少. 推荐所有有监控需求的人都去研究一下.

cacti 和 nagios 是不同功用的系统, nagios 适合监视大量服务器上面的大批服务是否正常, 重点并不在图形化的监控, 其集成的很多功能例如报警,都是 cacti 没有或者很弱的. cacti 主要用途还是用来收集历史数据和画图, 所以界面比 nagios 漂亮很多.

net-snmp 是一套广泛使用在类 unix 系统上的 snmp 软件, 包含一套 snmp agent 框架 ,一个 snmpd 和 一堆 snmp 工具 , 其前身为 ucd-snmp. 关于 snmp 是什么, 以及如何配置的文章,网上搜一下有一堆一堆的. 在这里就不重复了.

squid 是一个 web 缓存加速程序, 本来跟监控没有太大关系, 只是因为他支持 snmp 查询,而我要用 cacti 监控他, 然后遇到了他的缺陷被折腾了一阵子,所以也拉进今天的讨论.
我跟这三个东西斗争的过程如下…

首先先把 cacti 架起来, 在架的过程中我没有遇到问题,但是把 czz 搞了一下, 因为 cacti 要调用外部程序, 不能开 safe_mode, 如果开了就会出奇怪问题.
接下来配置 squid 的查询, squid 的查询数据比较多且复杂,自己做模版的话很麻烦,于是 google 了一下,找了一个 SquidStats (见附件) 的模版, 按照他的 readme 一步一步来, 就可以正常安装. 于是我就遇到了第一个坎…
设置完成以后执行 poller 的时候总是无法产生 rrd 数据, 给 php 里面加 log 也没有看出来什么, google 换了很多关键词, 总算发现了原因: cacti 在进行 snmp 查询之前会先确定对方是否在运行, 他用的方法是查询 .1.3.6.1.2.1.1.3.0 这个 oid, 但是 squid 不支持这个 oid , 于是 cacti 就以为 squid down 了,不去真正查询. 临时解决方法是在 cacti 的 settings 里面, poller 页的 Downed Host Detection 选择 Ping, 不要选择带有 snmp 字样的.
然后在弄 64 位机的时候遇到了第二个坎, 发现 64 位 linux 机器的流量图总是不正确. 大部分时候没有结果,有时候又特大, 其实这个应该很容易想到是 counter 回绕不正确的问题, 但是我第一次 google 出来的结果是一个说和 tunnel 设备有关的 bug, 这两台 x64 机器上面确实有 tunnel ,于是我就一直以为是 tunnel 的问题. 折腾了好久. 最后才发现原来就是简单的 counter 回绕不正确的问题. 从 Fedora Core 5 的开发目录里面下一个 net-snmp 5.3 的 srpm 在 centos 4.2 上 build 一下, 就搞定了. 注意 FC4 里面的 net-snmp 5.2.x 也是有 bug 的,一定要 5.3 的.
最后推荐所有研究 cacti 的人,一定不要放过 cacti 的官方论坛扩展脚本版面 . 里面有很多的第三方的模版和脚本, 支持很多的网络设备.
http://tewuxiaoqiang.blog.51cto.com/279711/161207

# Cacti Nagios比较

http://www.oschina.net/p/zabbix 

zabbix是一个基于WEB界面的提供分布式系统监视以及网络监视功能的企业级的开源解决方案。zabbix能监视各种网络参数，保证服务器系统的安全运营；并提供柔软的通知机制以让系统管理员快速定位/解决存在的各种问题。
zabbix由2部分构成，zabbix server与可选组件zabbix agent。
zabbix server可以通过SNMP，zabbix agent，ping，端口监视等方法提供对远程服务器/网络状态的监视，数据收集等功能，它可以运行在Linux, Solaris, HP-UX, AIX, Free BSD, Open BSD, OS X等平台之上。
zabbix agent需要安装在被监视的目标服务器上，它主要完成对硬件信息或与操作系统有关的内存，CPU等信息的收集。zabbix agent可以运行在Linux ,Solaris, HP-UX, AIX, Free BSD, Open BSD, OS X, Tru64/OSF1, Windows NT4.0, Windows 2000/2003/XP/Vista)等系统之上。
zabbix server可以单独监视远程服务器的服务状态；同时也可以与zabbix agent配合，可以轮询zabbix agent主动接收监视数据（trapping方式），同时还可被动接收zabbix agent发送的数据（trapping方式）。
另外zabbix server还支持SNMP (v1,v2)，可以与SNMP软件(例如：net-snmp)等配合使用。
zabbix的主要特点：
- 安装与配置简单，学习成本低
- 支持多语言（包括中文）
- 免费开源
- 自动发现服务器与网络设备
- 分布式监视以及WEB集中管理功能
- 可以无agent监视
- 用户安全认证和柔软的授权方式
- 通过WEB界面设置或查看监视结果
- email等通知功能
等等
Zabbix主要功能：
- CPU负荷
- 内存使用
- 磁盘使用
- 网络状况
- 端口监视
- 日志监视
zabbix的License：GPL v2
标签： Linux PHP C/C++ 系统监控
开发语言： PHP C/C++
项目主页： http://www.zabbix.com/
文档地址： http://www.zabbix.com/documentation.php
下载地址： http://www.zabbix.com/download.php
收录时间：2008年09月16日

首先简单介绍一下: cacti 是一个用 rrdtool 来画图的网络监控系统, 通常一说到网络管理, 大家首先想到的经常是 mrtg, 但是 mrtg 画的图简单且难看, rrdtool 虽然画图本领一流, 画出来的图也漂亮, 但是他也就是一个画图工具, 不像 mrtg 那样本身还集成了数据收集功能. cacti 则是集成了各种数据收集功能,然后用 rrdtool 画出监控图形. 其本身界面比起同类系统要漂亮不少. 推荐所有有监控需求的人都去研究一下.

cacti 和 nagios 是不同功用的系统, nagios 适合监视大量服务器上面的大批服务是否正常, 重点并不在图形化的监控, 其集成的很多功能例如报警,都是 cacti 没有或者很弱的. cacti 主要用途还是用来收集历史数据和画图, 所以界面比 nagios 漂亮很多
1. 主要对流量及主机在线状态监控软件,如最初的MRTG,PRGT,CACTI,Hobbit,
2. 能对服务器的关键服务及进程进行监控的软件,如Big Brother,Nagios,



# Cacti介绍
    Cacti是一个用 rrdtool 来画图的网络监控系统，通常一说到网络管理，大家首先想到的经常是 mrtg，但是 mrtg 画的图简单且难看，rrdtool 虽然画图本领一流，画出来的图也漂亮, 但是他也就是一个画图工具，不像 mrtg 那样本身还集成了数据收集功能。cacti 则是集成了各种数据收集功能,然后用 rrdtool 画出监控图形。其本身界面比起同类系统要漂亮不少. 推荐所有有监控需求的人都去研究一下。Cacti是一套基于PHP，MySQL，SNMP及RRDTool开发的网络流量监测图形分析工具。它通过 snmpget来获取数据，使用RRDtool绘画图形
    Cacti三层架构：数据展现层、数据存储层、数据采集层,其具体如下：
        数据采集层：通过SNMP或自定义脚本进行数据采集
        数据存储层：通过cacti模板等数据存放至MYSQL中
        数据展现层：通过WEB方式呈现出来
Cacti应用场景
1）网络设备
（1）接口流量（进与出的带宽）
（2）监控CPU的负载、内存等等
（3）温度等等
2）主机系统
（1）网络接口流量（进与出的带宽）
（2）监控CPU的负载、内存等等
（3）监控磁盘的空间、进程数等等
3）cacti常见的监测对象
（1）服务器资源：CPU、内存、磁盘、进程、连接数等
（2）服务器类型：WEB、Mail、FTP、数据库、中间件
（3）网络接口：流量、转发速度、丢包率
（4）网络设备性能、配置文件（对比与备份）、路由数
（5）安全设备性能、连接数、攻击数
（6）设备运行状态：风扇、电源、温度
（7）机房运行环境：电流、电压、温湿度

# nagios介绍
    cacti 和 nagios 是不同功用的系统, nagios 适合监视大量服务器上面的大批服务是否正常, 重点并不在图形化的监控, 其集成的很多功能例如报警,都是 cacti 没有或者很弱的. cacti 主要用途还是用来收集历史数据和画图, 所以界面比 nagios 漂亮很多.
    Nagios通常由一个主程序(Nagios)、一个插件程序(Nagios-plugins)和四个可选的附件(NRPE、NSCA、 NSClient++和NDOUtils)组成。Nagios的监控工作都是通过插件实现的，因此，Nagios和Nagios-plugins是服务器端工作所必须的组件。
    其它四个附件：
   （1）NRPE：用来在监控的远程Linux/Unix主机上执行脚本插件以实现对这些主机资源的监控
   （2）NSCA：用来让 被监控的远程Linux/Unix主机主动将监控信息发送给Nagios服务器(这在冗余监控模式中特别要用到)
   （3）NSClient++：用来监控 Windows主机时安装在Windows主机上的组件
   （4）NDOUtils：则用来将Nagios的配置信息和各event产生的数据存入数据库，以实现 这些数据的快速检索和处理
    这四个ADDON(附件)中，NRPE和NSClient++工作于客户端，NDOUtils工作于服务器端，而NSCA则需要同时安装在服务器端和客户端
nagios主要功能
网络服务监控（SMTP、POP3、HTTP、NNTP、ICMP、SNMP、FTP、SSH）
主机资源监控（CPU load、disk usage、system logs），也包括Windows主机（使用NSClient++ plugin）
可以指定自己编写的Plugin通过网络收集数据来监控任何情况（温度、警告……）
可以通过配置Nagios远程执行插件远程执行脚本
远程监控支持SSH或SSL加通道方式进行监控
简单的plugin设计允许用户很容易的开发自己需要的检查服务,支持很多开发语言（shell scripts、C++、Perl、ruby、Python、PHP、C#等）
包含很多图形化数据Plugins（Nagiosgraph、Nagiosgrapher、PNP4Nagios等）
可并行服务检查
能够定义网络主机的层次, 允许逐级检查, 就是从父主机开始向下检查
当服务或主机出现问题时发出通告，可通过email, pager, sms 或任意用户自定义的plugin进行通知
能够自定义事件处理机制重新激活出问题的服务或主机
自动日志循环
支持冗余监控
包括Web界面可以查看当前网络状态，通知，问题历史，日志文件等
3、结合实际应用选型软件
分析：
1）、 NRPE与SNMP协议
Cacti在LINUX下主要采用SNMP协议；snmp是简单网络管理协议，通过固定协议运行方式以OID格式提供系统运行状态的全面信息，然后通过snmp agent去获取这些信息并绘制流量。
NAGIOS在LINUX下主要采用NRPE插件，NRPE通过ssl方式在C/S结构下调用被监控主机的状态监测脚本，并将获得的信息实时提供到监控服务器。
2)、NAGIOS与CACTI区别
Cacti：在监控方面绘图比较不错，在流量与图型展现比较存在优势
Nagios：在故障分析比较不错，报警机制相对来说比较好，报警机制：邮箱、短信等，而且也比Cacti灵活；同时适用监控大量服务器以及服务器上面大批服务状态是否正常，重点不在图形化，而在状态故障的监控
综合所知：
cacti偏沉于收集流量画图，系统负载方面的。而nagios偏沉于系统状态正常与否方面的， nagios能够和短信发送机共同用来规模较大的网络，Cacti+Nagios 两者结合使用取长补短方为上上之策。

#  zabbix介绍
zabbix是一个基于WEB界面的提供分布式系统监视以及网络监视功能的企业级的开源解决方案。zabbix能监视各种网络参数，保证服务器系统的安全运营；并提供柔软的通知机制以让系统管理员快速定位/解决存在的各种问题。zabbix由2部分构成，zabbix server与可选组件zabbix agent。zabbix server可以通过SNMP，zabbix agent，ping，端口监视等方法提供对远程服务器/网络状态的监视，数据收集等功能，它可以运行在Linux, Solaris, HP-UX, AIX, Free BSD, Open BSD, OS X等平台上。


# zabbix与nagios对比
web功能：
   Nagios简单直观，报警与数据都在同一页面，***、红色即为问题项。Nagios web端不要做任何配置。
   Zabbix监控数据与报警是分开的，查看问题项需要看触发器，查看数据在最新数据查看。而且zabbix有很多其它配置项
   结论：对于初学者，nagios更容易上手，但是zabbix界面更美观，同时由于功能多上手也更难。
 
画图展示：
   Nagios需要额外安装插件，且插件画图不够美观。
   Zabbix携带画图功能，且能手动把多个监控项集在一个图中展示，还能选择图形类别，有:折线图、面积图、饼形图、柱形图等供选择。
   结论：画图功能Zabbix用的爽
 
默认监控：
   Nagios自带的监控项很少。对一些变动的如多个分区、多个网卡进行监控时需要手动配置。
   Zabbix自带了很多监控内容，感觉zabbix一开始就为你做了很多事，特别是对多个分区、多个网卡等自动发现并进行监控时，那一瞬间很惊喜，很省心的感觉。
   结论：zabbix感觉爽很多
 
自定义监控服务：
   Zabbix与Nagios都是自写插件，然后修改client端的配置文件。
   结论：两者难易程度一样
 
批量监控主机：
   Nagios对于批量监控主机，需要用脚本在server端新增host，并拷贝service文件。
   Zabbix在server端配置自动注册规则，配置好规则后，后续新增client端不需要对server端进行操作。
   结论：zabbix的后续批量监控实施更简单
 
后期批量修改监控服务：
   Nagios用脚本来修改所有主机的services文件，加入新增服务。
   Zabbix只需手动在模板中新增一监控项即可。
   结论：一个需要构思脚本的实现，一个鼠标点几下即可，zabbix用的要爽一些。
 
报警实现：
   Nagios报警使用插件方式，只要插件能做到的报警，nagios都能实现，无论手机邮箱以及其它。
   Zabbix同Nagios
   结论：两者一致
 
其它扩展
   Zabbix自带web监控，自带对进程及端口监控等，当然还有一些其它的功能我还未探索到。
   Nagios也有插件，没有的可自己写插件。
 
   Zabbix提供API接口，方便其它平台调用。但Nagios可以由程序直接配置管理。
   结论：一个把时间花在摸索上，一个把时间花在写脚本上，说不上谁好，就差不多吧。
 
总结：
   Nagios要花很多时间写插件，Zabbix要花很多时间探索功能。
   Nagios更易上手，Nagios两天弄会，Zabbix两周弄会。
   Zabbix画图功能比Nagios更强大
   Zabbix对于批量监控与服务更改，操作更简洁；Nagios如果写好自动化脚本后，也很简单，问题在于写自动化脚本很费神。
 
对于企业的监控应用来说，两者都能实现大规模监控，都足以满足用户需求，没有绝对的孰好孰坏。  Zabbix是商业软件开源、all in one方式体验良好，Nagios是免费软件，插件组合多。
两者就像windows与linux一样，一个把所有的都已做好，一个可以定制所有。