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

![PAgP以太网头部](images/3302.png)
*图 33.2 -- PAgP以太网头部*

尽管对PAgP数据包格式的深入探讨超出了CCNA考试要求范围，下图33.3还是对一个典型的PAgP数据包所包含的字段进行了展示。PAgP数据所包含的一些字段与CCNA考试有关，在本模块的跟进中后详细进行说明这些字段。

![端口聚合协议数据帧](images/3303.png)
*图 33.3 -- 端口聚合协议数据帧*

##各种PAgP端口模式

**PAgP Port Modes**

PAgP支持不同的端口模式，而这些端口模式则决定在两台支持PAgP的交换机(two PAgP-capable switches)之间是否将形成一个以太网通道。在深入到这两种PAgP端口模式之前，一种特别的模式需要专门关注。该模式（就是“on”模式）有时被误当作一种PAgP模式。事实上，其并不是一种PAgP的端口模式。

**该`on`模式强制将某个端口无条件地置于某个通道当中。**该通道将只在另一个交换机端口接上且被配置为`on`模式时建立起来。在此模式开启后，就不会有该通道的协商被本地的以太网通道协议所执行。也就是说，这样做将有效地关闭以太网通道协商并强制该端口到该通道（when this mode is enabled, there is no negotiation of the channel performed by the local EtherChannel protocol. In other words, this effectively disables EtherChannel negotiation and forces the port to the channel）。该模式的运作与中继链路上的`switchport nonegatiate`类似。**而重要的是记住配置为`on`模式的交换机接口不会就PAgP数据包进行交换。**

采用PAgP的交换机以太网通道可被配置为以这两种模式运行：**自动**（`auto`）或**我要**（`desirable`）。这两种PAgP模式的运作，在下面的小节进行说明。

###自动模式

**Auto Mode**

自动模式(`auto` mode)是一种仅在该端口接收到一个PAgP数据包时，才将与另一PAgP端口进行协商的PAgP端口模式。在此模式开启后，该（这些）端口绝不会发起PAgP通信，而会在与邻居交换机建立一个以太网通道之前，被动地侦听任何接收到的PAgP数据包（when this mode is enabled, the port(s) will never initiate PAgP communications but will instead listen passively for any received PAgP packets before creating an EtherChannel with the neighbouring switch）。

###我要模式

**Desirable Mode**

我要模式（`desirable` mode）是一种导致该端口发起与另一PAgP端口就通道建立进行PAgP协商的PAgP端口模式（desirable mode is a PAgP mode that causes the port to initiate PAgP negotiation for a channel with another PAgP port）。也就是说，在此模式下，该端口主动尝试与运行了PAgP的另一交换机建立一个以太网通道。

总的来说，要记住配置成`on`模式的那些交换机接口不交换PAgP数据包，**但会与那些配置为`auto`或`desirable`模式的伙伴接口进行PAgP数据包的交换**（but they do exchange PAgP packets with partner interfaces configured in the auto or desirable modes）。
