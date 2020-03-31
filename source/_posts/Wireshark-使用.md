---
title: Wireshark 使用
date: 2019-03-03 08:36:03
tags:
- 网络
---

# Wireshark

> Wireshark网络分析就这么简单-书籍



## 抓包

### 只抓必要的包：

**第一种**：打开 Wireshark，在主界面上->Capture->using this filter（输入 `host www.baidu.com`）



**第二种**：

Capture->Options, Capture Filter（新建 Filter，使用IP 或者域名）

[CaptureFilters - The Wireshark Wiki](https://wiki.wireshark.org/CaptureFilters)



tcpdump

```shell
tcpdump -i eth0 host www.baidu.com -w ~/tmp/tcpdump.cap
```



### 每步操作打上标记：

macOS

```shell
# -c 次数，-s 包大小
ping www.baidu.com -c 1 -s 1
ping www.baidu.com -c 1 -s 2
ping www.baidu.com -c 1 -s 3
```

Windows

```shell
# -n 次数，-l 包大小
ping www.baidu.com -n 1 -l 1
ping www.baidu.com -n 1 -l 2
ping www.baidu.com -n 1 -l 3
```



## 个性化设置

### 日志时间格式

View->Time Display Format->Date and Time of Day（快捷键：command+option+1）



### 不同类型的网络包可以自定义颜色

View->Coloring Rules

> **颜色配置**可以导入/导出方便其他主机上的 Wireshark 使用。



### 更多的设置细节

> 例如：配置 TCP 协议相关的细节。

Preferences（快捷键：command+,）->Protocols->TCP



### 抓包服务器时区不同

如果你在其他时区的服务器上抓包，然后下载到自己的电脑上分析，最好把自己电脑的时区设成跟抓包的服务器一样。这样，Wireshark显示的时间才能匹配服务器上日志的时间。否则就得先换算时间。



## 过滤

> 很多时候，解决问题的过程就是层层过滤，直至找到关键包。前面已经介绍过抓包时的Capture Filter功能了。其实在包抓下来之后，还可以进一步过滤，而且这一层的过滤功能更加强大。

[Wireshark · Display Filter Reference: Index](https://www.wireshark.org/docs/dfref/)



### 已知协议

如果已知某个协议发生问题，可以用协议名称过滤一下，那么就在 Filter 框输入 `http` 为关键字过滤。



#### 协议之间依赖关系

用协议过滤时务必考虑到协议间的依赖性。比如NFS共享挂载失败，问题可能发生在挂载时所用的mount协议，也可能发生在mount之前的portmap协议。这种情况下就需要用 `portmap || mount`来过滤了。



### IP 地址加 port 号

> Wireshark是按照什么过滤出一个TCP/UDP Stream的？答案就是：两端的IP加port。单击Wireshark的Statistics-->Conversations，再单击TCP或者UDP标签就可以看到所有的Stream

#### **手工输入**

```
ip.addr==10.8.7.214
ip.addr eq 10.8.7.214

tcp.port==443
tcp.port eq 443
```



#### **自动追踪** （Follow TCP/UDP Stream）

鼠标右键要追踪的包->Follow->TCP Stream（快捷键：option+shift+command+T ）



### 用鼠标帮助过滤

> 我们有时因为Wireshark而苦恼，并不是因为它功能不够，而是强大到难以驾驭。比如在过滤时，有成千上万的条件可供选择，但怎么写才是合乎语法的？

- 右键单击Wireshark上感兴趣的内容，然后选择Prepare a Filter-->Selected，就会在Filter框中自动生成过滤表达式。在有复杂需求的时候，还可以选择And、Or等选项来生成一个组合的过滤表达式。

- 假如右键单击之后选择的不是Prepare a Filter，而是Apply as Filter-->Selected，则该过滤表达式生成之后还会自动执行。图12显示了在一个SMB包的SMB Command: Read AndX上右键单击，并选择Selected之后，所有的Read包都会被过滤出来。



### 过滤后得到的网络包存到一个新的文件

我们可以把过滤后得到的网络包存在一个新的文件里，因为小文件更方便操作。单击Wireshark的 File-->Export Specified Packets，得到的新文件就是过滤后的部分。



## 让Wireshark自动分析

### Export Information

单击Wireshark的Analyze-->Expert Infomation，就可以在不同标签下看到不同级别的提示信息。比如重传的统计、连接的建立和重置统计，等等。在分析网络性能和连接问题时，我们经常需要借助这个功能。



### Service Response Time

单击Statistics-->Service Response Time->再选定协议名称，可以得到响应时间的统计表。我们在衡量服务器性能时经常需要此统计结果。

> 支持 AFP，LDAP，SCSI，SMB，SMB 协议。其他的协议没有见过。



### TCP Stream Graph

单击Statistics-->TCP Stream Graph，可以生成几类统计图。



> 为什么Wireshark要把这个图称为“Stevens”呢？我猜是为了向《TCP/IP Illustrated》的作者Richard Stevens致敬。这也是我非常喜欢的一套书，在此推荐给所有读者。



### I/O Graph

单击Statistics->I/O Graph，可以看到一些统计信息，比如平均流量等，这有助于我们推测负载状况。



## 搜索功能

与很多软件一样，Wireshark也可以通过 “Ctrl+F”（command+F）搜索关键字。假如我们怀疑包里含有“error”一词，就可以按下 “Ctrl+F” 之后选中 `String` 单选按钮，然后在 Filter 中输入 `error` 进行搜索很多应用层的错误都可以靠这个方法锁定问题包。



> 一篇文章不可能涵盖所有技巧，本文就到此为止。最后要分享的，是我认为最“笨”但也是最重要的一个技巧——勤加练习。只要练到这些技巧都变成习惯，就可以算登堂入室了。



> 2011年Wireshark在SecTools排行第一，2012年被Insecure.org评为“No. 1 Packet Sniffers”。美国的技术作家们开始为它著书立说，中国的出版社也在引进（比如人民邮电出版社引进出版的《Wireshark数据包分析实战（第2版）》）。值得一提的是，CACE后来被Riverbed收购了，Riverbed成了Wireshark项目的赞助商。很多中国工程师可能觉得Riverbed名不见经传，但说到Linux里常用的tcpdump命令就不会陌生。tcpdump的开发者之一Steve McCanne就是Riverbed的CTO。而WinPcap的开发者Loris Degioanni也在Riverbed工作。似乎冥冥之中自有天意，Riverbed把网络探测界的先锋们聚到了一起。我们要向Riverbed致敬，多亏了这些伟大的工具，我们才得以窥探网络的秘密。



## 庖丁解牛

   

### NFS 协议的解析

Filter：`portmap || mount || nfs`

>NFS对客户端的访问控制是通过IP地址实现的。创建共享目录时可以指定哪些IP允许读写，哪些IP只允许读，还有哪些IP连挂载都不允许。虽然配置不难，但这方面出的问题往往很“诡异”，没有Wireshark是几乎无法排查的。比如，我碰到过一台客户端的IP明明已经加到允许读写的列表里，结果却只能读。这个问题难住了很多工程师，因为在客户端和服务器上都找不到原因。后来我们在服务器上抓了个包，才知道在收到的包里，客户端的IP已经被NAT设备转换成别的了。 



### HTTP 协议

### HTTPS 协议解密

> 通过浏览器保存的TLS 会话中使用的对称密钥来进行数据解密。

**Wireshark 的抓包原理是直接读取并分析网卡数据，要想让它解密 HTTPS 流量，有两个办法：**

1）如果拥有 HTTPS 网站的加密私钥，可以用来解密这个网站的加密流量；

2）某些浏览器支持将 TLS 会话中使用的对称加密密钥保存在外部文件中，可供 Wireshark 解密之用。

[TLS/SSL抓包常见方法(一)](http://www.rfc-editor.org/info/rfc2616)

[HTTP/2 流量调试](https://www.jianshu.com/p/83551b91e9a2)

[Wireshark解密HTTPS数据流](https://www.cnblogs.com/aucy/p/9082429.html)



macOS

```shell
touch ~/tmp/sslkeylog.log

# 设置环境变量
export SSLKEYLOGFILE=~/tmp/sslkeylog.log

# 查看变量是否生效
echo $SSLKEYLOGFILE
```

Preferences->Protocols->SSL

SSL debug file: `cat ~/tmp/ssl.log`

(Pre)-Master-Secret log filename: `~/tmp/sslkeylog.log`



**启动 Wireshark**: filter`host pan.baidu.com and tcp port 443`

**启动 Chrome**：`open /Applications/Google\ Chrome.app`

```shell
cat ~/tmp/sslkeylog.log
cat ~/tmp/ssl.log
```



> Life is tough, but Wireshark makes it easy.



SACK



## tshark

> macOS 下安装 Wireshark的时候,默认会附带 tshark、capinfos 和 editcap 等工具。

[tshark - The Wireshark Network Analyzer 3.0.0](https://www.wireshark.org/docs/man-pages/tshark.html) 



```shell
tshark -n -r <tcpdump name> -z 'proto, colinfo, frame.time_relative, frame.time_relative' -z 'proto, colinfo, tcp.ack && (tcp.srcport == <source port> && tcp.dstport == <destination _port>),tcp.ack' -z 'proto, colinfo, tcp.window_size && (tcp.srcport == <source_ port> && tcp.dstport == <destination port>), tcp.window_size'|awk -f <script>
```



tshark输出的分析文本大多可以直接写入分析报告中,而 Wireshark生成不了这样的报告。比如说,我想统计每一秒钟里CIFS操作的 Service Response Time,那只要执行以下命令就可以了。

```shell
tshark -n -q -r tcpdump.cap -z "io, stat, 1.00, AVG(smb.time)smb.time"
```



和其他软件一样,命令行往往比图形界面快得多。比如现在有一个很大的包需要用IP192.168.1.134过滤,用 Wireshark操作的话先得打开包,再用ip.adr=192.168.1.134过滤,最后保存结果。这三个步骤都很费时,但是tshark用下面一条命令就可以完成了。

```shell
tshark -r tcpdump log -R "ip.addr==192.168.1.134 -w tcpdump.log.filtered
```



重传状况要用到 `tcp.analysis.retransmission` 命令,包括了超时重传和快速重传两种情况。

```shell
tshark -n -a -r retran.cap -z "io, stat, 0, tcp.analysis.retransmission"
```

> 乱序状况则只要把“ retransmission”改成” out_of_order”



如何统计一个包里的所有对话? `conv,xxx` 就可以做到,其中xxx可以是tcp、udp、eth或者ip。

```shell
tshark -n -q -r retran.cap -z "conv, tcp"
```



如果一个包大得连 tshark都无法打开,有没有办法切分成多个? 有办法,可以使用 editar命令来做到。

```shell
# 每8秒为间隔切分了这个包
editcap <input file> <output file> -i <seconds per file>
editcap retran.cap output.cap -i 8

# 
editcap < input file> <output file> -c <packets per file>
```



> 除了这里介绍的这些, tshark下的网络分析技巧还有很多。利用管道( Pipeline) 还可以结合awk、sed等命令实现更为强大的功能,值得每位工程师长期学习。如果学习过程中遇到任何问题,建议查询 Wireshark的官方说明,地址为 [tshark - The Wireshark Network Analyzer 3.0.0](https://www.wireshark.org/docs/man-pages/tshark.html) 。就算我这样的老用户还经常能从中学到新知识呢。



## 一个技术男的自白

​	当我在台灯下写到这一篇时,不由得想到几个月后,另一束灯光下的读者正翻到这一页,跨越时空的交流真是奇妙。我要感谢你购买本书并坚持读到这里。作为小众图书的作者,我最珍视的是读者对本书内容的喜爱,也希望你在阅读中有所收获。最后一篇,就让我们忘记那些乏味的术语,谈些有趣一点的话题吧。

​	关于技术,当下的热点是 Full Stack Engineer,翻译过来就是全栈工程师。我的理解就是从前端到后端,从软件到硬件都懂的通才。其实在全栈的概念出现之前,关于技术广度和深度的讨论就从来没有停止过。在时间有限的情况下,究竟是应该扩展广度,各种技术都去涉猎,还是把所有精力都投入在一门技术上呢? 我个人更倾向于后者,因为当某项技术学到了较深的程度后,眼界就不一样了, 再学其他的技术也容易达到类似境界。以本书提到的协议为例,如果你已经精通CIFS,那很可能稍加点拨就能完全理解NFS；同样如果你理解了网络的分层和流控,再学习存储的层次和缓存也比较容易。但假如一个人连最擅长的技术都浅尝辄止,那学习其他技术也会停留在表面上。我有位技术出色的朋友用过一个生动的比喻来说明这个问题；技术深度和广度的关系,就像登山时的高度和视野。假如你爬到半山腰就停下来眺望,就只能看到一半的视野;但如果埋头爬到山顶, 抬头便是无边的风景。

​	关于薪水,是很多工程师自怨自艾的口水话题。不知道从何时开始,大家似平都觉得自己被亏待了。微博上流传各种自嘲的段子,比如“今天你编程时流的汗,就是当初填志愿时脑子进的水”;我也曾经开玩笑说自己的英文名是“Low Payman”;我有位年薪40多万的同事,MSN签名是“少壮不努力,老大干I”还有一种流行的说法,认为在中国不适合走技术路线,否则为什么在国外才有白发苍苍的老工程师?看过太多类似段子之后,我觉得这种群体心态已经有点矫情了。无论在什么国家,工程师都排不上收入最高的群体。相比国外,中国工程地位已经算高了,比如美国工程师的收入就完全比不上律师和医生等职业,但在中国就未必是这样。中国也不是没有老工程师的发展空间,而是因为第一批工程师还没有变老。热爱自嘲的人其实也心知肚明一一他们的薪水完全足以维持体面的生活,比如那位“少壮不努力”的同学,一直在上海这个大染缸过着纸醉金迷的日子。而真正徒伤悲的职业,恐怕根本没有心情自我编排……我认为自明是种难得的幽默,但是当一个群体的自嘲都专注在薪水上,听上去就有点无聊。

​	关于办公室政治,那真不是属于我们的战场。孟子的“劳心者治人,劳力者治于人”对中国影响太过深远,我不止一位朋友从技术路线改走管理路线的时候, 以这句话作为座右铭。而在我看来,自从人类进化到可以坐在办公室里“劳力”之后,“劳心”就缺乏吸引力了。人类比电脑狡诈太多,还是管电脑省心。我们就把办公室政治这样劳心的活儿留给走管理路线的同事吧,只要不站队不说是非, 用技术帮助所有人,自然会成为单位里最受尊敬的人。

​	关于创业,我想没有哪个行业比IT界更热衷于此了。或许是因为这一行有过太多轻易成功的故事,所以工程师们蠢蠢欲动,仿佛每个人都在想,连一个毫无技术含量的导航网站都能被高价收购,满腹才华的我能干出怎样惊天动地的事业?于是有志者开始对职业不满,觉得无论如何应该出去闯闯,寻找自己被封印的灵魂,他们振臂一挥,豪气万丈地说“走,创业去!”其实我个人是非常羡慕这样充满激情的人生的,无奈看过太多失败的例子,总觉得创业的成功率被高估。有位朋友到福建承包一片山林之后,很快发现这东西并没有想象中那么赚钱。终于在花光所有积蓄之后,萌发了“不如归去”的念头。虽然听上去颇有禅意,其实心里还是很懊悔的,最后不仅回到原来公司,还坐到原来的位子上。当然成功者也是有的,不要妒嫉他们,因为这是冒着风险得到的。
​	关于跳槽,除了印度之外,我还没有见过比中国工程师更爱跳槽的群体。由于每跳槽一次基本能加薪30%,的确让人难以淡定地呆在一个岗位上。不过在我看来,频繁跳槽所付出的代价恐怕高于这点收益,因为很快就会发现无处可跳了。而且更大的副作用是,多次换工作导致了各种技术都只学到皮毛,等醒悟过来已经晚了。如果某个新职位吸引你的亮点只是加薪,我建议三思而行。

​	关于理科生的骄做,在工程师群体中,有小部分年轻人至今还保持着源自高中理科班的自豪感。比如看到一本精彩的科幻小说,便觉得文科生不可能懂:如果新来的领导不是理工科出身,就感叹所处的并非技术驱动型公司:最让我吃惊的一次,是一位DBA质疑不懂技术的销售人员为什么地位那么高。这种错误的认知显然源于交际圈子的狭隘,对非技术人员的能力缺乏了解。其实你在调试代码时,他们同样在推敲文案;你在餐桌上只管品菜海侃,他们却要左右逢源,让所有宾客感到满意；你结交朋友只看心情喜好,他们在朋友圈里只说“正确”的话,永水远如沐春风地倾听;你在内部会议上发言都显拘谨,他们面对突如其来的话筒也能侃侃而谈……毫无疑问,非技术工作的“技术含量”一点都不低。幸好随着阅历的增长,大多数理科生都能改掉这个毛病。

​	关于生活,IT男们已经被打上了太多标签:宅、木讷、生活简单。这当然是种偏见,至少我身边的朋友就不是这样。不过比起国外的工程师群体,我们的业余生活似乎是单调了些。比如与我合作多年的国外同事中,有组乐队的、当冰球教练的、玩帆船的、DIY花园的……有些朋友对此羡慕不已,以为发达国家才玩得起多样化的娱乐,对此我不敢苟同。比如中国学习乐器的人数早就全球第一，在我屈指可数的女同事中,至少有三位在小时候考过钢琴十级。我所住的小区楼都配有朝南的大院子,园艺条件极佳,只是户户都铺砖硬化了…所以细想起来,经济上并不是主因,只是不够热情罢了。工程师本来就是最擅长DIY的群体, 只要行动起来,完全可以让业余生活更加丰富,成为一个更加有趣的人。

