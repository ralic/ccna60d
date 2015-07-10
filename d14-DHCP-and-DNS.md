#第14天

**DHCP及DNS**

**DHCP and DNS**

##第14天任务

+ 阅读今天的课文
+ 复习昨天的课文
+ 完成今天的实验
+ 阅读ICND1记诵指南
+ 花15分钟在[subnetting.org](http://www.subnetting.org)上

主机使用动态主机配置协议（Dynamic Host Configuration Protocol, DHCP），紧接着加电启动后，收集到包括了IP地址、子网掩码及默认网关等初始配置信息。因为所有主机都需要一个IP地址，以在IP网络中进行通信，而DHCP就减轻了手动为每台主机配置一个IP地址的管理性负担。

域名系统（Domain Name System, DNS）将主机名称映射到IP地址，使得你可[www.in60days.com](www.in60days.com)输入到web浏览器中，而无需输入寄存该站点的服务器IP地址。

今天将学到以下内容。

+ DHCP操作, DHCP operations
+ 配置DHCP, configuring DHCP
+ DHCP故障排除, troubleshooting DHCP issues
+ DNS操作, DNS operations
+ 配置DNS, configuring DNS
+ DNS故障排除, troubleshooting DNS issues

本课对应了以下CCNA大纲要求。

+ 配置和验证DNS（IOS路由器）
    - 将路由器接口配置为使用DHCP, configure router interfaces to use DHCP
    - DHCP选项, DHCP options
    - 排除的地址, excluded addresses
    - 租期，lease time

##DHCP功能

**DHCP Functionality**

###DHCP操作

**DHCP Operations**

DHCP通过在网络上给主机自动分配IP信息，简化了网络管理任务。分配的信息可以包括IP地址、子网掩码及默认网关，且通常实在主机启动时。

在主机第一次启动时，如其已被配置为采用DHCP（大多数主机都是这样的），它就会发出一个询问分配IP信息的广播报文。该广播将为DHCP服务器收听到，同时该信息会被中继。

> Farai指出 -- "这是假定主机和DHCP服务器实在同一子网的情形，而如它们不再同一子网，就看下面的`ip helper-address`命令。"

![主机请求IP配置信息](images/1401.png)

*图14.1 -- 主机请求IP配置信息*

