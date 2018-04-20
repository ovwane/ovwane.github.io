## 网络

### 使用 iperf 检测主机间网络带宽

**背景介绍：**
在调试网络时，经常需要检测两台主机间的最大带宽，我一直使用iperf命令，效果很好很准确，但发现有一些运维朋友并不知道有这个工具，于是打算写篇文章简单介绍一下。

**具体操作：**

```shell
yum -y install epel-release
  yum update
  yum -y install iperf
```

服务端

服务器端需要开启 tcp 5001端口

启动` iperf -s`


客户端 `iperf -c IP地址`



macOS

`brew install iperf3`



参考资料：**

http://heylinux.com/archives/3825.html

<https://blogs.oracle.com/mandalika/entry/measuring_network_bandwidth_using_iperf>

