#第33天

**各种以太网通道及链路聚合协议**

**EtherChannels and Link Aggregation Protocols**

##第33天任务

+ 阅读今天的课文
+ 复习昨天的课文
+ 完成今天的实验
+ 阅读ICND2记诵指南
+ 在网站[subnetting.org](http://subnetting.org/)上花15分钟

思科IOS软件允许管理员将交换机上的多条物理链路（multiple physical links）结合成为一条单一的逻辑链路。这样做提供了一种负载分配以及链路冗余的理想方案，同时该逻辑链路可同时为二层及三层子系统所使用。

今天将学习以下内容。

+ 掌握各种以太网通道, Understanding EtherChannels
+ 端口聚合协议概述，Port Aggregation Protocol(PAgP) overview
+ 各种端口聚合协议端口模式，PAgP port modes
+ PAgP 以太网通道协议的数据包转发
+ 链路聚合控制协议概述，Link Aggregation Control Protocol(LACP) overview
+ 各种LACP端口模式，LACP port modes
+ 不同以太网通道负载分配方法，EtherChannel load-distribution methods
+ 不同二层以太网通道的配置和验证，Configuring and verifying Layer 2 EtherChannels

本课对应了以下ICND2大纲要求。

+ 不同以太网通道技术，EtherChannels

##掌握各种以太网通道

**Understanding EtherChannels**

一个以太网通道是由一些物理的、单独的FastEthernet、GigabitEthernet或Ten-GigabitEthernet(10Gbps)链路绑定在一起所构成的一条单一逻辑链路，如下面的图33.1所示。由FastEthernet链路所构成的一个以太网通道叫做一个FastEtherChannel(FEC)；一个由GigabitEthernet链路所构成的通道被称为一个GigabitEtherChannel(GEC)；最后，一个由Ten-GigabitEthernet链路所构成的以太网通道则被称为是Ten-GigabitEtherChannel(10GEC)。

![以太网通道的物理和逻辑视图](images/3301.png)
*图33.1 -- 以太网通道的物理和逻辑视图*

**每个以太网通道最多可由8个端口构成。**一个以太网通道中的物理链路**必须有着相似特性**，诸如是定义在同一个VLAN中、或有着同样的速率以及双工设置。当在思科Catalyst交换机上配置以太网通道时，重要的是记住在不同Catalyst交换机型号之间，所支持的以太网通道数目会有所不同。

比如，在Catalyst 3750系列交换机上，支持的数目是1到48个；在Catalyst 4500系列交换机上，是1到64个；而在旗舰的Catalyst 6500系列交换机，有效的以太网通道配置数目则是依据软件版本（the software release）。对于早于12.1(3a）E3的版本，有效数值是1到256；对于12.1(3a）E3、12.1(3a）E4以及12.1(4)E1，有效数值是1到64。而对于12.1(5c)EX及以后的版本，支持最大64的数量，范围从1到256。

>**注意：**并不要求知道不同IOS版本中所支持的以太网通道数量。

可用于自动创建一个以太网通道组（an EtherChannel group）的链路聚合协议有两个选项：**端口聚合协议**（Port Aggregation Protocol, PAgP）及**链路聚合控制协议**(Link Aggregation Control Protocol, LACP)。**PAgP是一个思科专有协议，同时LACP则是IEEE 802.3ad用于从几条物理链路建立逻辑链路规格的一部分。**本模块中将详细对这两个协议进行讲述。

##端口聚合协议概述

**Port Aggregation Protocol Overview**

端口聚合协议（Port Aggregation Protocol, PAgP）是一个思科专有的实现以太网通道自动建立的链路聚合协议。默认下，PAgP数据包在可作为以太网通道的端口之间进行发送（PAgP packets are sent between EtherChannel-capable ports），以就以太网通道的形成进行协商。这些数据包被发送到多播目的MAC地址`01-00-0C-CC-CC-CC`(the destination Multicast MAC address `01-00-0C-CC-CC-CC`)，而该多播MAC地址也是CDP、UDLD、VTP以及DTP所用到同一多播地址。下图33.2显示了在线路上所见到的一个PAgP数据帧中所包含的字段。

