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
