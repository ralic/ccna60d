#第34天

**那些第一跳冗余协议**

**First Hop Redundancy Protocols**

##第34天任务

+ 阅读今天的课文
+ 复习昨天的课文
+ 完成今天的实验
+ 阅读ICND2记诵指南
+ 在网站[subnetting.org](http://subnetting.org/)上花15分钟

在交换网络的设计和实施时，高可用性是一个整体组成。HA是思科IOS软件中所交付的技术，提供了网络范围的恢复能力，以提升IP网络的可用性。所有网段都必须是具有回弹能力而能足够快地从故障中恢复过来，令到故障对于用户及网络应用是透明的（High Availability(HA) is an integral component when designing and implementing switched networks. HA is technology delivered in Cisco IOS software that enables networkwide reilience to increase IP network availability. All network segments must be resilient to recover quickly enough for faults to be transparent to users and network applications）。这些第一跳冗余协议（First Hop Redundancy Protocols, FHRPs）提供了交换LAN环境中的冗余。

今天你将学到以下内容。

+ 热备份路由器协议，Hot Standby Router Protocol
+ 虚拟路由器冗余协议，Virtual Router Redundancy Protocol
+ 网关负载均衡协议，Gateway Load Balancing Protocol

该课对应了以下ICND2大纲要求。

+ 认识高可用性（FHRP），Recognise High Availability(FHRP)
    - HSRP
    - VRRP
    - GLBP

##热备份路由协议

**Hot Standby Router Protocol**

热备份路由器协议是一项**思科专有的**第一跳冗余协议。HSRP允许两台配置为同一HSRP组的部分的物理网关，共享同样的虚拟网关地址。与此两台网关位于同一子网上的主机，被配置为将该虚拟网关IP地址作为它们的默认网关（HSRP allows two physical gateways that are configured as part of **the same HSRP group** to share the same virtual gateway address. Network hosts residing on the same subnet as the gateways are configured with the virtual gateway IP address as their default gateway）。

当主要网关（the primary gateway）处于可运作状态，其就对以HSRP组（the HSRP group）的虚拟网关IP地址为目的地址的数据包进行转发。而在该主要网关失效时，第二（次要）网关（the secondary gateway）就承担主要网关的角色，而转发所有发送到虚拟网关IP地址的数据包。下图34.1对某个网络中HSRP的运作进行了演示。

![热备份路由器协议的运作](images/3401.png)

*图34.1 -- 热备份路由器协议的运作*

