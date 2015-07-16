#第15天

**一二层排错**

**Layer 1 and Layer 2 Troubleshooting**

##第15天任务

+ 阅读今天的课文
+ 复习昨天的课文
+ 完成今天的实验
+ 阅读ICND1记诵指南
+ 在[subnetting.org](http://www.subnetting.org)花15分钟

先前数课中以涵盖了ICND1排错的许多要求，尤其是关于ACLs及IP分址方面。许多可能的故障都发生在一二层，一二层故障及其原因，是今天这课的重点。

LAN交换一种用在局域网中的包交换形式。LAN交换是在数据链路层的硬件中完成的。因为LAN交换是基于硬件的，其使用被称为介质访问控制地址（Media Access Control addresses, MAC地址）的硬件地址。**LAN交换机使用MAC地址来转发帧。**

今天将学习以下内容。

+ 物理层排错
+ VLAN、VTP及中继概述
+ VLANs排错
+ 运用`show vlan`命令

本模块对应了下面的CCNA大纲要求。

+ 一层故障的排错及处理
    - 组帧，framing
    - 循环冗余校验，CRC
    - 畸形帧，runts
    - 巨大帧，giants
    - 丢掉的数据包，dropped packets
    - 晚发冲突，[late collision](https://en.wikipedia.org/wiki/Late_collision)
    - 输入/输出错误，Input/Output errors
+ VLAN故障的排错和处理
    - 验证VLANs已配置, verify that VLANs are configured
    - 验证端口成员关系是正确的，verify that port membership is correct
    - 验证配置了IP地址, verify that the IP address is configured
+ 思科交换机上中继问题的排错和处理
    - 验证中继状态是正确的, verify that the trunk states are correct
    - 验证封装类型是正确配置的, verify that encapsulation is configured correctly
    - 验证那些VLANs是被放行的，verify that VLANs are allowed

##物理层上的排错

**Troubleshooting at the Phycical Layer**

思科IOS交换机支持好几个可用于一层，或至少怀疑是一层故障排错的命令。但是，除了对这些软件命令工具包要熟悉外，对可用对对链路状态的排错，或指示出错误情形的物理指示器（也就是那些LEDs）的掌握，也是重要的。

###使用发光二极管（LEDs）的链路状态排错

**Troubleshooting Link Status Using Light Emitting Diodes(LEDs)**

如可物理接触到交换机，那么LEDs就会是一项有用的排错工具。不同类型的思科Catalyst交换机，提供了不同的LED能力。掌握这些LEDs的意义，是Catalyst交换机链路状态及系统排错所不可或缺的部分。思科Catalyst交换机有一些可用于判断链路状态及其它一些诸如系统状态等变量的前面板LEDs。

经由Google"Catalyst 2960 Switch Hardware Installation Guide"，来查看Catalyst 2960型号交换机的思科文档。该安装和配置手册包含了几百页的注记、建议和技术信息。通读一下该文档是值得的，但不要期望从该文档得到CCNA考试大纲的内容（CCNA考试大纲内容在这本书才有）。

![思科2960交换机LEDs，图片版权归思科系统公司](images/1501.png)

<table>
<tr>
<td>1</td><td>系统LED</td><td>5</td><td>速率LED</td>
</tr>
<tr>
<td>2</td><td>冗余电源（redundant power supply, RPS） LED</td><td>6</td><td>PoE LED</td>
</tr>
<tr>
<td>3</td><td>状态LED</td><td>7</td><td>模式按钮</td>
</tr>
<tr>
<td>4</td><td>复用LED</td><td>8</td><td>端口LEDs</td>
</tr>
</table>

*图15.1 -- 思科2960交换机LEDs，图片版权归思科系统公司*

PoE LED只有在Catalyst 2960交换机型号上才能找到。

**系统LED**

**System LED**

系统LED表明系统通电了的（或是未通电）且正常发挥功能。

下表15.1列出了系统LED颜色及其所表明的状态。

*表15.1 -- 系统LED*

<table>
<tr><th>系统LED颜色</th><th>系统状态</th></tr>
<tr><td>不亮</td><td>系统未通电</td></tr>
<tr><td>绿色</td><td>系统运行正常</td></tr>
<tr><td>琥珀色（amber）</td><td>系统以通电，但未有正确发挥功能</td></tr>
</table>

**冗余电源LED**

**RPS LED**

冗余电源LED只在那些有着冗余电源的交换机上才有。下表15.2列出了RPS LED的颜色和其意义。

*表15.2 -- 冗余电源LEDs*

<table>
<tr><th>RPS LED颜色</th><th>状态</th></tr>
<tr><td>绿色</td><td>连接了RPS，且RPS在需要时就可提供后备电力</td></tr>
<tr><td>绿色闪烁(Blinking Green)</td><td>连接了RPS，但因为其正为另一设备提供电力（冗余已被分配给一台相邻设备）而不可用</td></tr>
<tr><td>琥珀色</td><td>RPS处于待机模式或故障状态（in standby mode or in a fault condition）。按下RPS上的Standby/Active按钮，此时该LED应变成绿色。如未变成绿色，则该RPS风扇可能损坏。请联系思科系统公司。</td></tr>
<tr><td>琥珀色闪烁</td><td>交换机内部电源失效，且正由RPS给交换机供电（冗余电源已分配给该设备）</td></tr>
</table>

**端口LEDs及其模式**

**Port LEDs and Modes**

端口LEDs提供了一组端口或单个端口的信息，如下表15.3所示。

*表15.3 -- 端口LEDs的模式*

<table>
<tr><th>所选模式LED</th><th>端口模式</th><th>说明</th></tr>
<tr><td>1 -- 系统</td><td></td><td></td></tr>
<tr><td>2 -- RPS</td><td></td><td>RPS状态</td></tr>
<tr><td>3 -- 状态</td><td>端口状态</td><td>端口状态（默认模式）</td></tr>
<tr><td>4 -- 复用</td><td>端口复用情况</td><td>复用模式：全双工或半双工</td></tr>
<tr><td>5 -- 速率</td><td>端口速率</td><td>端口运行速率：10, 100或1000Mbps</td></tr>
<tr><td>6 -- PoE</td><td>PoE端口供电</td><td>PoE状态</td></tr>
<tr><td>7 -- 模式</td><td></td><td>循环显示端口状态、复用模式及速率LEDs</td></tr>
<tr><td>8 -- 端口</td><td></td><td>依不同模式有不同含义</td></tr>
</table>

不停按下模式按钮（the Mode button）可在不同模式之间循环，直到需要的模式设置。这会改变端口LED颜色的意义，如下表15.4所示。

*表15.4 -- 模式设置*

<table>
<tr><th>端口模式</th><th>LED颜色</th><th>系统状态</th></tr>
<tr><td rowspan=6></td><td>不亮</td><td>未插入网线或管理性关闭</td></tr>
<tr><td>绿色</td><td>有链路且链路无问题</td></tr>
<tr><td>绿色闪烁</td><td>活动的：端口在发送或接收数据</td></tr>
<tr><td>绿色琥珀色交替闪烁</td><td>链路故障（link fault）：出现可影响连通性的错误帧，以及过多的冲突、循环冗余校验（CRC），同时将对以太网的alignment及jabber问题进行检测（[以太网错误的说明](EthernetErrorDescription.pdf), [以太网错误](EthernetErrors.pdf)）</td></tr>
<tr><td></td><td></td></tr>
<tr><td></td><td></td></tr>
<tr><td rowspan=2></td><td></td><td></td></tr>
<tr><td></td><td></td></tr>
<tr><td rowspan=6></td><td colspan=2></td></tr>
<tr><td></td><td></td></tr>
<tr><td></td><td></td></tr>
<tr><td></td><td></td></tr>
<tr><td></td><td></td></tr>
<tr><td colspan=2></td></tr>
<tr><td></td><td></td></tr>
<tr><td></td><td></td></tr>
<tr><td rowspan=5></td><td></td><td></td></tr>
<tr><td></td><td></td></tr>
<tr><td></td><td></td></tr>
<tr><td></td><td></td></tr>
<tr><td></td><td></td></tr>

</table>
