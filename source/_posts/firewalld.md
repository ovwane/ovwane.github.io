```
#显示状态
firewall-cmd --state

#查看区域信息
firewall-cmd --get-default-zone

#将接口添加到区域，默认接口都在public
firewall-cmd --zone=public --add-interface=eth0 --permanent

#设置默认接口区域
firewall-cmd --set-default-zone=public

#查看指定接口所属区域
firewall-cmd --get-zone-of-interface=eth0

#要得到特定区域的所有配置
firewall-cmd --zone=public --list-all

#查看所有打开的端口
firewall-cmd --zone=public --list-ports

#查看所有打开的服务
firewall-cmd --zone=public --list-services

#开启服务
firewall-cmd --zone=public --add-service=http

#永久开启服务
firewall-cmd --zone=public --add-service=http --permanent

#移除服务
firewall-cmd --zone=public --remove-service=dhcpv6-client --permanent

#更新防火墙规则
firewall-cmd --reload
```