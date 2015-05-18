# 第五天

__IP 地址分配__
__IP Addressing__

## 第五天的任务

* 阅读今天的课文
* 复习昨天的课文
* 完成今天的实验
* 阅读 ICND1 记诵指南
* 花 15 分钟浏览 [subnetting.org](http://subnetting.org/) 网站

欢迎来到今天的学习，许多人都发现今天的内容是 CCNA 大纲中最难掌握的部分之一。为理解 CCNA 考试的 IP 分址， 我们必须涵盖__二进制运算及十六进制计数系统__（binary mathematics and the hexadecimal numbering system）、__地址类别__(classes of adresses)、__2 的指数__（powers of two）和__诸如零号子网（subnet zero）等规则__, 以及__广播地址与网络地址__，还有__用于计算子网地址和主机地址的公式__。

尽管有这些难点，但请无需担心；这都是有个过程的，而不是一蹴而就的，所以请跟上课文，就一定会发现我们会在本书中屡次回顾到这些概念。

今天会学到这些内容。

* IP 分址（采用二进制和十六进制），IP addressing (using binary and hexadecimal)
* IP 地址的使用，Using IP addresses
* 子网划分，Subnetting
* 简易子网划分，Easy subnetting
* 网络规划设计，Network design
* 采用 VLSM, Using VLSM
* 切分网络，Slicing down networks

本模块对应的是 CCNA 大纲要求的以下部分。

* 描述 IPv4 分址中使用私有和公共 IP 地址的做法和必要性，Describe the operationg and necessity of using private and public IP addresses for IPv4 addressing
* 找出采行 VLSM 和汇总技术，用以满足某个 LAN/WAN 环境中分址要求的恰当 IPv4 分址方案，Identify the appropriate IPv4 addressing scheme using VLSM and summarisation to satisfy addressing requirements in a LAN/WAN environment
* 对有关 IP 分址和主机配置有关的故障进行排除和修正， Troubleshoot and correct common problems associated with IP addressing and host configurations

思科已经将一些 VLSM 要求加入到 ICND1 和 ICND2 考试中了。而在 ICND2 考试中看起来考得更多一些，不过两个考试都需要你做好解答问题的准备。__在掌握 VLSM 前，你需要先理解 IP 分址和子网划分__。

## IP 分址，IP addressing

网络上的所有设备，都需要某种方法来将其识别为某台特定主机。早期网络简单地采用某种命名格式，同时服务器上维护着一张 MAC 地址与主机名称的映射图。服务器上的表格迅速增长，伴随其产生诸如一致性及准确性（consistency and accuracy）问题（如图 5.1）。IP 分址有效地解决了此问题。

!["设备命名表变得十分累赘"](images/0501.png)

### IP 版本 4, IP Version 4

IP 版本 4(IPv4）设计用于解决设备命名问题。IPv4 使用二进制在网络设备上应用一个地址。它使用 32 位二进制数，分成每 8 位的 4 组。下面是二进制的 IPv4 地址的一个实例。

`11000000.10100011.11110000.10101011`

以十进制来看，就是。

`192.168.240.171`

每一个二进制位表示一个十进制数，你可以在相应的列中，依据该列是 1 还是 0, 而使用或不用其对应的十进制数。下面是 8 个列。

<table>
<tr><td>128</td><td>64</td><td>32</td><td>16</td><td>8</td><td>4</td><td>2</td><td>1</td></tr>
<tr><td>1</td><td>1</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
</table>

从上表中可以看到，仅有前两个十进制数被用到（下方有 1 的两个），这就产生出数值 128+64=192。

### 二进制，Binary

为理解 IP 寻址(IP addressing)的工作原理, 你需要理解二进制计数法（binary mathematics）。计算机及网络设备是不理解十进制的。我们使用 10 进制，是由于它是一种使用了 10 个数字的计数系统，由很久很久以前的穴居人类在意识到他有 10 个手指头，可以在有恐龙经过洞口时用来数恐龙时发明的。

计算机和网络设备只明白电信号。而电信号不是开就是关，唯一可用的计数系统就是二进制。二进制只用到两个数字，0 和 1。0 表示线路上没有电脉冲，而 1 就表示线路上有一个脉冲。

使用二进制值，可以生成任何数字。加的二进制数值越多，得到的数量就越大。所加入的每个二进制值，其下一个数字都要是它的两倍（也就是，1 到2到4到8到16,以致无穷），从右往左。如有两位，最多可计到3。只需将0或1放入到表格的列中，以确定是否要使用该列的值。

我们从仅有两位的二进制数开始。

<table>
<tr><td>2</td><td>1</td></tr>
<tr><td>0</td><td>0</td></tr>
</table>

0+0=0

<table>
<tr><td>2</td><td>1</td></tr>
<tr><td>0</td><td>1</td></tr>
</table>

0+1=1

<table>
<tr><td>2</td><td>1</td></tr>
<tr><td>1</td><td>0</td></tr>
</table>

2+0=2

<table>
<tr><td>2</td><td>1</td></tr>
<tr><td>1</td><td>1</td></tr>
</table>

2+1=3

如你使用8位二进制数（也就是一个八位字节），你能取得如何从0到255之间的数值。而你可以看到，这些位数自右往左移动。

<table>
<tr><td>128</td><td>64</td><td>32</td><td>16</td><td>8</td><td>4</td><td>2</td><td>1</td></tr>
<tr><td></td><td></td><td></td><td></td><td></td><td></td><td></td><td></td></tr>
</table>

在往各列中填入0时，就有了十进制的0。

<table>
<tr><td>128</td><td>64</td><td>32</td><td>16</td><td>8</td><td>4</td><td>2</td><td>1</td></tr>
<tr><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td><td>0</td></tr>
</table>

而将1填入各列，就得到了十进制的255.

<table>
<tr><td>128</td><td>64</td><td>32</td><td>16</td><td>8</td><td>4</td><td>2</td><td>1</td></tr>
<tr><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td><td>1</td></tr>
</table>

不信吗？

128+64+32+16+8+4+2+1=255

如此，逻辑使然，你实际上可以通过将0或1放入不同的列，而生成0到255之间的任何数值。比如。

<table>
<tr><td>128</td><td>64</td><td>32</td><td>16</td><td>8</td><td>4</td><td>2</td><td>1</td></tr>
<tr><td>0</td><td>0</td><td>1</td><td>0</td><td>1</td><td>1</td><td>0</td><td>0</td></tr>
</table>

32+8+4=44

__上面的基础知识，是IP寻址和子网划分的基础。__下面的表5.1对你现在所掌握的进行了总结。这些值可用作任意子网掩码，所以请留心一下。

__表 5.1 -- 二进制值，Binary Values__

<table>
<tr><th>二进制，Binary</th><th>十进制，Decimal</th></tr>
<tr><td>1000 0000</td><td>128</td></tr>
<tr><td>1100 0000</td><td>192</td></tr>
<tr><td>1110 0000</td><td>224</td></tr>
<tr><td>1111 0000</td><td>240</td></tr>
<tr><td>1111 1000</td><td>248</td></tr>
<tr><td>1111 1100</td><td>252</td></tr>
<tr><td>1111 1110</td><td>254</td></tr>
<tr><td>1111 1111</td><td>255</td></tr>
</table>

构造一些你自己的二进制数，确保你完全地掌握了这个概念。

### 十六进制，Hexadecimal

十六进制（hex）是另一个替代的计数系统。比起以2或10来计数，它用到16个数字或字母。十六进制从0开始知道F，如下面所示。
`0	1	2	3	4	5	6	7	8	9	A	B	C	D	E	F`

每位十六进制数实际上代表的是4位二进制数，如表5.2所示。

__表5.2 -- 十进制、十六进制和二进制位数，Decimal, Hex, and Binary Digits__

<table>
<tr><th>十进制，Decimal</th><td>0</td><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td><td>6</td><td>7</td></tr>
<tr><th>十六进制，Hex</th><td>0</td><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td><td>6</td><td>7</td></tr>
<tr><th>二进制，Binary</th><td>0000</td><td>0001</td><td>0010</td><td>0011</td><td>0100</td><td>0101</td><td>0110</td><td>0111</td></tr>
</table>

<table>
<tr><th>十进制，Decimal</th><td>8</td><td>9</td><td>10</td><td>11</td><td>12</td><td>13</td><td>14</td><td>15</td></tr>
<tr><th>十六进制，Hex</th><td>8</td><td>9</td><td>A</td><td>B</td><td>C</td><td>D</td><td>E</td><td>F</td></tr>
<tr><th>二进制，Binary</th><td>1000</td><td>1001</td><td>1010</td><td>1011</td><td>1100</td><td>1101</td><td>1110</td><td>1111</td></tr>
</table>

将二进制转换成十六进制及十进制，是相当简单的，如表5.3所示。

<table>
<tr><th>十进制，Decimal</th><td>13</td><td>6</td><td>2</td><td>12</td></tr>
<tr><th>十六进制, Hex</th><td>D</td><td>6</td><td>2</td><td>C</td></tr>
<tr><th>二进制，Binary</th><td>1101</td><td>0110</td><td>0010</td><td>1100</td></tr>
</table>
