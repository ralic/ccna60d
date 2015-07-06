#第7天

**互联网协议版本6**

**IPv6**

##第七天任务

+ 阅读下面的课文理论部分
+ 阅读ICND1记诵指南

IPv6已经开发了很多年，且已在全世界网络中投入使用（与IPv4共同运行）。许多网络工程师在面对不得不学习一种新的分址方式时，表现出了他们的恐惧，笔者也曾听他们中的许多人说希望在IPv6成为一项必备技能之前能够退休。

恐惧是站不住脚的，是没有有事实依据的。IPv6是一种对用户友好格式，一旦对其熟悉了，就会发现其是IPv4的改良，而你可能会优先选用IPv6。**CCNA考试中IPv6占了很大部分**; 为此，需要**掌握其工作原理**，及**如何配置IPv6地址**，**掌握其有关标准**，并**应用IPv6来满足网络的各项需求**。

今天将会学到下面这些知识点。

+ IPv6的历史
+ IPv6分址格式
+ 应用IPv6
+ IPv6子网划分

本模块对应了以下CCNA大纲要求。

+ 拿出恰当的IPv6分址方案，以满足某个LAN/WAN环境的分址要求
+ 正确描述IPv6的各种地址
	- 全球单播地址, Global Unicast addresses
	- 多播地址, Multicast addresses
	- 本地链路地址, Link-Link addresses
	- 本地唯一地址，Unique-Local addresses
	- 扩展唯一识别符，Extended Unified Identifier 64, EUI-64
	- 自动配置地址（autoconfiguration）

##IPv6历史

**History of IPv6**

###满足目标吗？

**Fit for Purpose?**

在Tim Berners-Lee爵士于1989年发明WWW时，他无法预测到该技术对世界的巨大影响。个人计算机曾经贵得高攀不起，此外，除非能够负担得起昂贵的WAN连接费用，否则就没有方便的长距离通信方法。那时也没有大家共同遵循的通信模型。

那时，某些事需要一些变化，以IP这种新型分址标准的的形式，变革发生了。业界从犯下的大量失误中终有收获，并在对商业需求的回应下，IETF早在1998年就发布了众多IPv6标准中最早的一些标准。

并不会有一个日期，能够整个地从IPv4转变为IPv6；而是网络将会逐渐地变为同时运行IPv4和IPv6, 并最终IPv4会滚粗。当下，全部互联网流量的近1%运行在IPv6上（来源：Yves Poppe, IPv6 -- A 2012 Report Card）。

###为何要迁移

**Why Migrate?**

笔者已经指出，在IPv4发明时，互联网不是由普罗大众所使用的，也没有使用的必要。那时还没有网站，没有电子商务，没有移动网络，没有社交媒体。就算买得起PC，拿来也干不了什么事。现在的情况是几乎所有人都在线上了。我们使用互联网来完成日常工作，很多业务都依赖互联网而存在。很快我们又会使用移动装置来管理我们的汽车及家庭安防，来打开咖啡机，设置空调，设定电视录制爱看的电视剧等等。

这些事情已经在发生当中，不光在欧洲和美国，在那些有着数十亿人口的快速发展中国家，比如印度和中国，都在发生着。IPv4就是不能胜任了，就算勉强可以，也没有足够的地址来满足需求。

下面是迁移到IPv6所能带来的一些好处。

+ 简化了的IPv6数据包头部
+ 更大的地址空间
+ IPv6层次化的分址方法
+ IPv6的扩展性扩充性
+ IPv6消除了广播
+ 无状态的自动配置
+ 集成移动能力
+ 集成了安全增强

我喜欢从其**数据包层的探究，来分析IPv6, 同时也会去探究IPv6中可用的许多种类型的包头部**，但限于篇幅，同时考试中也不会考到这两点，所以就不包含这两方面的内容了。而着重在为考试和成为一名思科工程师，所需要掌握的内容上。

###十六进制计数

**Hex Numbering**

这里很有必要回顾一下有关十六进制计数的内容。

我们知道十进制数有着从0到9的10个数字。二进制则有从0到1的2个数字。那么十六进制就有从0到F的16个数字。这些地址分别叫做基数10、基数2和基数16的地址。

可以发现各个计数系统都是从0开始的，就像下面这样。

十进制 -- 0,1,2,3,4,5,6,7,8,9
二进制 -- 0,1
十六进制 -- 0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F

在写下这些地址时，可能不会意识到是在使用那些从右往左的列；最右边的列是权重为1的列，接下来的列，是权重为计数基数的前一列序号次幂的列。如同下表所示。

<table>
<tr><th>计数基数</th><th>N乘计数基数的3次幂</th><th>N乘计数基数的2次幂</th><th>N乘计数基数的1次幂</th><th>N乘1</th></tr>
<tr><td>10 -- 十进制</td><td>1000</td><td>100</td><td>10</td><td>1</td></tr>
<tr><td>2 -- 二进制</td><td>8</td><td>4</td><td>2</td><td>1</td></tr>
<tr><td>16 -- 十六进制</td><td>4096</td><td>256</td><td>16</td><td>1</td></tr>
</table>

可以看出每一位都从其右边的那位继承了数值。十进制基数是10乘1。二进制是1, 同时1乘了计数系统的2。如对三种计数系统的最后一个十六进制数位进行比较，就会发现将十六进制作为IPv6分址首选格式的原因了。

<table>
<tr><th>十进制</th><th>二进制</th><th>十六进制</th></tr>
<tr><td>0</td><td>0000</td><td>0</td></tr>
<tr><td>1</td><td>0001</td><td>1</td></tr>
<tr><td>2</td><td>0010</td><td>2</td></tr>
<tr><td>3</td><td>0011</td><td>3</td></tr>
<tr><td>4</td><td>0100</td><td>4</td></tr>
<tr><td>5</td><td>0101</td><td>5</td></tr>
<tr><td>6</td><td>0110</td><td>6</td></tr>
<tr><td>7</td><td>0111</td><td>7</td></tr>
<tr><td>8</td><td>1000</td><td>8</td></tr>
<tr><td>9</td><td>1001</td><td>9</td></tr>
<tr><td>10</td><td>1010</td><td>A</td></tr>
<tr><td>11</td><td>1011</td><td>B</td></tr>
<tr><td>12</td><td>1100</td><td>C</td></tr>
<tr><td>13</td><td>1101</td><td>D</td></tr>
<tr><td>14</td><td>1110</td><td>E</td></tr>
<tr><td>15</td><td>1111</td><td>F</td></tr>
</table>

为提供足够的地址来满足我们在今后许多年的需求，IPv6已被设计成可以提供数以百亿亿的地址。为做到这点，计数范围从32位二进制数，扩展到128位。每4位可用一个十六进制数位表示（这可从上面的图表看出）。逻辑上推断就是2个十六进制位给出的是8位二进制数，也就是一个字节。

一个IPv6地址有128位长，又被分为8组的16位，在以完整格式写出时，用冒号将每组分开。每4位十六进制数的范围是`0000`到`FFFF`，其中F是十六进制计数方法中最高的数。

<table>
<tr><th>0000</th><th>0000</th><th>0000</th><th>0000</th><th>0000</th><th>0000</th><th>0000</th><th>0000</th></tr>
<tr><td>to</td><td>to</td><td>to</td><td>to</td><td>to</td><td>to</td><td>to</td><td>to</td></tr>
<tr><td>FFFF</td><td>FFFF</td><td>FFFF</td><td>FFFF</td><td>FFFF</td><td>FFFF</td><td>FFFF</td><td>FFFF</td></tr>
</table>

##IPv6分址

**IPv6 Addressing**

我们已经知道，IPv6用到128位的地址。因为**此种地址格式不同于我们所熟悉的IPv4地址格式，在初次见到时通常会犯迷糊**。但是，一旦掌握了，那么就知道其逻辑和结构都十分简单。**这些128位的IPv6地址，使用了十六进制数值**（也就是说，0到9以及字母A到F）。**而在IPv4中，子网掩码既可以用CIDR表示法表示**（比如`/16`或`/32`）, **也可以用点分十进制表示法表示**（dotted-decimal notation, 比如`255.255.0.0`或`255.255.255.255`）, 但**在IPv6中，子网掩码只用CIDR表示法表示**，因为IPv6地址的长度很长。全球范围内的128位IPv6地址，由下面3部分组成。

+ 由服务商分配的前缀，the provider-assigned prefix
+ 站点前缀，the site prefix
+ 接口或主机ID，the interface or host ID

所谓服务商分配的前缀，也被称作**全球地址空间**(the global address space)，是一个**48位**的前缀，又被分为下面的3部分。

+ 16位保留的IPv6全球前缀，the 16-bit reserved IPv6 global prefix
+ 16位服务商持有的前缀，the 16-bit provider-owned prefix
+ 16位服务商分配给其客户的前缀，the 16-bit provider-assigned prefix

**IPv6全球前缀，用于表示IPv6全球地址空间**（the IPv6 global address space）。**所有IPv6全球互联网地址，都位于从`2000::/16`到`3FFF::/16`的范围**。而16位**服务商持有的IPv6前缀，是IANA分配给服务商，且归其所有的**。ISP持有前缀，处于`0000::/32`到`FFFF::/32`范围。

**接下来的16位，表示由实际服务提供商从其分到前缀地址空间中，再分配给某个组织的IPv6前缀**。该前缀处于`0000::/48`到`FFFF::/48`范围。于是，前48位就共同构成了IPv6地址第一部分 -- 服务提供商分配的前缀，如下图7.1所示。

![48位服务提供商分配的IPv6前缀](images/0701.png)

*图7.1 -- 48位服务提供商分配的IPv6前缀*

在48位服务商分配的前缀之后，紧接着的16位就是**站点前缀**。站点前缀的子网掩码长度是`/64`, 该子网掩码已经包括了之前的48位服务商分配的前缀。**此前缀长度允许在每个站点前缀中有2的64次幂个地址**。图7.2演示了该16位站点前缀。

![16位的IPv6站点前缀](images/0702.png)

*图7.2 -- 16位的IPv6站点前缀*

而在站点前缀之后，接下来的64位就用于接口或主机的分址了。**IPv6地址的接口或主机ID部分，表示了某个IPv6子网上的某台网络设备或主机**。至于确定接口或主机地址的不同方式，在今天的课程稍后会详细讲到。图7.3说明了IPv6的这些前缀是如何分配的。

![IPv6前缀的分配](images/0703.png)

*图7.3 -- IPv6前缀的分配*

参考图7.3, 客户一旦收到由ISP提供的/48前缀，就可以该前缀范围内，对站点前缀和主机或接口地址进行自由分配了。基于可用的地址空间全部容量，任何单一机构客户，只需一个的服务商分配前缀，机构网络上的所有设备就保证可以分配到一个唯一IPv6全球地址。因此，IPv6绝对不需要NAT这样的技术。

###IPv6地址表示法

**IPv6 Address Representation**

IPv6地址可像下面这三种方式进行表示。

+ 首选的或者说完整地址表示/形式
+ 压缩的表示法
+ 带有一个嵌入了IPv4地址的IPv6地址

尽管在以文本格式表示128位IPv6地址时，**首选形式或表示法是最常用的方式**，**熟悉其它两种IPv6地址表示法**也很重要。下面会对这三种方式进行说明。

###首选形式

**The Prefered Form**

**IPv6地址的首选表示法**(the prefered representation for an IPv6 address)，有着最长的格式，又被称作**IPv6地址的完整形式**(the complete form of an IPv6 address)。此格式表示法使用32个十六进制字符，以构成一个IPv6地址。通过将某地址写作共八组的十六进制字段，用冒号将这8个字段分开（比如，`3FFF:1234:ABCD:5678:020C:CEFE:FEA7:F3A0`）。

每个16位字段，由4个十六进制字符表示，那么每个字符就表示了4位。每个16位十六进制字段，可以是`0x0000`和`0xFFFF`之间的值，但就如同今天后面讲到的那样，**第一组的一些数值已被保留，那么所有可能的数值都不被使用**（as will be described later in this module, different values have been reserved for use in the first 16 bits, so all possible values are not used）。在书写IPv6地址时，**十六进制字符不区分大小写**。也就是说，`2001:ABCD:0000`和`2001:abcd:0000`是完全一样的。IPv6地址表示法的完整形式，在下图7.4中有演示。

![IPv6地址表示法的首选形式](images/0704.png)

*图7.4 -- IPv6地址表示法的首选形式*

下面的这些IPv6地址，是完整形式下的有效IPv6地址实例。

+ `0000:0000:0000:0000:0000:0000:0000:0001`
+ `2001:0000:0000:1234:0000:5678:af23:bcd5`
+ `3FFF:0000:0000:1010:1A2B:5000:0B00:DE0F`
+ `fec0:2004:ab10:00cd:1234:0000:0000:6789`
+ `0000:0000:0000:0000:0000:0000:0000:0000`

###压缩的表示法

**Compressed Representation**

压缩的表示法，允许以两种压缩方式之一，对IPv6地址进行压缩。第一种压缩方式，允许使用**一对**冒号（`::`）, 对**一个有效IPv6地址中的那些由0s构成的连续16位字段的连续的0值，或者IPv6地址中前面的0s**，进行压缩。在使用这种方式时，**务必要记住，双冒号在一个IPv6地址中，只能使用一次**。

在用到压缩格式时，各个节点及各台路由器，负责去对双冒号两侧的位数进行计数，以判断出该双冒号究竟表示了多少个0s。表7.1显示了那些IPv6地址的首选形式及其压缩表示法。

*表7.1 -- 首选和压缩形式下的完整IPv6地址*

<table>
<tr><th>完整IPv6地址表示法</th><th>压缩的IPv6地址表示法</th></tr>
<tr><td><pre>0000:0000:0000:0000:0000:0000:0000:0001</pre></td><td><pre>::0001</pre></pre></td></tr>
<tr><td><pre>2001:0000:0000:1234:0000:5678:af23:bcd5</pre></td><td><pre>2001::1234:0:5678:af23:bcd5</pre></td></tr>
<tr><td><pre>3FFF:0000:0000:1010:1A2B:5000:0B00:DE0F</pre></td><td><pre>3FFF::1010:1A2B:5000:B00:DE0F</pre></td></tr>
<tr><td><pre>FEC0:2004:AB10:00CD:1234:0000:0000:6789</pre></td><td><pre>FEC0:2004:AB10:CD:1234::6789</pre></td></tr>
<tr><td><pre>0000:0000:0000:0000:0000:FFFF:172.16.255.1</pre></td><td><pre>::FFFF:172.16.255.1</pre></td></tr>
<tr><td><pre>0000:0000:0000:0000:0000:0000:172.16.255.1</pre></td><td><pre>::172.16.255.1</pre></td></tr>
<tr><td><pre>0000:0000:0000:0000:0000:0000:0000:0000</pre></td><td><pre>::</pre></td></tr>
</table>

跟前面指出的那样，在单个的IPv6地址中，双冒号不能多于一次地使用。比如说，如要对这个完整IPv6地址`2001:0000:0000:1234:0000:0000:af23:bcd5`以压缩形式表示，那么你就只能使用双冒号一次，就算在该地址中有两组连续的0s字符串。那么，在尝试将该地址压缩成`2001::1234::af23:bcd5`，就被看成是非法的；但是此IPv6地址既可以压缩成`2001::1234:0:0:af23:bcd5`, 也可以压缩成`2001:0:0:1234::af23:bcd5`, 取决于自己喜好。

第二种IPv6压缩地址表示法，对于**单个的16位字段，及前导0s，可从该IPv6地址中省略成单个的0**。在使用该方法时，如某个16位字段都是0, 那么就必须用一个0来表示此字段。在这种情况下，并非所有的0都能省略。表7.2中展示了首选形式的IPv6地址，以及它们怎样通过第二种IPv6压缩形式表示法进行压缩。

*表7.2 -- 以替代的压缩形式表示的完整IPv6地址*

<table>
<tr><th>完整IPv6地址表示法</th><th>压缩IPv6地址表示法</th></tr>
<tr><td><pre>0000:0123:0abc:0000:04b0:0678:f000:0001</pre></td><td><pre>::123:abc:0:4b0:678:f000:1</pre></td></tr>
<tr><td><pre>2001:0000:0000:1234:0000:5678:af23:bcd5</pre></td><td><pre>2001::1234:0:5678:af23:bcd5</pre></td></tr>
<tr><td><pre>3FFF:0000:0000:1010:1A2B:5000:0B00:DE0F</pre></td><td><pre>3FFF::1010:1A2B:5000:B00:DE0F</pre></td></tr>
<tr><td><pre>fec0:2004:ab10:00cd:1234:0000:0000:6789</pre></td><td><pre>fec0:2004:ab10:cd:1234::6789</pre></td></tr>
<tr><td><pre>0000:0000:0000:0000:0000:FFFF:172.16.255.1</pre></td><td><pre>::FFFF:172.16.255.1</pre></td></tr>
<tr><td><pre>0000:0000:0000:0000:0000:0000:172.16.255.1</pre></td><td><pre>::172.16.255.1</pre></td></tr>
<tr><td><pre>0000:0000:0000:0000:0000:0000:0000:0000</pre></td><td><pre>::</pre></td></tr>
</table>

这里就有了两种以压缩形式表示完整IPv6地址的方法，要记住，**两种方法之间并不互相排斥**。也就是说，在表示一个IPv6地址时，可以同时使用这两种方法。当某个完整IPv6地址既包含了连续0s字符串，又在其它字段中有前导0s时，这经常会用到。表7.3展示了一些既包含了连续0s字符串，又有前导0s的一些IPv6地址的完整形式，以及如何将这些地址表示成压缩形式。

*表7.3 -- 使用了两种压缩格式方法的完整IPv6地址*

<table>
<tr><th>完整IPv6地址表示法</th><th>压缩IPv6地址表示法</th></tr>
<tr><td><pre>0000:0000:0000:0000:1a2b:000c:f123:4567</pre></td><td><pre>::1a2b:c:f123:4567</pre></td></tr>
<tr><td><pre>FEC0:0004:AB10:00CD:1234:0000:0000:6789</pre></td><td><pre>FEC0:4:AB10:CD:1234::6789</pre></td></tr>
<tr><td><pre>3FFF:0c00:0000:1010:1A2B:0000:0000:DE0F</pre></td><td><pre>3FFF:c00:0:1010:1A2B::DE0F</pre></td></tr>
<tr><td><pre>2001:0000:0000:1234:0000:5678:af23:00d5</pre></td><td><pre>2001::1234:0:5678:af23:d5</pre></td></tr>
</table>

###带有一个嵌入的IPv4地址的IPv6地址

**IPv6 Addresses with an Embedded IPv4 Address**

这是**第三种IPv6地址表示法，用于在IPv6地址内部使用一个IPv4地址**。尽管这也是有效的IPv6地址，但请记住这种方法是不赞成的做法，同时也在考虑废弃这种方法，因为该方法仅适用于从IPv4到IPv6的过渡。

##IPv6地址的不同类型

**The Different IPv6 Address Types**

**IPv4支持4中不同类别的地址，分别是任意播（Anycast）、广播(Broadcast)、多播(Multicast)及单播(Unicast)地址**。尽管在本教程之前的模块中并未用到任意播一词, 但请仍要记住，**任意播地址并非特殊类型的地址**。相反，**一个任意播地址简单地就是一个分配给多个接口的IP地址**。常见的使用了任意播的技术包括IP多播应用(IP Multicast implementations)，以及6to4中继应用(6to4 relay implementation)。

>**注意：** 6to4是一种IPv4迁移到IPv6的过渡机制。对于CCNA考试来说，只需知道有这么个东西就行了。

**在任意播寻址方式下，设备使用从路由协议度量值上看离它们最近的那个公共地址**(the common address)。假如该主要地址不可达时，就会使用下一个最近的地址（with Anycast adressing, devices use the common address that is closest to them based on routing protocol metric. The next closest address is then used in the event that the primary address is no longer reachable）。此概念在下图7.5中进行了演示。

![理解任意播寻址方式](images/0705.png)

*图7.5 -- 理解任意播寻址方式*

在图7.5中，R1和R2都有一个配置了公共地址15.1.1.254/32的环回接口Loopback 254。该前缀此时会经由EIGRP进行通告。默认情况下，R1和R2都会经由它们各自的相应环回接口，优先选择15.1.1.254/32前缀，因为该前缀是一个直接连接的子网。因此，两台路由器上所使用的公共地址绝不会发生冲突。

假定是在一般EIGRP度量值计算下，则R3和R5都会优先选择R1通告的那个任意播地址（the Anycast address）, 这是由于其有着较小的内部网关协议（Interior Gateway Protocol, IGP）度量值(due to the lower IGP metric)。同样R4和R6则会优先选择R3通告的那个任意播地址，也是由于其有着较小的IGP度量值。要是R1或R3中的某台失效，网络中的路由器就会使用由剩下的那台路由器通告的任意播地址了。某个组织在应用任意播分址时，既可以使用RFC 1918中定义的地址空间中的某个单播地址(私有地址)，也可以使用其公网地址块中的某个单播地址。

>**注意：**当前的CCNA考试并不要求你采用任何的任意播分址或解决方案。但熟悉此概念是必要的。在完成路由章节的学习后，你将更为明白。

在CCNA层次，IPv4的广播、多播及单播地址都无需更为详尽地阐述，本课程及本模块都不会对它们进行更为详细的说明。与IPv4支持这四种类型的地址相比，IPv6废除了广播地址，同时取而代之的仅支持以下类型的地址。

+ 本地链路地址，Link-Local addresses
+ 站点本地地址，Site-Local addresses
+ 可聚合全球单播地址，Aggregatable Global Unicast addresses
+ 多播地址，Multicast addresses，已被废除，取而代之的是本地唯一地址（Unique-Local addresses, ULAs)
+ 任意播地址，Anycast addresses
+ 环回地址，Loopback addresses
+ 未指明的地址，Unspecified addresses，`::/128`

###本地链路地址

**Link-Local Addresses**

**IPv6本地链路地址只能用在本地链路上**（也就是一个设备间所共享的网段），**是在某个接口上开启了IPv6时，自动分配给接口的**。这些地址分配自本地链路前缀（the Link-Local prefix）**`FE80::/10`**。记住`FE80::/10`等价于`FE80:0:0:0:0:0:0:0/10`, 又可以表示为`FE80:0000:0000:0000:0000:0000:0000:0000/10`。为了构成该地址，从第11到64位被设置为0, 同时接口的EUI-64(Extended Unique Identifier 64，64位扩展唯一标识）给追加到本地链路地址上去，作为下一顺位的64位（the lower-order 64 bits）。**EUI-64是由IEEE分配给接口产商的24位ID(Organization Unified Identifier, OUI)，以及产商分配给其产品的40位值构成**。本模块稍后会更为详细地说明EUI-64分址。图7.6演示了本地链路地址的格式。

![IPv6本地链路分址](images/0706.png)

*图7.6 -- IPv6本地链路分址*

**本地链路地址是唯一的，一旦分配给了某个接口，就不再改变**。这就是说，某个接口在分配了一个公网IPv6地址后（比如，`2001:1000::1/64`），就算该公网IPv6前缀发生改变(变成`2001:2000::1/64`)，本地链路地址也是不会改变的。这允许主机或路由器在IPv6全球互联网地址改变时，对其邻居始终保持可达。而**IPv6路由器是不会转发那些以本地链路地址作为源或目的地址的数据包，到其它IPv6路由器的**。

###站点本地地址

**Site-Local Addresses**

站点本地地址是**那些仅在某个站点内部使用的地址**。与本地链路地址不同，必须**在网络设备上手动为其配置站点本地地址**。这些地址就是在IPv6中，与RFC 1918所定义的私有IPv4地址等价的地址，对于那些没有可全球路由IPv6地址空间的组织，可以使用这些地址。在IPv6互联网上，这些地址是不可路由的。

尽管在IPv6上进行NAT是可能的，但绝不建议这么做。理由就是有着大得多的IPv6地址（hence, the reason for the much larger IPv6 addresses）。站点本地地址是由`FEC0::/10`前缀、该前缀之后的54位子网ID，以及同样的为本地链路地址所用到的EUI-64格式的接口ID组成。与本地链路地址中设置为0的54位相比，站点本地地址中的54位，被用于构建不同的IPv6前缀（最多2的54次幂个）。下图7.7演示了站点本地地址的格式。

![IPv6站点本地分址](images/0707.png)

*图7.7 -- IPv6站点本地分址*

尽管在本章节中有对IPv6站点本地地址进行说明，同时在思科IOS软件中仍有对其的支持，但要知道**这些地址已被RFC 3879（废弃站点本地地址，Deprecating Site Local Addresses）所废弃**。与此同时，**RFC 4193(唯一本地IPv6单播地址，Unique Local IPv6 Unicast Addresses）又阐述本地唯一地址（Unique-Local addresses, ULAs）**, 本地唯一地址提供了站点本地地址的功能，它们在IPv6全球互联网上也是不可路由的，仅能在某个站点内部路由。

本地唯一地址分配自`FC00::/7`这个IPv6地址块，该地址块又被划分成两个`/8`的地址块，分别作为分配组和随机组（the assigned and random groups）。那么这两组就分别是`FC00::/8`和`FD00::/8`了。`FC00::/8`这个地址块是由一个分配机构（an allocation authority）管理其使用到的/48s，同时`FD00::/8`地址块则是通过在其后追加上随机生成的40位字符串，得到的一个有效`/48`地址块的。

###可聚合全球单播地址

**Aggregatable Global Unicast Addresses**

**可聚合全球单播地址，就是那些用于一般IPv6流量传输、IPv6互联网的IPv6地址了**。这些地址与IPv4中用到的公网地址相似。而从网络分址角度看，每个IPv6全球单播地址都**是由三个主要部分构成的**：自服务商处收到的前缀（48位长）、站点前缀（16位长），以及主机部分（64位长）。这就构成了IPv6中所用到的128位地址了。

如同本模块前面提到的，服务商分配的前缀，是由IPv6服务提供商分配给作为其客户的某家组织的。默认情况下，这些前缀用到`/48`的前缀长度。此外，这些前缀又是从该服务提供商所拥有的IPv6地址空间中分配的（也就是/32前缀长度）。每家服务提供商都将有着其自己的IPv6地址空间，同时由一家服务提供商分配的IPv6前缀，不能在另一家的网络上使用。

而在某个站点内部，管理员此时就能通过用于子网划分的第49到64位，将服务提供商分配的48位前缀，划分成64位的站点前缀，从而可以得到65535个不同的，可在其网络中使用的子网。IPv6地址的主机部分表示该IPv6子网上的某台网络设备或主机。而这又是通过IPv6地址的低64位表示的（this is represented by the low-order 64 bits of the IPv6 address）。

IPv6的可聚合全球单播地址，是由互联网号码分配局（the Internet Assigned Numbers Authority, IANA）分配的，这些地址处于IPv6前缀`2000::/3`中。此前缀允许的可聚合全球单播地址范围是从`2000`到`3FFF`，如下表7.4所示。

*表7.4 -- IPv6可聚合全球单播地址*

<table>
<tr><th>说明</th><th>地址</th><tr>
<tr><td><pre>范围中的第一个地址</pre></td><td><pre>2000:0000:0000:0000:0000:0000:0000:0000</pre></td></tr>
<tr><td><pre>范围中的最后一个地址</pre></td><td><pre>3FFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF</pre></td></tr>
<tr><td><pre>二进制标记</pre></td><td><pre>高位序的三位被设置为001</pre></td></tr>
</table>

在本模块编写时，`2000::/3`IPv6地址块中，仅分配使用了3个子网。这三个子网如下表7.5所示。

*表7.5 -- 由IANA所分配的IPv6可聚合全球单播地址*

<table>
<tr><th>IPv6全球前缀</th><th>二进制表示法</th><th>说明</th></tr>
<tr><td><pre>2001::/16</pre></td><td><pre>0010 0000 0000 0001</pre></td><td>全球IPv6互联网(单播)</td></tr>
<tr><td><pre>2002::/16</pre></td><td><pre>0010 0000 0000 0000</pre></td><td>6to4迁移前缀</td></tr>
<tr><td><pre>3FFE::/16</pre></td><td><pre>0010 1111 1111 1110</pre></td><td>6bone前缀</td></tr>
</table>

>**注意：** 6to4迁移地址和6bone前缀将在本课程的后面说明。

在IPv6全球可聚合单播地址范围，保留了一个叫做**ORCHID**（RFC 4843中定义的覆盖可路由加密散列标识、Overlay Routable Cryptographic Hash Identifiers defined in RFC 4843）特别的实验范围。ORCHID是用于加密散列标识的不可路由IPv6地址。这些地址使用IPv6前缀`2001:10::/28`。关于ORCHID地址的细节，是超出当前CCNA考试要求范围的，本模块或本课程都不会包含。

###多播地址

**Multicast Addresses**

IPv6中用到的多播地址，是从`FF00::/8`这个IPv6前缀中得到的。IPv6中的多播和IPv4中的多播，运行的方式是不一样的。**IPv6中重度使用到IP多播**，并用IP多播替换了一些诸如地址解析协议（Address Resolution Protocol, ARP）这样的IPv4协议。此外，IPv6中还用多播来完成前缀通告及其重编号（prefix advertisements and renumbering）, 以及重复地址侦测（Duplicate Address Detection, DAD）等。本模块后面会对这些概念进行说明。

**IPv6中的多播数据包**，不是通过使用TTL值来将其限制在本地网段上。代之以**使用多播地址内部的范围字段（the Scope filed），定义出其范围**。网段上的IPv6节点，都侦听着多播包，甚至也会发出多播包来交换信息。这样,IPv6网段上所有节点，都知道在其同一网段上所有其它邻居节点了。下图7.8中演示了IPv6网络中用到的多播地址格式。

![IPv6多播分址](images/0708.png)

*图7.8 -- IPv6多播分址*

如同图7.8中所演示的那样，IPv6多播地址格式与其它之前学到的IPv6地址略有不同。IPv6多播地址的前8位表示多播前缀FF::/8。IPv6多播地址的标志字段（the Flag field）用于指明多播地址类型 -- 是永久的还是临时的。

**IPv6永久多播地址是由IANA分配的，而IPv6临时地址则可用于多播预部署的测试**(Permanent IPv6 Multicast addresses are assigned by IANA, while temporary IPv6 Multicast addresses can be used in pre-deployment Multicast testing)。标志字段所包含的值可以是表7.6中所示的两个。

*表7.6 -- IPv6永久及临时多播地址*

<table>
<tr><th><pre>多播地址类型</pre></th><th><pre>二进制表示法</pre></th><th><pre>十六进制值</pre></th></tr>
<tr><td>永久</td><td><pre>0000</pre></td><td>0</td></tr>
<tr><td>临时</td><td><pre>0001</pre></td><td>1</td></tr>
</table>

多播地址中接下来的4位表示**多播范围**。在IPv6多播分址中，该字段是一个**用于限制多播数据包发往网络其它区域的_强制_字段**（this field is a mandatory field that restricts Multicast packets from being sent to other areas in the network）。该字段本质上提供了与IPv4中所用到的TTL字段一样的功能。但是，**在IPv6中，范围的类型有好几种**，下表7.7中列出了这些类型。

*表7.7 -- IPv6多播地址范围的类型*

<table>
<tr><th><pre>范围类型</pre></th><th><pre>二进制表示法</pre></th><th><pre>十六进制值</pre></th></tr>
<tr><td><pre>本地接口，Interface-Local</pre></td><td><pre>0001</pre></td><td>1</td></tr>
<tr><td><pre>本地链路，Link-Local</pre></td><td><pre>0010</pre></td><td>2</td></tr>
<tr><td><pre>本地子网, Subnet-Local</pre></td><td><pre>0011</pre></td><td>3</td></tr>
<tr><td><pre>本地管理域范围，Admin-Local</pre></td><td><pre>0100</pre></td><td>4</td></tr>
<tr><td><pre>本地站点范围，Site-Local</pre></td><td><pre>0101</pre></td><td>5</td></tr>
<tr><td><pre>组织范围，Organization</pre></td><td><pre>1000</pre></td><td>8</td></tr>
<tr><td><pre>全球范围，Global</pre></td><td><pre>1110</pre></td><td>E</td></tr>
</table>

在这些IPv6多播前缀中，又**保留了一些地址**。这些保留地址称作多播指定地址（Multicast Assigned addresses）, 如下表7.8中所示。

*表7.8 -- 保留的IPv6多播地址*

<table>
<tr><th>地址</th><th>范围</th><th>说明</th></tr>
<tr><td><pre>FF01::1</pre></td><td>主机</td><td>所有在本地接口范围内的主机</td></tr>
<tr><td><pre>FF01::2</pre></td><td>主机</td><td>所有在本地接口范围内的路由器</td></tr>
<tr><td><pre>FF02::1</pre></td><td>本地链路</td><td>所有在本地链路范围内的主机</td></tr>
<tr><td><pre>FF02::2</pre></td><td>本地链路</td><td>所有在本地链路范围内的路由器</td></tr>
<tr><td><pre>FF05::2</pre></td><td>站点</td><td>所有在本地站点范围内的路由器</td></tr>
</table>

除了这些地址外，对路由器接口和网络主机上配置的每个单播和任意播地址，都自动启用了一个节点询问多播地址（a Solicited-Node Multicast address）。此地址有着一个本地链路范围，就是说该地址绝不会超出本地网段之外（this address has a Link-Local scope, which means that it will never traverse farther than the local network segment）。**节点询问多播地址用于以下两个目的：取代IPv4的ARP和DAD**。

由于IPv6不会用到ARP，那么节点询问多播地址就被网络主机和路由器用于获悉邻居设备的数据链路地址（the Data Link address）。这样就可以实现IPv6数据包向帧的转换，并将帧发往IPv6主机和路由器了。DAD是IPv6邻居发现协议（Neighbor Discovery Protocol, NDP）的一部分, 在本模块的稍后会详细说明这个协议。DAD就是在设备在采用自动配置方法时，将某个IPv6地址配置为其自己的地址之前，检查该地址是否在本地网段上已被使用的方法。本质上，DAD提供与IPv4中用到的无故ARP（Gratuitous ARP）相似的功能。这些**节点询问多播地址**, 是由IPv6前缀`FF02::1:FF00:0000/104`定义出来的。它们的构成为前缀`FF02::1:FF00:0000/104`, 与单播或任意播地址低位序的24位结合而成。图7.9演示了这些节点询问多播地址的格式。

![IPv6节点询问多播地址](images/0709.png)

*图7.9 -- IPv6节点询问多播地址*

而作为与IPv4到二层以太网的多播映射的一个类似方案，IPv6提供了一种独特的方法，来将三层IPv6多播地址，映射到二层多播地址。IPv6中的多播映射是通过在某多播地址的后32位加上一个16位前缀`33:33`，这个前缀就是IPv6网络中定义的多播以太网前缀（the defined Multicast Ethernet prefix for IPv6 Networks）。其在下图7.10中，演示了所有位于本地接口范围前缀`FF02::2`上的路由器的以太网映射多播地址。

![IPv6多播地址](images/0710.png)

*图7.10 -- IPv6多播地址*

###任意播地址

**Anycast Addresses**

本章节的早前引入了任意播，其可被简单地说成是一对最近的通信（one-to-nearest communication）, 这是因为基于路由协议度量值的那个最近的公共地址（the nearest common address），总是会为本地设备所优先选用。在IPv6中，并没有为任意播特别分配的地址范围，因为任意播地址使用的是全球单播地址、站点本地地址，甚或本地链路地址。尽管如此，仍然保留一个作为特殊用途的任意播地址。该特别地址被称为**子网路由器任意播地址**(the Subnet-Router Anycast address)，是由前面的该子网64位单播前缀，及将后64位全部设置为0（比如`2001:1a2b:1111:d7e5::`）构成的。**任意播地址是绝对不能作为某个IPv6数据包的源地址的**。它们典型地用于诸如移动IPv6(Mobile IPv6）等的协议中，任意播地址的用途，超出CCNA考试范围。

###环回地址

**Loopback Address**

IPv6中的环回地址，用法和IPv4中的一样。与IPv4中用到的环回地址`127.0.0.1`相比，每台设备也都有一个IPv6环回地址，且该地址有设备自身使用。IPv6环回地址用的是前缀`::1`, 用首选地址格式表示为`0000:0000:0000:0000:0000:0000:0000:0001`。也就是说，在环回地址中，除了最后一位总是1外，其它所有位都设置为0。当设备开启IPv6时，总是会自动分配上这些地址，且这些地址绝不会发生变化。

###未指明地址

**Unspecified Addresses**

在IPv6分址里，未指明地址就是那些没有指派到任何接口上的单播地址。这些地址表明设备缺少一个IPv6地址，同时这些地址还用于某些诸如IPv6 DHCP和DAD等的用途。未指明地址是以IPv6地址中的全0值表示的，可以使用前缀`::`进行书写。在首选格式下，这些地址表示为`0000:0000:0000:0000:0000:0000:0000:0000`。

##一些IPv6的协议和机制

**IPv6 Protocols and Mechanisms**

尽管互联网协议版本6与版本4是相似的，但在具体运作上，前者与后者相比仍然有着显著的不同。本节对以下的一些IPv6协议和机制进行了说明。

+ IPv6的ICMP
+ IPv6邻居发现协议（the IPv6 Neighbor Discovery Protocol, NDP）
+ IPv6的有状态自动配置机制（IPv6 stateful autoconfiguration）
+ IPv6的无状态自动配置机制（IPv6 stateless autoconfiguration）

###IPv6下的ICMP

**ICMP for IPv6**

ICMP用于将有关发往预期目的主机的IP数据的错误和其他信息，汇报给源主机。在RFC 2463中，作为58号协议定义的ICMPv6，支持ICMPv4的各种报文，还包含了ICMPv6的一些额外报文。**ICMPv6作为一个如同TCP一样的，较高级别的协议，意味着在IPv6数据包中，ICMPv6是放在所有尽可能的扩展头部之后的**。下图7.11演示了ICMPv6数据包中所包含的字段。

![ICMPv6数据包头部](images/0711.png)

*图7.11 -- ICMPv6数据包头部*

在ICMPv6数据包头部，其8位类型字段（the 8-bit Type field）用于表明或区分ICMPv6报文的类型。该字段用于提供错误报文和信息性报文。表7.9列出并说明了一些可在此字段发现的常见值。

*表7.9 -- ICMPv6报文类型*

<table>
<tr><th>ICMPv6 类型</th><th>说明</th></tr>
<tr><td>1</td><td>目的主机不可达</td></tr>
<tr><td>2</td><td>数据包太大</td></tr>
<tr><td>3</td><td>发生了超时</td></tr>
<tr><td>128</td><td>Echo请求</td></tr>
<tr><td>129</td><td>Echo回应</td></tr>
</table>

> **注意：** ICMPv4也是使用的这些报文类型。

紧接着类型字段的8位代码字段（the 8-bit Code field），提供了有关发出报文的细节。表7.10演示了该字段的常用值，也是ICMPv4所共用的。

*表7.10 -- ICMPv6代码*

<table>
<tr><th>ICMPv6代码</th><th>说明</th></tr>
<tr><td>0</td><td>Echo回应</td></tr>
<tr><td>3</td><td>目的主机不可达</td></tr>
<tr><td>8</td><td>Echo</td></tr>
<tr><td>11</td><td>发生了超时</td></tr>
</table>

在代码字段后面的16位校验和字段（the 16-bit Checksum field），包含的是一个用于检测ICMPv6中数据错误的运算值。ICMPv6数据包的最后，就是报文或数据二选一的字段（the Message or Data field is an optional）, 它是一个可变长度字段，包含了由类型及代码字段指明的报文类型特定数据。在用到报文或数据字段时，该字段提供了发送给目的主机的信息。

**ICMPv6是IPv6的一个核心部件**。在IPv6中，ICMPv6有以下用途。

+ 重复地址检测，Duplicate Address Detection, DAD
+ ARP的替代，the replacement of ARP
+ IPv6无状态自动配置, IPv6 stateless autoconfiguration
+ IPv6前缀重新编号, IPv6 prefix renumbering
+ 路径MTU发现，Path MTU Discovery, PMTUD

> **注意：** 在上述用途中，DAD和无状态自动配置会在本章的稍后进行说明。PMTUD是超出当前CCNA考试要求范围的，在本模块及本教程中不会对其进行任何细节上的说明。

**IPv6邻居发现协议**

**The IPv6 Neighbor Discovery Protocol, NDP**

**IPv6邻居发现协议带来IPv6的即插即用特性**。它是在RFC 2461中定义的，是IPv6的一个必不可少的组成部分。**NDP运行在链路层**，负责**发现链路上的其它节点**、**确定其它节点的链路层地址**、**发现可用的路由器**，以及**维护有关到其它邻居节点路径的可达性信息**。NDP实现了IPv6的**类似于IPv4的ARP**（这正是其取代的功能）、**ICMP路由器发现**(ICMP Router Discovery)以及**路由器重定向协议（Router Redirect Protocols）等**功能。尽管如此，要记住NDP提供了比起IPv4中用到的诸多机制，都更为了不起的功能。在与ICMPv6配合使用时，NDP可以完成以下任务。

+ 动态邻居和路由器发现，dynamic neighbor and router discovery
+ 取代ARP，the replacement of ARP
+ IPv6无状态自动配置，IPv6 stateless autoreconfiguration
+ 路由器重定向，router redirection
+ 主机参数发现，host parameter discovery
+ IPv6地址解析，IPv6 address resolution
+ 确定下一跳路由器，next-hop router determination
+ 邻居不可达检测，Neighbor Unreachablitiy Detection, NUD
+ 重复地址检测，Duplicate Address Detection, DAD

> **注意：** 并不要求对上面列出的每个优势进行细节上的探究。

邻居发现协议又定义了五种ICMPv6数据包类型，在下表7.11中有列出和说明。

*表7.11 -- ICMPv6邻居发现报文类型*

<table>
<tr><th>ICMPv6类型</th><th>说明</th></tr>
<tr><td>133</td><td>用于路由器询问报文，used for Router Solicitation(RS) messages</td></tr>
<tr><td>134</td><td>用于路由器通告报文，used for Router Advertisement(RA) messages</td></tr>
<tr><td>135</td><td>用于邻居询问报文，used for Neighbor Solicitation(NS) messages</td></tr>
<tr><td>136</td><td>用于邻居通告报文，used for Neighbor Advertisement(NA) messages</td></tr>
<tr><td>137</td><td>用于路由器重定向报文, used for Router Redirect messages</td></tr>
</table>

**路由器询问报文**是在主机的接口开启IPv6时，由主机所发出的。这些报文用于请求位于本地网段上的路由器立即生成RA报文，而不要等到下一个计划的RA时间间隔才生成RA报文。下图7.2演示了一条在线路上捕获到的RS报文。

![IPv6路由器询问报文](images/0712.png)

*图7.12 -- IPv6路由器询问报文*

路由器收到该RS报文后，便使用RA报文通告其存在，RA报文通常包含了本地链路的前缀信息，及所有诸如建议的跳数限制等额外配置。RA中包含的信息在下图7.13中进行了演示。

![IPv6路由器通告报文](images/0713.png)

*图7.13 -- IPv6路由器通告报文*

这里重申一点，**RS和RA报文，都是路由器到主机(route-to-host)或主机到路由器(host-to-router)的信息交换**, 如下图所示。

![IPv6的RS和RA报文](images/0714.png)

*图7.14 -- IPv6的RS和RA报文*

IPv6的NS报文，则是位处本地网段上的IPv6路由器, 所发出的多播报文，用于确定某个邻居的数据链路地址，或是用于检查某个邻居是否仍然可达（因而NS报文取代的是ARP的功能）。这些报文也用于重复地址检测(DAD)目的。尽管对NS报文的深入探究超出了CCNA考试要求的范围，下面的图7.15仍然演示了一个在线路上捕获到的IPv6邻居询问报文数据包。

![IPv6邻居询问报文](images/0715.png)

*图7.15 -- IPv6邻居询问报文*

而邻居通告报文（Neighbor Advertisement messages）通常也是由位处本地网段上的路由器发出的，用于对收到的NS报文进行回应。此外，**在一个IPv6前缀改变时，路由器也会发出一条无询问的NS报文**，以此来告知本地网络网段上的其它设备，发生了这个变化。在NA报文上，对NA报文中的格式或包含的字段的细节探究，也是超出CCNA考试要求范围之外的。图7.16和图7.17演示了一条在线路上捕获的邻居通告报文，**邻居通告报文也是通过IPv6多播发出的**。

![IPv6邻居通告报文](images/0716.png)

*图7.16 -- IPv6邻居通告报文*

![IPv6邻居通告报文](images/0717.png)

*图7.17 -- IPv6邻居通告报文*

最后，路由器重定向（router redirect）使用的是报文类型为137的ICMPv6重定向报文（ICMPv6 Redirect messages），路由器重定向用于告知网络主机，网络上存在一台路由器，该路由器有着前往预计目的主机的更优路径。ICMPv6的路由器重定向与ICMPv4的工作方式一样，而ICMPv4的路由器重定向就是用来对当前IPv4网络中的流量进行重定向的。

###IPv6的有状态自动配置

**IPv6 Stateful Autoconfiguration**

如同本模块先前指出的那样，有状态自动配置允许网络主机从某台网络服务器（比如通过DHCP）上收到其地址信息。IPv4和IPv6都支持这种方式。在IPv6网络中，使用DHCPv6来为IPv6主机提供有状态（及无状态）自动配置服务。**在IPv6的部署中，当某台IPv6主机收到来自本地网段上的路由器RA报文后，该主机就会检查这些数据包，以判定是否可以使用DHCPv6**。RA报文通过将那些M（受管理的，Managed）或O（其它方式，Other）位设置为1的方式，提供是否可以使用DHCPv6的信息。

在DHCP下，客户端设定为从DHCP服务器取得有关信息。而在DHCPv6下，客户端却不并知道从哪里得到这些信息，因为既可以从SLAAC，也可以从有状态的DHCPv6, 抑或从结合了SLAAC及DHCPv6两种的方式取得。

路由器通告报文中的M位，指的是受管理的地址配置标志位（the Managed Address Configuration Flag bit）。在此位设置了时（也就是说该位的值为1时）， 它指示IPv6主机要取得一个由DHCPv6服务器所提供有状态的地址，并忽略之后的O位。而路由器通告报文中的O位，指的是其它有状态配置标志位（the Other Stateful Configuration Flag bit）。当该位设置了（也就是说该位的值为1）后，指示IPv6主机要使用DHCPv6取得更多的配置设置项，比如DNS及WINS服务器等。

如某台主机未曾配置一个IPv6地址，它就可以采用下面的三种方法之一，来获得一个IPv6地址，及诸如DNS服务器地址等的其他网络设置。

+ SLAAC -- 无状态自动配置（Stateless Autoconfiguration） 。M和O位设置为0, 也就是没有DHCPv6信息。主机从一条RA收到所有必要信息。
+ 有状态DHCPv6 -- M标志位设置为1, 告诉主机使用DHCPv6取得所有地址和网络信息。
+ 无状态DHCPv6 -- M标志位设置为0, O标志位设置为1, 意味着主机将采用SLAAC来得到地址（从一条RA），而同时从DNS服务器处取得其它信息。

尽管无状态自动配置能力是IPv6的一项优势，有状态自动配置仍然有着许多好处，包括以下这些。

+ 相较无状态自动配置所提供的那些项目，有状态自动配置有着更大的控制权
+ 在无状态自动配置可用的网络上，同样可以使用有状态自动配置
+ 在缺少路由器的情形下，仍然可以为网络主机提供分址
+ 通过分配新的前缀给主机，而用来对网络重新编号
+ 可用于将全部子网发布给用户侧设备（can be used to issue entire subnets to customer premise equipment）

###IPv6无状态自动配置

**IPv6 Stateless Autoconfiguration**

IPv6容许设备为自己配置一个IP地址，以便进行主机到主机的通信。有状态自动配置需要一台服务器来分配地址信息，对于IPv6来说，就要用到DHCPv6。有状态就是说，信息交换的细节在服务器（或路由器）上是有保存的，那么无状态就说的是没有服务器来保存这些细节了。DHCPv6既可以是有状态的，也可以是无状态的。

在IPv6中，无状态自动配置，允许主机依据自本地网络网段上的路由器发出的前缀通告，来自己配置它们的单播IPv6地址。所需的其它信息（比如DNS服务器的地址等）可从DHCPv6服务器获取。IPv6中无状态自动配置可以用到的三种机制，如下所示。

+ 前缀通告，prefix advertisement
+ 重复地址检测，DAD
+ 前缀重编号，prefix renumbering

IPv6地址前缀通告用到了ICMPv6路由器通告报文，而ICMPv6 RA是发往链路上的所有主机（all-hosts-on-the-local-link）的，带有多播地址`FF02::1`的ICMPv6数据包。根据IPv6的设计，仅有路由器才被允许在本地链路上通告前缀。在采行了无状态自动配置后，就务必要记住，所用到的前缀长度，必须是64位（比如`2001:1a2b::/64`）。

在前缀配置之后，IPv6无状态自动配置用到的RA报文还包含了以下信息。

+ IPv6前缀，the IPv6 prefix
+ 生命期，the lifetime
+ 默认路由器信息，default router information
+ 标志和/或选项字段，Flags and/or Options fields

就像刚才指出的那样，**IPv6前缀必须是64位**。此外，**在本地网段上还可以通告多个的IPv6前缀**。在该网络网段上的主机收到IPv6前缀后，就将它们的MAC地址以EUI-64格式，追加到前缀后面，从而自动地配置上他们的IPv6单播地址这在本模块的先前部分已有说明。这样就为该网段上的每台主机，都提供了一个唯一的128位IPv6地址。

同样提供了每个通告的前缀的生命期数值给这些节点，生命期字段可以是从0到无穷的值。在节点收到前缀后，就对生命期值进行检查，从而在生命期数值到0时停用该前缀。此外，如收到的某个特定前缀的生命期值为无穷，网络主机就绝不会停用那个前缀。又每个通告的前缀又带有两个生命期值：**有效生命期值**及**首选生命期值**。

有效生命期值用于确定出该主机地址将保持多长时间的有效期。在该值超时后（也就是说到值为0时），主机地址就成为无效地址。而首选生命期值则用于确定经由无状态自动配置的某个地址将保持多长时间的有效期。此值必须小于或等于在有效生命期值，同时该值通常用于前缀的重编号。

默认路由器提供了它自己的IPv6地址的存在情况和生命期。默认情况下，用于默认路由器的那个地址是本地链路地址（`FE80::/10`）。这样做就可以在全球单播地址发生改变时，也不会像在IPv4中那样，某个网络被重新编号时，导致网络服务中断。

最后，一些标志和选项字段可被用作指示网络主机采行无状态自动配置或有状态自动配置。这些字段在图7.13中的RA线路捕获中有包含。

重复地址检测（DAD）是一种用在无状态自动配置中，在某网段上的主机启动时，用到的NDP机制。DAD要求某台网络主机启动期间，在永久地配置它自己的IPv6地址之前，先要确保没有别的网络主机已经使用了它打算使用的那个地址。

DAD通过使用邻居询问（135类型的ICMPv6）及节点询问多播地址（Solicited-Node Multicast addresses），来完成这个验证。主机使用一个未指明IPv6地址（an unspecified IPv6 address, 也就是地址`::`）作为报文数据包的源地址，并将其打算使用的那个IPv6单播地址，作为目的地址，在本地网段上发送一个邻居询问ICMPv6报文数据包。如有其它主机使用着该地址，那么主机就不会自动将此地址配置为自己的地址；而如没有其他设备使用这个地址，则该主机就自动配置并开始使用这个IPv6地址了。

最后，前缀重编号（prefix renumbering）允许在IPv6中，当网络从一个前缀变为另一个时，进行前缀的透明重编号。与IPv4中同样的全球IP地址可由多个服务提供商进行通告不同，IPv6地址空间的严格聚合阻止了服务提供商对不属于其组织的前缀进行通告（Unlike in IPv4, where the same global IP address can be advertised by multiple providers, the strict aggregation of the IPv6 address space prevents providers from advertising prefixes that do not belong to their organization）。

在网络发生从一家IPv6服务提供商迁移至另一家时，IPv6前缀重编号机制，就提供了一种自一个前缀往另一前缀平滑和透明的过渡。前缀重编号使用与在前缀通告中同样的ICMPv6报文和多播地址。而前缀重编号可经由运用RA报文中包含的时间参数完成。

在思科IOS软件中，路由器可配置通告带有被减少到接近0的有效和首选生命期的当前前缀，这就令到这些前缀能够更快地成为无效前缀。此时再将这些路由器配置为在本地网段上通告心的前缀。这样做会允许旧的前缀和新的前缀在同样的网段上并存。

迁移期间，本地网段上的主机用着两个单播地址：一个来自旧的前缀，一个来自新的前缀。那些使用旧前缀的当前连接仍被处理着；但所有自主机发出的新的连接，都是使用新的前缀的。在旧前缀超时后，就只使用新的前缀了。

###配置无状态DHCPv6

**Configuring Stateless DHCPv6**

为在某台路由器上配置无状态的DHCPv6, 需要完成一些简单的步骤。

+ 创建地址池名称和其它参数, create the pool name and other parameters
+ 在某个借口上开启它, enable it on an interface
+ 修改RA设置，modify Router Advertisement settings

一个身份关联是分配给客户端的一些地址（an Identity Association is a collection of addresses assigned to the client）。使用到DHCPv6的每个借口都必须要有至少一个的身份关联（IA）。这里不会有CCNA考试的配置示例。

###在思科IOS软件中开启IPv6路由

现在，你对IPv6基础知识有了扎实掌握，本模块剩下的部分将会专注于思科IOS软件中IPv6的配置了。默认下，思科IOS软件中的IPv6路由功能是关闭的。那么就必须通过使用__`ipv6 unicast-routing`这个全局配置命令__来开启IPv6路由功能。

在全局开启IPv6路由之后，接口配置命令`ipv6 address [ipv6-address/prefix-length | prefix-name sub-bits/prefix-length | anycast | autoconfig <default> | dhcp | eui-64 | link-local]`就可以用于配置接口的IPv6分址了。关键字`[ipv6-address/prefix-length]`用于指定分配给该接口的IPv6前缀和前缀长度。下面的配置演示了如何为一个路由器接口配置子网`3FFF:1234:ABCD:5678::/64`上的第一个地址。

```
R1(config)#ipv6 unicast-routing
R1(config)#interface FastEthernet0/0
R1(config-if)#ipv6 address 3FFF:1234:ABCD:5678::/64
R1(config-if)#exit
```

按照此配置，`show ipv6 interface [name]`命令就可用于验证配置的IPv6地址子网（即`3FFF:1234:ABCD:5678::/64`）, 如下面的输出所示。

```
R1#show ipv6 interface FastEthernet0/0
FastEthernet0/0 is up, line protocol is up
	IPv6 is enabled, link-local address is FE80::20C:CEFF:FEA7:F3A0
	Global unicast address(es):
		3FFF:1234:ABCD:5678::1, subnet is 3FFF:1234:ABCD:5678::/64
	Joined group address(es):
		FF02::1
		FF02::2
		FF02::1:FF00:1
		FF02::1:FFA7:F3A0
...
[Truncated Output]
```

就如在本模块早先指出的那样，IPv6允许在同一接口上配置多个前缀。而如过在同一借口上配置了多个前缀，`show ipv6 interface [name] prefix`命令，就可以用来查看所有分配的前缀，以及它们各自的有效和首选生命期数值。下面的输出显示了在一个配置了多个IPv6前缀的路由器接口上，该命令所打印出的信息。

<pre>
R1#show ipv6 interface FastEthernet0/0 prefix
<b>IPv6 Prefix Advertisements FastEthernet0/0</b>
Codes:	A - Address, P - Prefix-Advertisement, O - Pool
		U - Per-user prefix, D - Default
		N - Not advertised, C - Calendar
	default [LA] Valid lifetime 2592000, preferred lifetime 604800
AD	<b>3FFF:1234:ABCD:3456::/64</b> [LA] Valid lifetime 2592000, preferred lifetime 604800
AD	<b>3FFF:1234:ABCD:5678::/64</b> [LA] Valid lifetime 2592000, preferred lifetime 604800
AD	<b>3FFF:1234:ABCD:7890::/64</b> [LA] Valid lifetime 2592000, preferred lifetime 604800
AD	<b>3FFF:1234:ABCD:9012::/64</b> [LA] Valid lifetime 2592000, preferred lifetime 604800
</pre>

> **注意：** 和早前指出的一样，有效和首选生命期数值可自默认值进行修改，以实现在应用前缀重编号时的平滑过渡。但此配置是超出CCNA范围的，所以本教程不会对其进行演示。

跟着接口配置命令`ipv6 prefix`的使用之后，关键字`[prefix-name sub-bits/prefix-length]`用于配置一个通用前缀（a general prefix），通用前缀指定要配置到该接口上的子网的那些前导位。这个配置也是超出当前CCNA考试要求的，本模块不会对其进行演示。

关键字`[anycast]`用于配置一个IPv6任意播地址。和先前指出的那样，任意播分址允许将同一个公共地址（the same common address）分配到多个路由器接口。主机使用从路由协议度量值上看离它们最近的任意播地址。任意播配置超出CCNA考试要求范围，不会在本模块进行演示。

`[autoconfig <default>]`关键字开启无状态自动配置（SLAAC）。如用到该关键字，路由器将动态学习链路上的前缀，之后将EUI-64地址加到所有学习到的前缀上。`[default]`关键字是一个允许安装一条默认路由的可选关键字（the `<default>` keyword is an optional keyword that allows a default route to be installed）。下面的配置样例，演示了如何在某个路由器接口上开启无状态自动配置，同时额外地允许安装上默认路由。

```
R2(config)#ipv6 unicast-routing
R2(config)#interface FastEthernet0/0
R2(config-if)#ipv6 address autoconfig default
R2(config-if)#exit
```

按照这个配置，路由器R2将会监听FastEthernet0/0接口所在本地网段上的RA报文。该路由器将会对每个学习到的前缀，动态地配置一个EUI-64地址，并接着安装上指向该RA通告路由器本地链路地址的默认路由。使用`show ipv6 interface [name]`命令，即可对动态地址配置进行验证，如下面的输出所示。

<pre>
R2#show ipv6 interface FastEthernet0/0
FastEthernet0/0 is up, line protocol is up
	IPv6 is enabled, link-local address is FE80::213:19FF:FE86:A20
	Global unicast address(es):
		<b>3FFF:1234:ABCD:3456:213:19FF:FE86:A20, subnet is 3FFF:1234:ABCD:3456::/64 [PRE]</b>
			valid lifetime 2591967 preferred lifetime 604767
		<b>3FFF:1234:ABCD:5678:213:19FF:FE86:A20, subnet is 3FFF:1234:ABCD:5678::/64 [PRE]</b>
			valid lifetime 2591967 preferred lifetime 604767
		<b>3FFF:1234:ABCD:7890:213:19FF:FE86:A20, subnet is 3FFF:1234:ABCD:7890::/64 [PRE]</b>
			valid lifetime 2591967 preferred lifetime 604767
		<b>3FFF:1234:ABCD:9012:213:19FF:FE86:A20, subnet is 3FFF:1234:ABCD:9012::/64 [PRE]</b>
			valid lifetime 2591967 preferred lifetime 604767
		<b>FEC0:1111:1111:E000:213:19FF:FE86:A20, subnet is FEC0:1111:1111:E000::/64 [PRE]</b>
			valid lifetime 2591967 preferred lifetime 604767
	  Joined group address(es):
		FF02::1
		FF02::2
		FF02::1:FF86:A20
	  MTU is 1500 bytes
...
[Truncated Output]
</pre>

在上面的输出中，注意到尽管接口上没有配置显式的IPv6地址，还是动态地为该路由器经由侦听RA报文所发现的子网，配置了一个EUI-64地址。每个这些前缀的计时器，都继承自通告RA报文的那台路由器。为了进一步验证无状态自动配置，可以使用`show ipv6 route`命令，来验证到首选通告路由器本地链路地址的默认路由，如下面所演示的那样。

<pre>
R2#show ipv6 route ::/0
IPv6 Routing Table - 13 entries
Codes:	C - Connected, L - Local, S - Static, R - RIP, B - BGP
		U - Per-user Static route
		I1 - ISIS L1, I2 - ISIS L2, IA - ISIS inter area, IS - ISIS summary
		O - OSPF intra, OI - OSPF inter, OE1 - OSPF ext 1, OE2 - OSPF ext 2
		ON1 - OSPF NSSA ext 1, ON2 - OSPF NSSA ext 2
<b>S	::/0 [1/0]
	via FE80::20C:CEFF:FEA7:F3A0, FastEthernet0/0</b>
</pre>

在命令`ipv6 address`之后，关键字`[dhcp]`用于配置该路由器接口使用有状态自动配置（也就是DHPCv6），来请求该接口的分址配置。在此配置下，有着一个额外的关键字，`[rapid-commit]`, 同样可以追加到此命令之后，以允许地址分配及其它配置信息的二报文交换快速方式（the two-message exchange method）。

再回到讨论主题，在`ipv6 address`命令下，关键字`[eui-64]`用于为某个接口配置一个IPv6地址，并在地址的低64位使用一个EUI-64地址而在该接口上开启IPv6处理。默认情况下，**本地链路、站点本地以及IPv6无状态自动配置都用到EUI-64格式来构造其各自的IPv6地址**。EUI-64分址**将48位MAC地址扩展到一个64位地址**。通过两步实现该扩展，这两步将在下一段进行说明。该过程就叫作无状态自动配置（stateless autoconfiguration），或SLAAC。

构造EUI-64地址的第一步，将值`FFEE`插入到MAC地址的中间，就将12个十六进制字符的48位MAC地址扩展到16个十六进制字符的64位了。下图7.18演示了48位MAC地址到64位EUI地址的转换。

![创建EUI-64地址](images/0718.png)

*图7.18 -- 创建EUI-64地址*

EUI-64分址的下一步涉及64位地址的第七位的设置。此第七位用于区分该MAC地址是否是唯一的。如该位设置为1, 就表明该MAC地址是一个全球受管理MAC地址（a globally managed MAC address）-- 也就是说该MAC地址是有某厂商分配的。如该位设置为0, 就表明该MAC地址是本地分配的--就意味着该MAC地址有可能是由管理员添加的。为更进一步搞清楚此声明，MAC地址实例`02:1F:3C:59:D6:3B`就被认为是一个全球分配的MAC地址（a globally-assigned MAC address）, 而MAC地址`00:1F:3C:59:D6:3B`则被看作是一个本地地址。下图7.19有演示。

![确定本地及全球MAC地址](images/0719.png)

*图7.19 -- 确定本地及全球MAC地址*

按照这样的配置，命令`show ipv6 interface`就可用于验证验证分配到接口FastEthernet0/0上的IPv6接口ID， 如下面的输出所示。

<pre>
R2#show ipv6 interface FastEthernet0/0
FastEthernet0/0 is up, line protocol is up
	IPv6 is enabled, link-local address is FE80::213:19FF:FE86:A20
	<b>Global unicast address(es):
	  	3FFF:1A2B:3C4D:5E6F:213:19FF:FE86:A20, subnet is 3FFF:1A2B:3C4D:5E6F::/64 [EUI]</b>
	Joined group address(es):
		FF02::1
		FF02::2
	FF02::1:FF86:A20
	MTU is 1500 bytes
...
[Truncated Output]
</pre>

要验证该EUI-64地址的构造过程，同样可以通过使用`show interface`命令，查看指定接口的MAC地址的方式，来检查该完整的IPv6地址。

<pre>
R2#show interface FastEthernet0/0
FastEthernet0/0 is up, line protocol is up
	<b>Hardware is AmdFE, address is 0013.1986.0a20 (bia 0013.1986.0a20)</b>
		Internet address is 10.0.1.1/30
</pre>

从上面的输出可以看出，该EUI-64地址实际上是有效的，且是基于该接口的MAC地址。此外，该地址是全球地址，因为那个第七位是开启的（也就是改为包含的是一个非零值）。

最后的`[link-local]`关键字用于分配给接口一个本地链路地址。一定要记住在默认情况下，对于动态地创建出一个本地链路地址来说，接口上并不是非得要启用一个IPv6前缀。而是当在某个接口下执行了接口配置命令`ipv6 enable`时，就会以EUI-64分址方式，自动创建出那个接口的一个本地链路地址。

而如果要手动配置一个本地链路地址，就必须分配一个本地链路地址块`FE80::/10`中的地址。下面的配置实例，演示了如何在某接口上配置一个本地链路地址。

```
R3(config)#interface FastEthernet0/0
R3(config-if)#ipv6 address fe80:1234:abcd:1::3 link-local
R3(config-if)#exit
```

按照该配置，就可用`show ipv6 interface [name]`命令验证这个手动配置的本地链路地址，如下面的输出所示。

```
R3#show ipv6 interface FastEthernet0/0
FastEthernet0/0 is up, line protocol is up
	IPv6 is enabled, link-local address is FE80:1234:ABCD:1::3
	Global unicast address(es):
		2001::1, subnet is 2001::/64
	Joined group address(es):
		FF02::1
		FF02::2
		FF02::1:FF00:1
		FF02::1:FF00:1111
	MTU is 1500 bytes
...
[Truncated Output]
```

> **注意：** 在进行手动配置本地链路地址时，如思科IOS软件侦测到另一主机正在使用一个它的IPv6地址，控制台上就会打印出一条错误消息，同时该命令将被拒绝。所以在手动配置本地链路地址时，要小心仔细。

###IPv6子网划分

**Subnetting with IPv6**

如你已经学到的，IPv6地址分配给机构的是一个前缀。而IPv6地址的主机部分总是64位的EUI-64, 同时标准的前缀通常又是48位或/48。那么剩下的16位，就可由网络管理员用来自主用于子网划分了。

在考虑网络分址时，因为同样的规则对IPv4和IPv6都是适用的，那就是每个网段只能有一个网络。不能分离地址而将一部分主机位用在这个网络，另一部分主机位用在其它网络。

如你看着下面图表中的分址，就能更清楚这个情况。

<table>
<tr><th>全球路由前缀</th><th>子网ID</th><th>接口ID</th></tr>
<tr><td>48位或/48</td><td>16位（65535个可能的子网）</td><td>64位</td></tr>
</table>

绝不用担心会用完每个子网的主机位，因为每个子网有超过2的64次幂的主机。任何组织要用完这些子网都是不大可能的，而就算发生了这种情况，也可以轻易地从ISP那里要一个前缀。

比如我们说分得了全球路由前缀（the global routing prefix）`0:123:abc/48`。该地址占用了一个完整IPv6地址的三个区段，而每个区段或4位字节（quartet）则是16位，那么到目前为止就用了48位。主机部分则需要64位，留下16位用于子网的分配。

可以简单的从零（子网零也是合法的）开始以十六进制数下去。对于主机来说，也可以这样做，除非想要将头几个地址留给网段上的服务器，比如说。

用一个更简单的前缀来打比方吧 -- `2001:123:abc/48`。第一个子网就是全零，当然，每个子网上的第一台主机也可以是全零，这也是合法的（只要不保留IPv6中的全0s和全1s地址）。又会将全零主机表示为缩写形式的`::`。那么这里就有开头的几个子网及主机地址。

<table>
<tr><th>全球前缀</th><th>子网</th><th>第一个地址</th></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>0000</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>0001</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>0002</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>0003</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>0004</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>0005</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>0006</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>0007</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>0008</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>0009</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>000A</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>000B</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>000C</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>000D</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>000E</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>000F</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>0010</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>0011</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>0012</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>0013</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>0014</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>0015</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>0016</pre></td><td><pre>::</pre></td></tr>
<tr><td><pre>2001:123:abc</pre></td><td><pre>0017</pre></td><td><pre>::</pre></td></tr>
</table>

我肯定你已经注意到这与IPv4分址规则有所不同，不同之处就在与**可以使用全零子网，同时子网的第一个地址总是全零**。请看看下面这个简单的网络拓扑，可以照这种方式进行子网分配。

![IPv6子网分配](images/0720.png)

*图7.20 -- IPv6子网分配*

就是那么容易吗？如回忆一下IPv4子网划分章节，要完成子网划分，以及算出有多少主机多少子网并记住要排除一些地址，简直就是一场噩梦。**IPv6子网划分就容易得多**。你分配到的不一定是一个48位前缀，可能是一个用于家庭网络的`/56`或更小的前缀，但原则是一样的。也可以自位界限以外进行子网划分，但这是很少见的，且如果思科要你用考试中的很段时间完成那么深的细节，也是不公平的（You can also subnet off the bit boundary, but this would be most unusual and unfair of Cisco to expect you to go into that amount of detail in the short amount of time you have in the exam）。还好的是，考试不是要你考不过，但谁又知道呢（Hopefully, the exam won't be a mean attempt to catch you out, but you never know）。为以防万一，这里给出一个有着`/56`前缀长度的地址示例。

`2001:123:abc:8bbc:1221:cc32:8bcc:4231/56`

该前缀是56位，转换一下就是14个十六进制数位（14x4=56）, 那么就知道了该前缀将带到一个4位字节（quartet）的中间。**这里有个坑**。在前缀终止前，必须要将该4位字节的第3和4位置为零。

<pre>2001:123:abc:<b>8b00</b>:0000:0000:0000:0000/56</pre>

上面对位界限分离的地方进行了加粗（I've blod the quartet where the bit boundary is broken）。在匆忙中及考试中时间上的压力下，可能会完全忘记这重要的一步。请记住也要将下面这个地址（第一个子网上的第一台主机）写作这样。

<pre>2001:123:abc:<b>8b00</b>::/56</pre>

如他们硬要在考试中把你赶出去，就可能会试着让你把那两个零从位界限分离处之前的4位字节中去掉（If they do try to catch you out in the exam, it would probably be an attempt to have you remove the trailing zeros from the quartet before the bit boundary is broken）。

<pre>2001:123:abc:<b>8b</b>::/56</pre>

那么上面这个缩写就是非法的了。

也可以从主机部分借用位来用于子网划分，但绝没有理由这么做，同时这么做也会破坏采行发明IPv6而带来的可资利用的那些众多特性的能力，包括无状态自动配置（You can steal bits from the host portion to use for subnets, but there should never be a reason to and it would break the ability to use many of the features IPv6 was invented to utilise, including stateless autoconfiguration）。

##IPv6和IPv4的比较

**IPv6 Compared to IPv4**

一名网络工程师应有一幅IPv6比起IPv4所带来众多优势的图景。看着IPv6的增强，可以总结出下面这些优势。

+ IPv6有着一个扩展的地址空间，从32位扩展到了128位, IPv6 has an expanded address space, from 32 bits to 128bits
+ IPv6使用十六进制表示法，而不是IPv4中的点分十进制表示法, IPv6 uses hexadecimal notation instead of dotted-decimal notation(as in IPv4)
+ 因为采用了扩展的地址空间，IPv6地址是全球唯一地址，从而消除了NAT的使用需求, IPv6 addresses are globally unique due to the extended address space, eliminating the need for NAT
+ IPv6有着一个固定的头部长度（40字节），允许厂商在交换效率上进行提升, IPv6 has a fixed header length(40 bytes), allowing vendors to improve switching efficiency
+ IPv6通过在IPv6头部和传输层之间放入扩展头部，而实现对一些增强选项（这可以提供新特性）的支持, IPv6 supports enhanced options(that offer new features)by placing extension headers between the IPv6 header and the Transport Layer header
+ IPv6具备地址自动配置的能力，提供无需DHCP服务器的IP地址动态分配, IPv6 offers address autoconfiguration, providing for dynamic assignment of IP addresses even without a DHCP server
+ IPv6具备对流量打标签的支持, IPv6 offers support for labeling traffic flows
+ IPv6有着内建的安全功能，包括经由IPSec实现的认证和隐私保护功能等, IPv6 has security capabilities built in, including authentication and privacy via IPSec
+ IPv6具备在往目的主机发送数据包之前的路径MTU发现功能，从而消除碎片的需求, IPv6 offers MTU path discovery before sending packets to a destination, eliminating the need for fragmentation
+ IPv6支持站点多处分布，IPv6 supports site multi-homing
+ IPv6使用ND（邻居发现，Neighbor Discovery）协议取代ARP，IPv6 uses the ND protocol instead of ARP
+ IPv6使用AAAA DNS记录，取代IPv4中的A记录, IPv6 uses AAA DNS records instead of A records (as in IPv4)
+ IPv6使用站点本地分址，取代IPv4中的RFC 1918， IPv6 uses Site-Local addressing instead of RFC 1918(as in IPv4)
+ IPv4和IPv6使用不同的路由协议, IPv4 and IPv6 use different routing protocols
+ IPv6提供了任意播分址, IPv6 provides for Anycast addressing

##第七天的问题

1. IPv6 addresses must always be used with a subnet mask. True or false?
2. Name the three types of IPv6 addresses.
3. Which command enables IPv6 on your router?
4. The `0002` portion of an IPv6 address can be shortened to just 2. True or false?
5. How large is the IPv6 address space?
6. With IPv6, every host in the world can have a unique address. True or false?
7. IPv6 does not have natively integrated security features. True or false?
8. IPv6 implementations allow hosts to have multiple addresses assigned. True or false?
9. How can the broadcast functionality be simulated in an IPv6 environment?
10. How many times can the double colon (`::`) notation appear in an IPv6 address?

##第七天问题答案

1. False.
2. Unicast, Multicast, and Anycast.
3. The `ipv6 unicast-routing`
4. True.
5. 128 bits.
6. True.
7. False.
8. True.
9. By using Anycast.
10. One time.

##第七天实验

###IPv6概念实验

**IPv6 概念实验**

在一对直接连接的思科路由器上，对在本模块中提到的IPv6概念和命令，进行测试。

+ 在两台路由器上都开启IPv6全球单播路由
+ 在每个连接的接口上手动配置一个IPv6地址，比如下面这样。
	- 在路由器R1的连接接口上配置`2001:100::1/64`
	- 在路由器R2的连接接口上配置`2001:100::2/64`
+ 使用命令`show ipv6 interface`和`show ipv6 interface prefix`对配置进行验证
+ 测试直接ping的连通性
+ 使用IPv6无状态自动配置（`ipv6 address autoconfig default`）进行重新测试
+ 使用EUI-64地址（IPv6地址`2001::/64` EUI-64）进行重新测试
+ 硬编码一个借口本地链路地址: `ipv6 address fe80:1234:adcd:1::3 link-local`
+ 查看IPv6路由表

###十六进制转换及子网划分练习

**Hex Conversion and Subnetting Practice**

请把今天剩下的时间用于练习这些重要的题目上。

+ 将十进制转换成十六进制（随机数字）
+ 将十六进制转换成十进制（随机数字）
+ IPv6子网划分（随机网络和场景）

