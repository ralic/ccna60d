#第31天

**生成树协议**

**Spanning Tree Protocol**

##第31天任务

+ 阅读今天的课文
+ 完成今天的实验
+ 阅读ICND2记诵指南
+ 在[subneting.org](http://subnetting.org/)上花15分钟

生成树协议（Spanning Tree Protocol, STP）的作用是通过建立一个无循环的逻辑拓扑，阻止网络上循环的发生，同时还允许在具备冗余的交换网络拓扑中物理链路的存在（the role of Spanning Tree Protocol(STP) is to prevent loops from occuring on your network by creating a loop-free logical topology, while allowing physical links in redundant switched network topologies）。在网络中所用到的交换机数量极具增加，以及VLAN信息传播主要目的下，围绕网络的以太网帧的无尽循环问题开始出现。

先前CCNA考试仅要求对STP有基本理解。但当前版本则希望对此方面有极好的掌握。

今天将学习以下内容。

+ STP的使用需求，the need of STP
+ STP桥ID，STP Bridge ID
+ STP根桥选举，STP Root Bridge election
+ STP开销及优先级，STP cost and priority
+ STP根及指定端口，STP Root and Designated Ports
+ STP增强，STP enhancements
+ STP排错，Troubleshooting STP

本课对应了以下CCNA大纲要求。

+ PVSTP运作的配置和验证，configure and verify PVSTP operation
    - 对根桥选举进行描述, describe root bridge election
    - 生成树的模式, spanning tree mode

##STP的使用需求

**The Need for STP**

STP是在IEEE 802.1D标准中定义的。为维护起一个无循环的逻辑拓扑，交换机**每两秒**传递桥协议数据单元（Bridge Protocol Data Units, BPDUs）。BPDUs是一些在生成树拓扑中用到的用于传递有关端口、地址、优先级及开销等信息的数据报文。BPDUs打上了VLAN ID标签。

下图31.1现实了网络中循环是如何能创建出来的。因为所有交换机都学到VLAN 20, 同时这些交换机又通告给其它交换机其各自又能到达VLAN 20。很快，所有交换机都认为其是VLAN 20流量的源，那么都造成了循环，因此所有以VLAN 20为目的地的帧将自一台交换机往另一台不停传递。

![循环是怎么建立的](images/3101.png)

*图31.1 -- 循环是如何建立的*

STP运行着一种算法，用于根据所考虑的特定VLAN，决定哪些端口保持开放或活动，以及那些端口需要对特定VLAN关闭。

位处生成树域中的所有交换机都采用BPDUs来沟通和交换报文。STP利用BPDUs的交换，来决定出网络拓扑，而网络拓扑则是由以下三个变量决定的。

+ 所有交换机的相关唯一MAC地址（交换机识别符），the unique MAC address(switch identifier) that is associated with each switch
+ 所有交换机端口的到根桥的路径开销，the path cost to the Root Bridge associated with each switch port
+ 所有交换机端口的端口识别符（该端口的MAC地址），the port identifier(MAC address of the port) associated with each switch port

BPDUs都是每两秒发出的，此特性允许实现快速网络循环探测及拓扑信息交换。BPDUs的两个类型分别是配置BPDUs及拓扑变化通知BPDUs（Configuration BPDUs and Topology Change Notification BPDUs）; 这里只会对配置BPDUs进行说明。

##IEEE 802.1D配置BPDUs

**IEEE 802.1D Configuration BPDUs**

配置BPDUs是由LAN交换机发出，用于对生成树协议进行通信及计算。在交换机端口初始化后，该端口就被置为阻塞状态，同时一个BPDU被发送给交换机的所有端口。默认情况下，所有交换机最初都假定其为生成树的根，直到其与其它交换机进行配置BPDUs的交换为止。在某端口仍将其自身配置BPDUs视为最具吸引力（the most attractive）时，其就会持续发送配置BPDUs。这些交换机基于以下4个因素（以列出顺序），决定出最佳配置BPDU。

1. 有着最低的根桥ID的, lowest Root Bridge ID
2. 有着到根桥最低根路径开销的，lowest Root path cost to Root Bridge
3. 有着最低发送者桥ID的，lowest sender Bridge ID
4. 有着最低发送者端口ID的，lowest sender Port ID

配置BPDU交换的完成，引起以下的这些行为。

+ 选举出整个生成树域的根桥, a Root Switch is elected for the entire Spanning Tree domain
+ 选举出生成树域中所有非根交换机上的根端口，a Root Port is elected on every Non-Root Switch in the Spanning Tree domain
+ 选举出所有LAN网段中的指定交换机，a Designated Switch is elected for every LAN segment
+ 选举出所有网段的指定交换机的指定端口(根交换机上的所有活动端口也都是指定端口)，a Designated Port is elected on the Designated Switch for every segment(all active ports on the Root Switch are also designated)
+ 通过阻塞冗余路径，网络中的循环得以消除，loops in the network are eliminated by blocking redundant paths

> **注意：**随着逐步深入本模块内容，这些特性将会一一介绍。

一旦所有交换机端口都处于转发或阻塞状态，生成树网络（the Spanning Tree network）就完成了收敛, 此时配置BPDUs就由根桥以默认每两秒的间隔发出。这就是配置BPDUs的起源。配置BPDUs通过根桥上的指定端口，转发到下游邻居交换机（this is referred to as the origination of Configuration BPDUs. The Configuration BPDUs are forwarded to downstream neighboring switches via the Designated Port on the Root Bridge）。

在非根桥（a Non-Root Bridge）在提供了到根桥的最优路径的其根端口上，接收到一个配置BPDU时，就会通过其指定端口，发送出一个该BPDU的更新版本消息。这就是BPDUs的传播（when a Non-Root Bridge receives a Configuration BPDU on its Root Port, which is the port that provides the best path to the Root Bridge, it sends an updated version of the BPDU via its Designated Port(s). This is referred to as the propagation of BPDUs）。

而指定端口则是指定交换机上，在转发来自那个LAN网段数据包到根桥时，有着最低路径开销的端口（the Designated Port is a port on the Designated Switch that has the lowest cost when forwarding packets from that LAN segment to the Root Bridge）。

