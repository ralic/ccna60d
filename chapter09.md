#第九天

__访问控制清单__

__Access Control Lists__

##第九天的任务

+ 阅读今天的课文
+ 完成今天的实验
+ 阅读ICND1记诵指南
+ 在[subnetting.org](http://www.subnetting.org)上花15分钟

和子网划分及VLSM一样，访问控制清单（access control lists, ACL）对于新CCNA学员来说，也是一大难点（one of the bugbear subjects）。有关ACL的问题有，学习有关的IOS配置命令、理解ACL规则（包括隐式的“deny all”规则），以及端口号和协议类型的学习。

与其它CCNA科目一样，应该在学习过程中一次完成一个小部分，在路由器上使用所见到的每个命令，并完成许多实验。

今天你将学到以下内容。

+ ACL基础
+ 标准ACLs，扩展ACLs， 以及命名ACLs
+ ACL 规则
+ 反掩码（wildcard masks）
+ ACL的配置
+ ACLs 故障排除

本课程对应了以下CCNA大纲要求。

+ 描述ACLs的类型、特性及应用
	- 标准ACLs, standard
	- 序列号, sequence numbers
	- ACLs的编辑, editing
	- 扩展的ACLs, extended
	- 命名的ACLs, named
	- 编号的ACLs, numbered
	- 日志选项, log option
	- 在某个网络环境下配置并验证ACLs

##ACL基础

__ACL Basics__

__ACLs用于过滤那些通过路由器的流量__。没有那个网络是会让任何流量都进入或流出该网络的。

__在流量过滤的同时，ACLs 还可用于对NAT地址池的引用，及对调试命令进行过滤（filter your debugging commands）, 以及对路由地图进行过滤（这是超出CCNA大纲要求的）__。

依据所配置的ACL类型，可实现基于源网络/IP地址的过滤、基于目的网络/IP地址的过滤、基于协议或基于端口号的过滤。可在路由器的任何接口，包括Telnet端口，上应用ACLs。

下面是3中主要的ACLs类型。

+ 标准的编号ACLs
+ 扩展的编号ACLs
+ 标准或扩展的命名ACLs

__标准的编号ACLs是可以应用到路由器上的最为基本的ACL形式__。它们是最易于配置的，因此其可用的过滤有着最大的限制。__它们仅能依据源IP地址或源网络进行过滤__。识别标准ACL的方法就是看配置行的前导数字；标准ACLs的该数字为1到99。

__扩展的编号ACLs可以有多得多的粒度, 但配置和故障排除起来会更难应付。__它们可以对某个目的或源IP地址或网络、某种协议类型以及某个端口号进行过滤（they can filter a destination or source IP address or network, a protocol type, and a port number）。可用于配置扩展ACLs的编号为100到199(包含100和199）。

__命名ACLs允许给某过滤清单一个名称，而不是编号__。这就令到在路由器配置中更易于区别这些ACLs了。命名ACLs可以是标准及扩展ACLs; 在该ACLs的初始化配置行处，可以选择其作为标准ACL还是扩展ACL。

为在CCNA考试中取得成功，并成为一名思科工程师，你需要理解以下内容。

+ 端口号，port numbers
+ ACL规则，ACL rules
+ ACLs的命令语法，command syntax for ACLs

##端口号，Port Numbers

如要通过CCNA考试，以及要在实际网络上工作，就必须要记住这些常见的端口号。在客户盯着你做事时，去查一下常见端口号是不可能的。这里有些你会碰到且需掌握的一些最常见的端口号。

<table>
<tr><th>端口</th><th>服务</th><th>端口</th><th>服务</th></tr>
<tr><td>20</td><td>FTP数据</td><td>80</td><td>HTTP</td></tr>
<tr><td>21</td><td>FTP控制</td><td>110</td><td>POP3</td></tr>
<tr><td>22</td><td>SSH</td><td>119</td><td>NNTP</td></tr>
<tr><td>23</td><td>Telnet</td><td>123</td><td>NTP</td></tr>
<tr><td>25</td><td>SMTP</td><td>161/162</td><td>SNMP</td></tr>
<tr><td>53</td><td>DNS</td><td>443</td><td>HTTPS(带有SSL的HTTP)</td></tr>
<tr><td>69</td><td>TFTP</td><td></td><td></td></tr>
</table>

## 访问控制清单规则，Access Control List Rules

这是最难掌握的部分之一。我从没有在哪本思科手册中见到里面曾写过一条完整的规则清单。仅有一些手册对其简单概过或是稍加解释，另外一些则完全不讲。难点就在于这些规则一直都在用，但到目前为止你都是通过试误法发现的它们（the difficulty is that the rules always apply but unitil now, you found them only by trial and error）。下面就是你需要知道的这些规则了。

###ACL规则一 -- 在每个接口的每个方向，只使用一条ACL

__Use only one ACL per interface per direction__

这么做是很明智的。在同一接口上，有多条ACLs去做不同的事情，大概不是你想要的。简单地配置一条ACL，来完成需要完成的事情，而不是将过滤器分散到两条或多条的清单中。本应将“每个协议（per protocol）”加入到此规则中，因为这里是可以包含IPX的访问控制清单的，不过在现代网络中，IP已成为唯一的协议了。

![接口上的一个方向仅做一条ACL](images/0901.png)
__图9.1 -- 接口上的一个方向仅做一条ACL__

###ACL规则二 -- ACLs行自顶向下进行处理

__the lines are processed top-down__

某些工程师在他们的ACL未如预期那样运行时感到迷惑。路由器会看看ACL的顶行，在发现匹配后，就会停在那里且不再对其它行进行检查了。为此，需要将__最明确的(最小的)那些条目放在ACL的顶部__（you need to put the most specific entries at the top of the ACL）。比如在利用ACL来阻挡主机172.16.1.1时的做法。

<table>
<tr><td>Permit 10.0.0.0</td><td></td><td>没有匹配的</td></tr>
<tr><td>Permit 192.168.1.1</td><td></td><td>没有匹配的</td></tr>
<tr><td>Permit 172.16.0.0</td><td>〇</td><td>匹配了-放行</td></tr>
<tr><td>Permit 172.16.1.0</td><td></td><td>不会处理了</td></tr>
<tr><td>Deny 172.16.1.1</td><td></td><td>不会处理了</td></tr>
</table>

在本例中，应该将`Deny 172.16.1.1`这行，放到顶部，或至少应在语句（statement）`Permit 172.16.0.0`之前。

###ACL规则三 -- 在每条ACL的底部，都有一句隐式的“deny all”

__There is an implicit "deny all" at the bottom of every ACL__

这条规则另很多工程师为难。在每条ACL的底部，有着一条看不见的命令。该命令设置为拒绝尚未匹配的所有流量。而阻止此命令起作用的唯一方法，就是在底部手动配置一条`permit all`命令。在取得来自IP地址172.20.1.1的某个进入的数据包时的做法。

<table>
<tr><td>Permit 10.0.0.0</td><td>无匹配项</td></tr>
<tr><td>Permit 192.168.1.1</td><td>无匹配项</td></tr>
<tr><td>Permit 172.16.0.0</td><td>无匹配项</td></tr>
<tr><td>Permit 172.16.1.0</td><td>无匹配项</td></tr>
<tr><td>[Deny all]</td><td>匹配 -- 丢弃数据包</td></tr>
</table>

你实际上想要路由器放行该数据包，但却拒绝了。原因就在于那条隐式的“deny all”命令了，而该命令实际上是一种安全手段。

###ACL规则四 -- 路由器是不能过滤自己产生的流量的

__The router can't filter self-generated traffic.__

这在某个实际网络上于部署ACL前进行测试时，会造成混乱。路由器不会过滤其自身产生的流量。在图9.2中有演示。

![对自身流量的ACL测试](images/0902.png)
__图9.2 -- 对自身流量的ACL测试__

###ACL规则五 -- 不能对运行中的ACL进行编辑

__You can't edit a live ACL.__

实际上，在IOS 12.4之前的版本中，只能对命名ACL进行编辑, 而不能对标准ACL或扩展ACLs两种进行编辑。这曾是ACL架构的一个局限（this was a limitation of ACL architecture）。在IOS 12.4之前，如想要编辑标准ACL或扩展ACL，就必须按照以下步骤进行（这里使用list 99作为例子）。

1. 使用命令`no ip access-group 99 in`，在接口上停用ACL流量（stop ACL traffic on the interface with the `no ip access-group 99 in` command）。
2. 将该条ACL复制粘贴到文本编辑器，并在那里编辑好。
3. 进入到ACL模式，将新的ACL粘贴上去。
4. 再次将该ACL应用到接口。

在实际的路由器上，执行下面的这些命令。

在接口上已创建并应用的ACL。

```
Router>en
Router#conf t
Enter configuration commands, one per line. End with CNTL/Z.
Router(config)#access-list 1 permit 172.16.1.1
Router(config)#access-list 1 permit 172.16.2.1
Router(config)#interface FastEthernet0/0
Router(config-if)#ip access-group 1 in
```

现在其从接口上卸下。

```
Router(config)#int FastEthernet0/0
Router(config-if)#no ip access-group 1 in
Router(config-if)#^Z
```

查看那些ACLs。将其复制并粘贴到文本编辑器，并进行修改。

```
Router#show run ← or show ip access lists
access-list 1 permit host 172.16.1.1
access-list 1 permit host 172.16.2.1
```

实际上还需在配置行之间加入一个叹号（如是将其粘贴到路由器上的情况下），来告诉路由器执行一次确认（you actually need to add an exclamation mark in-between each line of configuration, if you are pasting it in, to tell the router to do a carriage return）[wikipedia: 回车符](http://zh.wikipedia.org/wiki/%E5%9B%9E%E8%BD%A6%E7%AC%A6)。

```
access-list 1 permit host 172.16.1.1
!
access-list 1 permit host 172.16.2.2
```

下面是正被粘贴到路由器配置中的那些行。要先删除早先的ACL，再粘贴进新版本。

```
Router#conf t
Enter configuration commands, one per line. End with CNTL/Z.
Router(config)#no access-list 1
Router(config)#access-list 1 permit host 172.16.1.1
Router(config)#!
Router(config)#access-list 1 permit host 172.16.2.2
Router(config)#exit
Router#
%SYS-5-CONFIG_I: Configured from console by console
show ip access
Router#show ip access-lists
Standard IP access list 1
	permit host 172.16.1.1
	permit host 172.16.2.2
Router#
Router(config)#int FastEthernet0/0
Router(config-if)#ip access-group 1 in ← reapply to the interface
```

如使用的是Packet Tracer, 那么这些命可能不会工作。同时，请一定在某台路由器上尝试这些命令，因为它们是考试考点。__记住在编辑ACL前要先在接口上关闭它（此时它就不再是活动的了），以避免一些奇怪或是不可预期的行为发生__。而在IOS 12.4及以后的版本中，如何来编辑ACLs，会在后面演示。

###ACL规则六 -- 在接口上关闭ACL

__Disable the ACL on the interface.__

在打算短时间对ACL进行测试或是撤销ACL时，许多工程师都会将其完全删除掉。这是不必要的。如你要停止ACL运行，只需简单地将其从所应用到的接口上移除即可。

```
Router(config)#int FastEthernet0/0
Router(config-if)#no ip access-group 1 in
Router(config-if)#^Z
```

###ACL规则七 -- 可重用同一ACL

__You can reuse the same ACL.__

这是我在实际网络中经常见到的。整个网络通常都有着同样的ACL策略。与其配置多条ACLs，只需简单地引用同一ACL，然后在所需要的那些接口上应用该ACL即可。图9.3演示了此概念。

![ACL的重用](images/0903.png)
__图9.3 -- ACL的重用__

###ACL规则八 -- ACL应保持简短

ACLs的基本规则就是保持简短且只专注于做一件事情。许多新手的思科工程师，将其ACL延伸到数行那么长，最后，经深思熟虑后，就可以紧缩到少数几行的配置。前面提到的将那些最为特定的（最小的）行放在ACL的顶部。这是好的做法，从而可以节约路由器CPU的执行周期。

优良的ACL配置技能，来自于知识和操练。

###ACL规则九 -- 尽可能将ACL放在接近源的地方

思科文档建议将扩展ACL尽量放在源处，而将标准ACL尽量放在目的处，因为这可以避免不必要的开销，又能放行那些合法流量。

![将ACL尽量放在离源近的地方](images/0904.png)
__图9.4 -- 将ACL尽量放在离源近的地方__

>Farai 指出 -- “思科官方建议是扩展ACL尽量里源近，而标准ACL尽量离目的近”。

##反掩码

__Wildcard Masks__

__因为在ACLs及某些路由协议的配置中，反掩码是命令行的组成部分，所有有必要学习反掩码__。之所有存在反掩码，是因为需要有某种方式来告诉路由器，我们要去匹配IP地址或网络地址的哪些部分。

匹配实在二进制层面完成的，却可以采用与子网掩码相同的表示法，来容易地配置反掩码。一个二进制1告诉路由器忽略对应的位，0则是要匹配的位。

CCNA考试中反掩码计算的一种简易方法，就是把一个数与子网掩码相加，确保它们的和为255。那么如果子网掩码的某个8位值为192时，需要加上63才等于255。而如果子网掩码的某个8位值为255, 则需要加的就是0。看看下面的例子吧。

<table>
<tr><th>子网掩码</th><td>255</td><td>255</td><td>255</td><td>192</td></tr>
<tr><th>反掩码</th><td>0</td><td>0</td><td>0</td><td>63</td></tr>
<tr><th>相加之和</th><td>255</td><td>255</td><td>255</td><td>255</td></tr>
</table>

<table>
<tr><th>子网掩码</th><td>255</td><td>255</td><td>224</td><td>0</td></tr>
<tr><th>反掩码</th><td>0</td><td>0</td><td>31</td><td>255</td></tr>
<tr><th>相加之和</th><td>255</td><td>255</td><td>255</td><td>255</td></tr>
</table>

<table>
<tr><th>子网掩码</th><td>255</td><td>128</td><td>0</td><td>0</td></tr>
<tr><th>反掩码</th><td>0</td><td>127</td><td>255</td><td>255</td></tr>
<tr><th>相加之和</th><td>255</td><td>255</td><td>255</td><td>255</td></tr>
</table>

在想要ACL与匹配某个子网或是整个网络时，就需要输入一个反掩码。比如，要匹配172.20.1.0 255.255.224.0, 就需要输入下面的命令。

`Router(config)#access-list 1 permit 172.20.1.0 0.0.31.255`

而要匹配子网192.200.1.0 255.255.255.192, 就需要下面的命令。

`Router(config)#access-list 1 permit 192.200.1.0 0.0.0.63`

在OSPF中应用网络语句时也要当心(be careful when applying network statements with OSPF)，那位那也要用到反掩码。

在有着一个仅有两位主机位的网络时，也要当心，因为需要输入一条ACL来匹配这些主机位。比如，要匹配子网192.168.1.0 255.255.255.252, 或/30的话，需要输入下面的命令。

`Router(config)#access-list 1 permit 192.168.1.0 0.0.0.3`

这里剔除了一些配置，是为展示出对应的部分。上面的命令将匹配192.168.1.0网络上的1号和2号主机。而如果要匹配192.168.1.4/30网络上的5号和6号主机，则需输入下面的命令。

`Router(config)#access-list 1 permit 192.168.1.4 0.0.0.3`

请阅读子网划分和VLSM部分的课文，以更好地掌握此概念。

##访问控制清单的配置

__Configuring Access Control Lists__

熟能生巧，对于任何技能都是适用的。如同前面提到的，你应该在路由器上输入这里给出的每个例子，完成尽可能多的实验，并构建出自己的实例。在考试和现实世界中，你都需要精准快速的设计ACL。

接下来的章节中出现的标准和扩展ACLs都是编号ACLs。它们是配置ACLs的经典方法。命名ACLs是配置ACLs的另一种方式，将在其后的部分出现。

###标准ACLs

__Standard ACLs__

标准的编号ACLs是最易于配置的，所以拿它来作为开端是最好的。__标准ACLs只能实现依据源网络或源IP地址的过滤__。

![带有源和目的地址的进入数据包](images/0905.png)
__图9.5 -- 带有源和目的地址的进入数据包__

在上面的图9.5中，进入的数据包有着一个源和目的地址，但标准ACL只会查看源地址。该ACL会放行会拒绝该源地址（参见图9.6）。


![有着多台/个主机网络的网络](images/0906.png)


```
Router(config)#access-list 1 permit host 172.16.1.1
Router(config)#access-list 1 permit host 172.16.1.1
Router(config)#access-list 1 permit host 192.168.1.1
Router(config)#access-list 1 permit 10.1.0.0 0.0.255.255
```

此ACL应在服务器侧的路由器上应用。又记得在清单的底部有一条隐式的“deny all”，所以其它流量都会给阻止掉。

###扩展ACLs

__Extended ACLs__

__扩展的编号ACLs中可以构建出细得多的粒度__。而正是由于有了细得多的粒度，令到扩展的编号ACLs变得诡异起来。藉由扩展的编号ACLs，可以对源或目的网络地址、端口、协议及服务进行过滤。

一般来说，你可以看看扩展的ACLs配置语法，就像下面这样。

`access list# permit/deny [service/protocol] [source network/IP] [destination network/IP] [port#]`

比如下面这样。

```
access-list 101 deny tcp 10.1.0.0 0.0.255.255 host 172.30.1.1 eq telnet
access-list 100 permit tcp 10.1.0.0 0.0.255.255 host 172.30.1.1 eq ftp
access-list 100 permit icmp any any
```

![阻止服务器访问实例](images/0907.png)
__图9.7 -- 阻止服务器访问实例__


可为上面的网络配置一条ACL，以e-mail、web和文件服务器为例，可以像下面这样（应用在服务器侧的）。

```
access-list 100 permit tcp host 172.16.1.1 host 172.20.1.1 eq smtp
access-list 100 permit tcp 10.1.0.0 0.0.255.255 host 172.30.1.1 eq ftp
access-list 100 permit tcp host 192.168.1.1 host 172.40.1.1 eq www
```

而如有不同要求，就也可以像下面这条ACL。

```
access-list 101 deny icmp any 172.20.0.0 0.0.255.255
access-list 101 deny tcp 10.1.0.0 0.0.255.255 host 172.30.1.1 eq telnet
```

或者也可以像下面这样。

`access-list 102 permit tcp any host 172.30.1.1 eq ftp established`

关键字`[established]`告诉路由器仅放行在网络内部的主机所发起的流量。三次握手标志（ACK或RST位）将表明这点（the three-way handshake flags, ACK or RST bit, will indicate this）。

###命名ACLs

__Named ACLs__

与编号ACLs不同，命名ACLs可由其描述性名称容易地区分，而这在一些大型的配置中尤其有用。引入命名ACLs就是为增加灵活性及ACLs的易于管理的。命名ACLs可以看着是配置增强的提升，因为它并未对ACLs结构进行修改（仅改变了引用ACL的方式而已）。

其语法跟编号ACLs是相似的，主要的不同就是使用名称而不是编号来区分ACLs。和编号ACLs一样，可以配置标准的或扩展的命名ACLs。

在配置命名ACLs时的另一不同之处，就是必须一直使用命令`ip access-list`，这与编号ACLs可以只使用简单的`access-list`命令，是不一样的。

<pre>
Router(config)#access-list ?
	<1-99>				IP standard access list
	<100-199>			IP extended access list
	<1100-1199>			Extended 48-bit MAC address access list
	<1300-1999>			IP standard access list (expanded range)
	<200-299>			Protocol type-code access list
	<2000-2699>			IP extended access list (expanded range)
	<700-799>			48-bit MAC address access list
	dynamic-extended	Extend the dynamic ACL absolute timer
	rate-limit			Simple rate-limit specific access list
Router(config)#ip access-list ?
	extended	Extended access list
	log-update	Control access list log updates
	logging		Control access list logging
	resequence	Resequence access list
	standard	Standard access list
R1(config)#ip access-list standard ?
	<1-99>	Standard IP access-list number<1300-1999> Standard IP access-list number (expanded range)
	<b>WORD		Access-list name</b>
R1(config)#ip access-list extended ?
	<100-199>	Extended IP access-list number
	<2000-2699>	Extended IP access-list number (expanded range)
	<b>WORD 	Access-list name</b>
</pre>

命名ACLs在语法上与其它类型的ACLs（也就是标准和扩展的编号ACLs）有着轻微的不同。同时也__可以编辑活动的命名ACLs__, 这是一个有用的特性。只需简单地告诉路由器要配置一条命名ACL， 而不管它是标准的还是扩展的。在较新的IOS版本上，也可以编辑编号ACLs，所以请检查所用的平台。

在使用`ip access-list`命令常见一条命名ACL时，思科IOS会将你带入ACL配置模式，在那里就可以输入或是移除ACL条目了（就是那些拒绝或放行的访问条件）。图9.8展示了一条命名ACL的实例，以及相应的输出。

![命名ACL](images/0908.png)
__图9.8 -- 命名ACL__

```
Router(config)#ip access-list extended BlockWEB
Router(config-ext-nacl)#?
Ext Access List configuration commands:
	<1-2147483647>	Sequence Number
	default			Set a command to its defaults
	deny			Specify packets to reject
	dynamic			Specify a DYNAMIC list of PERMITs or DENYs
	evaluate		Evaluate an access list
	exit			Exit from access-list configuration mode
	no				Negate a command or set its defaults
	permit			Specify packets to forward
	remark			Access list entry comment
Router(config-ext-nacl)#deny tcp any any eq 80
Router(config-ext-nacl)#permit ip any any
```

命名ACL的验证，可通过下面的命令完成。

+ `show ip access-list`: 显示设备上所创建的所有ACLs
+ `show ip access-list <acl_name>`: 显示某条特定的命名ACL

```
Router(config)#do show ip access-lists
Standard IP access list test
	30 permit 10.1.1.1
	20 permit 192.168.1.1
	15 permit 172.20.1.1
	10 permit 172.16.1.1
```

要知道如何来增加或是删除某条命令ACL中的条目，请参考下面的“ACL序号（ACL Sequence Numbers）”小节。

###应用ACLs

__Applying ACLs__

__为让ACLs发挥效果，就必须将ACL应用到路由器的某个接口或端口上__。之所以这样讲，是因为我曾见到许多的新手思科工程师在敲入了ACL后，就想为什么它不工作！或者他们配置了ACL，却将错误的ACL编号或命名应用到相应的接口上。

__如要应用在某条线路上，就必须使用`access-class`命令来指定它__，而__如果是应用在某个接口上，就要用`ip access-group`命令__。思科这么做的原因，我也不知道。

这里有应用ACLs到端口或接口上的三个实例。

接口上的应用。

```
Router(config)#int FastEthernet0/0
Router(config-if)#ip access-group 101 in
```

线路上的应用。

```
Router(config)#line vty 0 15
Router(config-line)#access-class 101 in
```

接口上的应用。

```
Router(config)#int FastEthernet0/0
Router(config-if)#ip access-group BlockWEB in
```

##ACL序号

__ACL Sequence Numbers__

自12.4往后，你会发现思科IOS给每个ACL条目添加了序号。那么现在就可以创建一条访问控制清单，并在其后从它里面一处一行了。

```
Router(config)#ip access-list standard test
Router(config-std-nacl)#permit 172.16.1.1
Router(config-std-nacl)#permit 192.168.1.1
Router(config-std-nacl)#permit 10.1.1.1
Router(config-std-nacl)#
Router(config-std-nacl)#exit
Router(config)#exit
Router#
*Jun 6 07:38:14.155: %SYS-5-CONFIG_I: Configured from console by console access
Router#show ip access-lists
Standard IP access list test
	30 permit 10.1.1.1
	20 permit 192.168.1.1
	10 permit 172.16.1.1
```

注意到__在路由器运行配置中，序号并不会显示出来__。要查看它们，必须执行一个`show [ip] access-list`命令。

##加入一个ACL行

__Add an ACL Line__

__要加入一个新的ACL行，只需简单地输入新的序号并接着输入该ACL语句__。下面的例子展示如何往现有的ACL中加入行15。

```
Router#conf t
Enter configuration commands, one per line. End with CNTL/Z.
Router(config)#ip access
Router(config)#ip access-list standard test
Router(config-std-nacl)#15 permit 172.20.1.1
Router(config-std-nacl)#
Router(config-std-nacl)#do show ip access
Router(config-std-nacl)#do show ip access-lists
Standard IP access list test
	30 permit 10.1.1.1
	20 permit 192.168.1.1
	15 permit 172.20.1.1
	10 permit 172.16.1.1
Router(config-std-nacl)#
```

###移除一个ACL行

__Remove an ACL Line__

要移除某个ACL行，只需简单地敲入`no <seq_number>`命令即可，就如同下面的例子中行20被删除掉了。

```
Router#conf t
Enter configuration commands, one per line. End with CNTL/Z.
Router(config)#ip access
Router(config)#ip access-list standard test
Router(config-std-nacl)#no 20
Router(config-std-nacl)#
Router(config-std-nacl)#do show ip access
Router(config-std-nacl)#do show ip access-lists
Standard IP access list test30 permit 10.1.1.1
	15 permit 172.20.1.1
	10 permit 172.16.1.1
Router(config-std-nacl)#
```

###为某条ACL重新编号

__Resequence an ACL__

要对某条ACL重新编号，使用`ip access-list resequence <acl_name> <starting_seq_number> <step_to_increment>`命令。该命令的行为可由下面的例子进行检验。

```
Router(config)#ip access-list resequence test 100 20
Router(config)#do show ip access-lists
Standard IP access list test
	100 permit 10.1.1.1
	120 permit 172.20.1.1
	140 permit 172.16.1.1
Router(config-std-nacl)#
```

命令`resequence`则会创建新的序号，自100开始，每个新行增加20。译者注：在更新的IOS版本中，此命令可指定开始序号及步进序号。

###ACL日志

__ACL Logging__

默认情况下，通过那些为某个接口的数据包所匹配上的ACL条目，会创建出一个不断增大的计数器，该计数器可使用`show ip access-list`命令进行查看，如下面的例子所示。

<pre>
Router#show ip access-lists
Extended IP access list test
	10 deny tcp any any eq 80 <b>(10 matches)</b>
	20 permit ip any any <b>(56 matches)</b>
</pre>
