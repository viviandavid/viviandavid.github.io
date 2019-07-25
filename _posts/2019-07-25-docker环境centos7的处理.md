---
layout:     post
title:      docker的centos7环境
subtitle:   centos7的注意事项
date:       2019-07-23
author:     BY LSD
header-img: img/post-bg-swift.jpg
catalog: true
tags:
    - docker
    - centos7
    - iptables
---


### centos7安装后
#### 开启网卡
centos7镜像安装好以后，默认ens33网卡是不开启的。
可以用 ip addr 命令查看，centos6版本的命令为ifconfig
可以看到ens33下面没有inet
需要编辑
> vi /etc/sysconfig/network-scripts/ifcfg-ens33
将[ONBOOT=no],改为[ONBOOT=yes]
重启网络 再次查看
> sudo service network restart 
> ip addr

#### Firewall命令
查看防火墙状态(三个都可以)
> firewall-cmd --state
> systemctl status firewalld
> systemctl status firewalld.service

停止/开启防火墙
> systemctl stop firewalld.service
> systemctl start firewalld.service

禁止/开启开机启动
> systemctl disable firewalld.service 
> systemctl enable firewalld.service 

开启端口
> firewall-cmd --zone=public --add-port=80/tcp --permanent

 命令含义：
--zone #作用域
--add-port=80/tcp #添加端口，格式为：端口/通讯协议
--permanent #永久生效，没有此参数重启后失效

重启防火墙(两个都可以)
> systemctl restart firewalld
> systemctl restart firewalld.service

查看启动服务列表
> systemctl list-unit-files|grep enabled

查看已经开放端口
> firewall-cmd --list-port

#### docker，firewall，iptables关系
默认情况下，Docker deamon会在启动container的时候向ipt
ables中添加转发的规则。它会创建一个filter chain - DOCKER。
默认情况下, docker启动后参数中如果加了端口映射, 就会自动将端口开放给所有网络设备访问,
并且这种情况下即使在本机的系统防火墙中加规则也无效, 因为docker会自动添加一个优先级最高的针对这个映射端口全开放规则。所以，如果你是在宿主机直接运行docker，同时这个宿主机已经拥有了关于防火墙的一些配置（利用iptables）。那么，可以考虑在创建或者运行container的时候使用--iptables=false。
在实际使用过程中，没有使用iptables.service，docker的端口转发也是正常的，因为iptables一直都在。docker会创建自己的iptables链，如果firewalld重启，docker创建的链也需要重新创建。
因此，如果使用firewalld，需要先启用firewalld，再启动docker。 如果你先启动docker，再启用firewalld，或者重启
firewalld，都需要重启Docker进程。