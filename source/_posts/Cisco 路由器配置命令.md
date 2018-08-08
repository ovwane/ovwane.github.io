---
date: 2017-11-05 20:31:30
---

# Cisco 路由器配置命令

### 改变命令状态 
```
任务  命令 
进入特权命令状态 enable 
退出特权命令状态 disable 
进入设置对话状态 setup  
进入全局设置状态 config terminal 
退出全局设置状态 end  
进入端口设置状态 interface type slot/number 
进入子端口设置状态 interface type number.subinterface [point-to-point | multipoint] 
进入线路设置状态 line type slot/number 
进入路由设置状态 router protocol 
退出局部设置状态  exit
```

路由器的状态

```
命令状态
route1>
特权命令状态(进入特权enable，退出特权disable)
route1>enable
route1#
特权状态下的命令
进入设置对话状态 setup
route1#setup
         --- System Configuration Dialog ---
Continue with configuration dialog? [yes/no]:

进入全局设置状态 config terminal(conf t)
route1#conf t
Enter configuration commands, one per line.  End with CNTL/Z.
route1(config)#

退出全局设置状态 end
route1(config)#end
route1#
进入端口配置
route2#show ip interface
FastEthernet0/0 is administratively down, line protocol is down
  Internet protocol processing disabled

route2(config)#interface FastEthernet0/0
route2(config-if)#
```

Cisco路由器初始化配置
设置路由器内部时钟

```
全局配置模式下 clock timezone GMT +8
exit
特权模式下
clock set 11:54:00 october 22 2017
```

1.路由器名
2.特权状态密码
3.Console密码 
```
line console 0
password 密码
login
```

4.AUX密码

```
line aux 0
password 密码
login
```

5.VTY密码
```
使用远程登录，需要先配置特权密码
line vty 0 4
pass 密码
login

限制IP远程登录
access-list 1 permit 12.1.1.1
line vty 0 4
access-class 1 in
end
```


5.配置接口IP

```
interface 
ip address 10.0.0.1 255.255.255.0
description "描述信息"
ip address 10.1.1.1 255.255.255.0 secondary
no shutdown
```

模式转换
用户模式-->特权模式 enable
特权模式-->全局配置模式 config t
全剧配置模式-->接口模式 interface
全局配置模式-->线控模式 line


默认进入的是命令状态，
config 之后 状态上有

```
Cisco3725124-25.T14-1#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Cisco3725124-25.T14-(config)#
```

% Unrecognized command
％无法识别的命令

设置路由器主机名

```
默认模式转换到全局配置模式 confit t

Cisco3725124-25.T14-1#configure terminal
Enter configuration commands, one per line.  End with CNTL/Z.
Cisco3725124-25.T14-(config)#hostname riga
```

设置特权密码
全局配置模式下执行

```
route2(config)#enable secret 密码
```

设置访问用户及密码