#第13天

__OSPF版本3__

__OSPFv3__

##第13天任务

+ 阅读今天的理论课文
+ 回顾昨天的理论课文

今天我们要着眼于OSPFv3, 这里将学习要下面的知识。

+ OSPF基础

本模块对应了以下CCNA大纲要求。

+ 配置OSPFv3
+ 路由器ID
+ 被动接口

##OSPF第3版

__OSPF Version 3__

OSPFv3定义在RFC 2740中，而其功能与OSPFv2相同，不过OSPFv3显式地是为IPv6路由协议设计（OSPFv3 is defined in RFC 2740 and is the counterpart of OSPFv2, but it is designed explicitly for the IPv6 routed protocol）。该版本号取自此种OSPF数据包中的版本字段，该字段已被更新到数字3. OSPFv3规格主要是基于OSPFv2, 但因为加入的对IPv6的支持，而包含了一些额外的功能增强。

OSPFv2和OSPFv3能在同一台路由器上允许。也就是说，同一台物理路由器可同时路由IPv4和IPv6流量，因为每个地址家族都有不同的SPF进程；这就是说，同样SPF算法对OSPFv2和OSPFv3分别有一个单独的实例。OSPFv2和OSPFv3有以下共同点。

+ OSPFv3继续使用着为OSPFv2所用到的那些数据包。包括数据库说明数据包（Database Description, DBD）, 链路状态请求数据包（Link State Requests, LSRs），链路状态更新数据包（Link State Updates, LSUs）, 以及链路状态通告数据包（Lins State Advertisements, LSAs）
+ OSPSv3中的动态邻居发现机制及邻接关系形成过程（也就是OSPF所经历的从初始或尝试建立邻接关系到邻接关系完整建立的过程），仍然和OSPFv2中一样
+ OSPFv3仍然对遵从RFC的对各种技术的支持（OSPFv3 still remains RFC-compliant on different technologies）。比如，若在某条PPP链路上开启了OSPFv3, 那么组网类型及仍然被指定为点对点（Point-to-Point）。同样，如在FR上开启OSPFv3, 默认组网类型仍然是非广播类型（Non-Broadcast）。另外，在思科IOS软件中，默认组网类型仍可通过使用不同的接口特定命令，手动进行改变。
+ OSPFv2和OSPFv3使用同样的LSA散布及老化机制（the same LSA flooding and aging mechanism）.
+ 与OSPFv2类似，OSPFv3的路由器ID（rid）仍然需要使用一个32位的IPv4地址。当在某台运行着双栈（dual-stack, 也就是同时有IPv4和IPv6）的路由器上开启OSPFv3时， 那么与在OSPFv2中为思科IOS路由器所用到的同样的RID选定过程，也用于确定OSPFv3中要用到的路由器ID。但是，__当在一台没有接口运行着IPv4的路由器上开启OSPFv3时，就强制性要求使用路由器配置命令`router-id`来手动配置OSPFv3的路由器ID了__。
+ OSPFv3链路ID表明，这些链路并非IPv6专用的，而是仍然基于一个32位IPv4地址，跟OSPFv2中一样。
+ 在OSPFv2与OSPFv3有着这些相同点同时，重要的是掌握你必须熟悉的一些存在的明显不同点。包括下面这些。

+ 以与EIGRP类似的方式，OSPFv3在一条链路上运行（in a manner similar to EIGRP, OSPFv3 runs over a link）。这就打消了给OSPFv3网络声明语句的需求。取而代之的是，通过使用接口配置命令`ipv6 router ospf [process id] area [area id]`，将该链路配置为某个OSPF进程的组成部分。但是，与OSPFv2类似，OSPF进程号仍然是通过在全局配置模式中，使用全局配置命令`ipv6 router ospf [process id]`进行指定的。
+ OSPFv3使用本地链路地址来区分OSPFv3的邻接关系。与EIGRPv6类似，OSPFv3路由的下一跳地址将反映出邻接的或邻居路由器的本地链路地址。
+ OSPFv3引入了两种新的OSPF LSA类型。分别是链路LSA（the Link LSA），被定义为LSA类型0x0008(LSA TType 0x0008，或LSA Type 8）, 还有区域内前缀LSA（the Intra-Area-Prefix LSA），被定义为LSA类型0x0029(LSA Type 0x0029, 或LSA Type 29）。链路LSA提供了路由器的本地链路地址，及加诸路由器上的所有IPv6前缀。每条链路都有一个链路LSA。因为有不同的链路状态IDs，所以就可能有多个区域内前缀LSAs。那么，区域LSA散布范围就既可能是与参考自网络LSA的所经过网络的相关前缀，也可能是参考自路由器LSA的某台路由器或末梢区域有关的前缀（There can be multiple Intra-Area-Prefix LSAs with different Link-State IDs. The Area flooding scope can therefore be an associated prefix with the transit network referencing a Network LSA or it can be an associated prefix with a router or Stub referencing a Router LSA）。
+ OSPFv2与OSPFv3所用到的传输方式是不同的。OSPFv3报文是用（封装成）IPv6数据包发出的。
+ OSPFv3使用两个标准IPv6多播地址。多播地址`FF02::5`与OSPFv2中用到的所有SPF路由器（AllSPFRouters）地址`224.0.0.5`等价，同时多播地址`FF02::6`就是所有DR路由器（AllDRRouters）地址，且与OSPFv2中用到的`224.0.0.6`组地址等价。（这将在ICND2部分讲到）。
+ OSPFv3利用到IPv6内建的IPSec的能力，并将AH和ESP扩展头部用着一种的认证机制，而不是想在OSPFv2中可配置的为数众多的认证机制（OSPFv3 leverages the built-in capabilities of IPSec and uses the AH and ESP extension headers as an authentication mechanism instead of the numerous authentication mechanisms configurable in OSPFv2）。因此，在OSPFv3的OSPF数据包中，那些认证和AuType字段就被移除了。

+ 最终的最后一个明显区别就是，OSPFv3 Hello数据包现在不包含任何地址信息，而是包含了一个接口ID，该接口ID是发出Hello数据包路由器分配的，用于对链路做其接口的唯一区分。此接口ID成为网络LSA（the Network LSA）的链路状态ID（Link State ID）, 判断该路由器是否应成为该链路上的指定路由器（This interface ID becomes the Network LSA's Link State ID, should the router become the Designated Router on the link）。

##思科IOS软件的OSPFv2和OSPFv3配置差异

__Cisco IOS Software OSPFv2 and OSPFv3 Configuration Differences__

在思科IOS软件中，配置OSPFv2与OSPFv3时有着一些配置差异。但应注意到，这些区别与其它路由协议的IPv4和IPv6版本的差异相比，并不那么显著。

在思科IOS软件中，通过使用全局配置命令`ipv6 router ospf [process id]`，来开启OSPFv6。和OSPFv2中的情况一样，OSPF进程ID是对路由器本地有效的，并不要求其在邻接路由器上为建立邻接关系保持一致。

> __译者总结:__ 邻居路由器要形成邻接关系，要求：1. 区域号一致；2. 认证一直；3. Hello包、死亡间隔时间直一致；不要求：进程号一致。Hello数据包用于动态邻居发现和形成邻接关系，因此Hello数据包包含上述要求的参数，不包含不要求的参数。只有形成了邻接关系，才能开始发送和接受LSAs。

与EIGRPv6（将在ICND2中涵盖）所要求的一样，OSPFv3的路由器ID也必须予以手动指定，或配置成一个带有IPv4地址的运行接口（比如一个环回接口）。与EIGRPv6类似，在启用OSPFv3时，是没有网络命令的。取而代之的是，OSPF的启用，是基于各个接口的，且在同一接口上可开启多个OSPFv3实例（similar to EIGRPv6, there are no network commands used when enabling OSPFv3. Instead OSPFv3 is enabled on a per-interface basis and multiple instances may be enabled on the same interface）。

最后，当在诸如FR及ATM这样的NBMA网络上配置OSPFv3时，是在指定接口下，使用接口配置命令`ipv6 ospf neighbor [link local address]`，来指定邻居声明语句（the neighbor statements）。而在OSPFv2中，这些语句会是在路由器配置模式中配置的。

> __注意：__ 当在NBMA传输技术上配置OSPFv3时，应该使用本地链路地址来创建出静态FR地图声明语句（static Frame Relay map statements）。这是因为正是使用本地链路地址，而不是全球单播地址，建立邻接关系。比如，为给一个FR部署创建一幅静态FR地图语句并指定一台OSPF邻居路由器，就要在该路由器上应用下面的配置（在ICND2部分将对FR进行讲解）。

```
R1(config)#ipv6 unicast-routing
R1(config)#ipv6 router ospf 1
R1(config-rtr)#router-id 1.1.1.1
R1(config-rtr)#exit
R1(config)#interface Serial0/0
R1(config-if)#frame-relay map ipv6 FE80::205:5EFF:FE6E:5C80 111 broadcast
R1(config-if)#ipv6 ospf neighbor FE80::205:5EFF:FE6E:5C80
R1(config-if)#exit
```

###思科IOS软件中OSPFv3的配置和验证

__Configuring and Verifying OSPFv3 in Cisco IOS Software__


