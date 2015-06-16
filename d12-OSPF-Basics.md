#第12天

__OSPF基础知识__

__OSPF Basics__

##今天的任务

+ 阅读今天的理论课文
+ 复习昨天的理论课文

以前版本的CCNA考试只要求对OSPF有基本了解。__现今版本要求对OSPFv2、v3及多区域OSPF都要有深入掌握__。OSPF考点分别在ICND1和ICND2中都有，ICND2中增加了难度。

今天将会学到下面这些内容。

+ 链路状态要点，Link State fundamentals
+ OSPF网络类型，OSPF network types
+ OSPF的配置

本模块对应了以下CCNA大纲要求。

+ OSPF（单区域）的配置和验证
	- 单区域的好处, benefit of single area
	- OSPFv2的配置, configure OSPFv2
	- 路由器ID，router ID
	- 被动接口，passive interface

##开放最短路径优先

__Open Shortest Path First__

开放最短路径优先，是一个开放标准的链路状态路由协议（an open-standard Link State routing protocol）。链路状态路由协议就它们的链路的状态进行通告。在某台链路状态路由器开始在某条网络链路上运行时，与那个逻辑网络有关的信息就被添加到路由器的__本地__链路状态数据库(Link State Database, LSDB)中。该本地路由器此时就在其运作的链路上，发出Hello数据包，以确定是否有其它链路状态路由器也在其各自接口上运行着链路状态路由协议。__OSPF直接运行在IP协议上，使用IP的89号协议__。

## OSPF概述及基础知识

__OSPF Overview and Fundamentals__

人们为OSPF撰写了多个RFCs。在本小节，将通过一些有关OSPF最常见的几个RFCs，来了解一下OSPF的历史。OSPF工作组成立与1987年，自成立以后，该工作组发布了为数众多的RFCs。下面列出了OSPF有关的一些最常见的RFCs。

+ RFC 1131 -- OSPF规格，OSPF Specification
+ RFC 1584 -- OSPF的多播扩展, Multicast Extensions to OSPF
+ RFC 1587 -- OSPF的NSSA选项，the OSPF NSSA Option
+ RFC 1850 -- OSPF版本2的管理信息库，OSPF Version 2 Management Informaiton Base
+ RFC 2328 -- OSPF版本2
+ RFC 2740 -- OSPF版本3

RFC 1131对OSPF的第一次迭代（the first iteration of OSPF）进行了说明, 而用于确定该协议是否工作的早期测试中。

RFC 1584为OSPF提供了对IP多播流量的支持扩展。这通常被称为多播OSPF（Multicast OSPF, MOSPF）。但该标准不常用到，且最重要的是思科不支持该标准。

RFC 1587 对一种OSPF次末梢区域（Not-So-Stubby Area, NSSA）的运作方式进行了说明。一个NSSA允许通过一台自治系统的边界路由器（an Autonomous System Boundary Router, ASBR）, 使用一条NSSA的外部链路通告（Link State Advertisement, LSA）, 实现外部路由知识的注入（the injection of external routing knowledge）。在本模块的稍后会对不同的NSSAs进行说明。

RFC 1850实现了使用简单网络管理协议（Simple Network Management Protocol, SNMP）对OSPF的网络管理。在网络管理系统中，SNMP用于监测接入网络的设备中需要留心的一些情况。本标准的应用超出了CCNA考试要求范围，不会在本书中进行说明。

RFC 2328详述了OSPF版本2的最新更新，而OSPF版本2正是现今在用的默认版本。OSPF版本2最初是在RFC 1247中进行说明的，该RFC解决了OSPF版本1初次发布中发现的一系列问题，并对该协议进行了修改，实现了未来的修改不致产生出向后兼容问题。正因为这个，OSPF版本2是与版本1不兼容的。

最后，RFC 2740说明了为支持IPv6而对OSPF做出的修改。可以假定本模块中所有对OSPF一词的使用，都是指的OSPF版本2。

###链路状态基础

__Link State Fundamentals__

当对某条特定链路开启链路状态路由协议时，与那个网络有关的信息就被加入到本地链路状态数据库中。该本地路由器此时就往其运作的各链路上发送Hello数据包，以确定有否其它__链路状态路由器__也在接口上运行着。__Hello数据包用于邻居发现并在邻居路由器之间维护邻接关系__。本模块稍后部分会详细说明这些消息。

在找到一台邻居路由器后，本地路由器就尝试建立一个邻接关系（adjacency）, 假定两台路由器在同一子网中，并在同一区域中，同时诸如认证方法及计时器等其它参数都是一致的（identical）。这样的邻接关系令到两台路由器，能够各自将摘要的LSDB信息通告给对方。而这种信息交换，交换的不是实在的详细数据库信息，而是数据的摘要。

各台路由器参照其自己的本地LSDB，对收到的摘要信息做出评估，以确保其有着最新信息。如邻接关系的一侧认识到它需要一个更新，路由器就从邻接路由器请求新信息。而来自邻居路由器的更新包含了LSDB中现在的数据。此交换过程持续下去，直到两台路由器拥有同样的LSDB。OSPF用到不同类型的消息，来交换数据库信息，以确保所有路由器都有着网络的统一视图。这些不同的数据包类型将在本模块稍后进行详细说明。

跟着数据库交换，SPF算法运行起来，创建出到某区域中, 或在网络主干中的所有主机的最短路径树（a shotest path tree to all hosts in an area or in the network backbone）, 将执行该运算的路由器，作为该树的根。在第10天中，对SPF算法进行了简要介绍。

###OSPF基础

__OSPF Fundamentals__

与EIGRP能够支持多个网络层协议不同，OSPF只能支持IP，也就是IPv4和IPv6。和EIGRP相同的是，OSPF支持VLSM、认证及在诸如以太网这样的多路访问（Multi-Access）网络上，于发送和接收更新时，利用IP多播技术（IP Multicast）。

OSPF是一种层次化的路由协议，将网络以逻辑方式，分为称作区域的众多子域。这种逻辑分段用于限制链路状态通告在某个OSPF域中扩散的范围（OSPF is a hierarchical routing protocol that logically divides the network into subdomains referred to as areas. This logical segmentation is used to limit the scope of Link State Advertisements(LSAs)）。LSAs是由运行着OSPF的路由器发出的特殊类型的数据包。在区域内和区域间用到不同类型的LSAs。通过限制某些类型的LSAs在不同区域之间的传播，OSPF的层次化实现有效地减少了在OSPF网络中路由协议流量的数量。

> __注意：__ OSPF的这些LSAs会在第39天详细说明。

在多区域OSPF网络中，必须指定一个区域作为__骨干区域, 或者叫0号区域__（the backbone area, or Area 0）。该OSPF骨干就是此OSPF网络的逻辑中心。__其它非骨干区域都必须物理连接到这个骨干区域__。但是，在非骨干区域和骨干区域之间有着一条物理连接，并非总是可能或可行的，所以OSPF标准允许使用到骨干区域的虚拟连接。这些虚拟连接也就是常说的虚拟链路，但此概念是不包括在当前的CCNA大纲中的。


