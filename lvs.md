<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [简介](#%E7%AE%80%E4%BB%8B)
- [IP虚拟服务器软件IPVS](#ip%E8%99%9A%E6%8B%9F%E6%9C%8D%E5%8A%A1%E5%99%A8%E8%BD%AF%E4%BB%B6ipvs)
  - [:point_right:IPVS软件实现了这三种IP负载均衡技术](#point_rightipvs%E8%BD%AF%E4%BB%B6%E5%AE%9E%E7%8E%B0%E4%BA%86%E8%BF%99%E4%B8%89%E7%A7%8Dip%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1%E6%8A%80%E6%9C%AF)
    - [:point_right:Virtual Server via Network Address Translation（VS/NAT）](#point_rightvirtual-server-via-network-address-translationvsnat)
    - [:point_right:Virtual Server via IP **Tunneling（VS/TUN）**](#point_rightvirtual-server-via-ip-tunnelingvstun)
    - [:point_right:Virtual Server via Direct Routing（VS/DR）](#point_rightvirtual-server-via-direct-routingvsdr)
  - [IPVS调度器实现了如下八种负载调度算法](#ipvs%E8%B0%83%E5%BA%A6%E5%99%A8%E5%AE%9E%E7%8E%B0%E4%BA%86%E5%A6%82%E4%B8%8B%E5%85%AB%E7%A7%8D%E8%B4%9F%E8%BD%BD%E8%B0%83%E5%BA%A6%E7%AE%97%E6%B3%95)
- [**三种方法的优缺点比较**](#%E4%B8%89%E7%A7%8D%E6%96%B9%E6%B3%95%E7%9A%84%E4%BC%98%E7%BC%BA%E7%82%B9%E6%AF%94%E8%BE%83)
- [适用性](#%E9%80%82%E7%94%A8%E6%80%A7)
- [:point_right:LVS命令行参数](#point_rightlvs%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%8F%82%E6%95%B0)
- [LVS调度算法](#lvs%E8%B0%83%E5%BA%A6%E7%AE%97%E6%B3%95)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 简介

针对高可伸缩、高可用网络服务的需求，我们给出了基于IP层和基于内容请求分发的负载平衡调度解决方法，并在Linux内核中实现了这些方法，将一组服务器构成一个实现可伸缩的、高可用网络服务的虚拟服务器。我们的负载调度技术是在Linux内核中实现的，我们称之为Linux虚拟服务器（Linux Virtual Server）。

对用硬件和软件方法实现高可伸缩、高可用网络服务的需求不断增长，这种需求可以归结以下几点：

- 可伸缩性（Scalability），当服务的负载增长时，系统能被扩展来满足需求，且不降低服务质量。
- 高可用性（Availability），尽管部分硬件和软件会发生故障，整个系统的服务必须是每天24小时每星期7天可用的。
- 可管理性（Manageability），整个系统可能在物理上很大，但应该容易管理。
- 价格有效性（Cost-effectiveness），整个系统实现是经济的、易支付的。

Linux Virtual Server项目的目标 ：使用集群技术和Linux操作系统实现一个高性能、高可用的服务器，它具有很好的可伸缩性（Scalability）、可靠性（Reliability）和可管理性（Manageability）。

# IP虚拟服务器软件IPVS

## :point_right:IPVS软件实现了这三种IP负载均衡技术

### :point_right:Virtual Server via Network Address Translation（VS/NAT）

通过网络地址转换，调度器重写请求报文的目标地址，根据预设的调度算法，将请求分派给后端的真实服务器；真实服务器的响应报文通过调度器时，报文的源地址被重写，再返回给客户，完成整个负载调度过程。

### :point_right:Virtual Server via IP **Tunneling（VS/TUN）**

采用NAT技术时，由于请求和响应报文都必须经过调度器地址重写，当客户请求越来越多时，调度器的处理能力将成为瓶颈。为了解决这个问题，调度器把请求报 文通过IP隧道转发至真实服务器，而真实服务器将响应直接返回给客户，所以调度器只处理请求报文。由于一般网络服务应答比请求报文大许多，采用 VS/TUN技术后，集群系统的最大吞吐量可以提高10倍。

### :point_right:Virtual Server via Direct Routing（VS/DR）

VS/DR通过改写请求报文的MAC地址，将请求发送到真实服务器，而真实服务器将响应直接返回给客户。同VS/TUN技术一样，VS/DR技术可极大地 提高集群系统的伸缩性。这种方法没有IP隧道的开销，对集群中的真实服务器也没有必须支持IP隧道协议的要求，但是要求调度器与真实服务器都有一块网卡连 在同一物理网段上。

## IPVS调度器实现了如下八种负载调度算法

1. **轮叫（Round Robin）**调度器通过"轮叫"调度算法将外部请求按顺序轮流分配到集群中的真实服务器上，它均等地对待每一台服务器，而不管服务器上实际的连接数和系统负载。
2. **加权轮叫（Weighted Round Robin）**调度器通过"加权轮叫"调度算法根据真实服务器的不同处理能力来调度访问请求。这样可以保证处理能力强的服务器处理更多的访问流量。调度器可以自动问询真实服务器的负载情况，并动态地调整其权值。
3. **最少链接（Least Connections）**调度器通过"最少连接"调度算法动态地将网络请求调度到已建立的链接数最少的服务器上。如果集群系统的真实服务器具有相近的系统性能，采用"最小连接"调度算法可以较好地均衡负载。
4. **加权最少链接（Weighted  Least Connections）**在集群系统中的服务器性能差异较大的情况下，调度器采用"加权最少链接"调度算法优化负载均衡性能，具有较高权值的服务器将承受较大比例的活动连接负载。调度器可以自动问询真实服务器的负载情况，并动态地调整其权值。
5. **基于局部性的最少链接（Locality-Based Least Connections**） "基于局部性的最少链接" 调度算法是针对目标IP地址的负载均衡，目前主要用于Cache集群系统。该算法根据请求的目标IP地址找出该目标IP地址最近使用的服务器，若该服务器 是可用的且没有超载，将请求发送到该服务器；若服务器不存在，或者该服务器超载且有服务器处于一半的工作负载，则用"最少链接"的原则选出一个可用的服务 器，将请求发送到该服务器。
6. **带复制的基于局部性最少链接（Locality-Based Least Connections with Replication）** "带复制的基于局部性最少链接"调度算法也是针对目标IP地址的负载均衡，目前主要用于Cache集群系统。它与LBLC算法的不同之处是它要维护从一个 目标IP地址到一组服务器的映射，而LBLC算法维护从一个目标IP地址到一台服务器的映射。该算法根据请求的目标IP地址找出该目标IP地址对应的服务 器组，按"最小连接"原则从服务器组中选出一台服务器，若服务器没有超载，将请求发送到该服务器，若服务器超载；则按"最小连接"原则从这个集群中选出一 台服务器，将该服务器加入到服务器组中，将请求发送到该服务器。同时，当该服务器组有一段时间没有被修改，将最忙的服务器从服务器组中删除，以降低复制的 程度。
7. **目标地址散列（Destination Hashing）** "目标地址散列"调度算法根据请求的目标IP地址，作为散列键（Hash Key）从静态分配的散列表找出对应的服务器，若该服务器是可用的且未超载，将请求发送到该服务器，否则返回空。
8. **源地址散列（Source Hashing）** "源地址散列"调度算法根据请求的源IP地址，作为散列键（Hash Key）从静态分配的散列表找出对应的服务器，若该服务器是可用的且未超载，将请求发送到该服务器，否则返回空。

# **三种方法的优缺点比较**

**1. Virtual Server via** **NAT**

VS/NAT 的优点是服务器可以运行任何支持TCP/IP的操作系统，它只需要一个IP地址配置在调度器上，服务器组可以用私有的IP地址。缺点是它的伸缩能力有限， 当服务器结点数目升到20时，调度器本身有可能成为系统的新瓶颈，因为在VS/NAT中请求和响应报文都需要通过负载调度器。 我们在Pentium 166 处理器的主机上测得重写报文的平均延时为60us，性能更高的处理器上延时会短一些。假设TCP报文的平均长度为536 Bytes，则调度器的最大吞吐量为8.93 MBytes/s. 我们再假设每台服务器的吞吐量为800KBytes/s，这样一个调度器可以带动10台服务器。（注：这是很早以前测得的数据）

基于 VS/NAT的的集群系统可以适合许多服务器的性能要求。如果负载调度器成为系统新的瓶颈，可以有三种方法解决这个问题：混合方法、VS/TUN和 VS/DR。在DNS混合集群系统中，有若干个VS/NAT负载调度器，每个负载调度器带自己的服务器集群，同时这些负载调度器又通过RR-DNS组成简 单的域名。但VS/TUN和VS/DR是提高系统吞吐量的更好方法。

对于那些将IP地址或者端口号在报文数据中传送的网络服务，需要编写相应的应用模块来转换报文数据中的IP地址或者端口号。这会带来实现的工作量，同时应用模块检查报文的开销会降低系统的吞吐率。

**2. Virtual Server via** **IP** **Tunneling**

在VS/TUN 的集群系统中，负载调度器只将请求调度到不同的后端服务器，后端服务器将应答的数据直接返回给用户。这样，负载调度器就可以处理大量的请求，它甚至可以调 度百台以上的服务器（同等规模的服务器），而它不会成为系统的瓶颈。即使负载调度器只有100Mbps的全双工网卡，整个系统的最大吞吐量可超过 1Gbps。所以，VS/TUN可以极大地增加负载调度器调度的服务器数量。VS/TUN调度器可以调度上百台服务器，而它本身不会成为系统的瓶颈，可以 用来构建高性能的超级服务器。

VS/TUN技术对服务器有要求，即所有的服务器必须支持“IP Tunneling”或者“IP Encapsulation”协议。目前，VS/TUN的后端服务器主要运行Linux操作系统，我们没对其他操作系统进行测试。因为“IP Tunneling”正成为各个操作系统的标准协议，所以VS/TUN应该会适用运行其他操作系统的后端服务器。

**3. Virtual Server via Direct Routing**

跟VS/TUN方法一样，VS/DR调度器只处理客户到服务器端的连接，响应数据可以直接从独立的网络路由返回给客户。这可以极大地提高LVS集群系统的伸缩性。

跟VS/TUN相比，这种方法没有IP隧道的开销，但是要求负载调度器与实际服务器都有一块网卡连在同一物理网段上，服务器网络设备（或者设备别名）不作ARP响应，或者能将报文重定向（Redirect）到本地的Socket端口上。

> 在分析网络地址转换方法（VS/NAT）的缺点和网络服务的非对称性的基础上，我们给出了通过IP隧道实现虚拟服务器的方法VS/TUN，和通过直接路由实现虚拟服务器的方法VS/DR，极大地提高了系统的伸缩性。

# 适用性

适用性

后端服务器可运行任何支持TCP/IP的操作系统，包括Linux，各种Unix（如FreeBSD、Sun Solaris、HP Unix等），Mac/OS和Windows NT/2000等。

负载调度器能够支持绝大多数的TCP和UDP协议：

协议        内 容

TCP        HTTP，FTP，PROXY，SMTP，POP3，IMAP4，DNS，LDAP，HTTPS，SSMTP等

UDP        DNS，NTP，ICP，视频、音频流播放协议等

无需对客户机和服务器作任何修改，可适用大多数Internet服务。

# :point_right:LVS命令行参数

```Bash
yum install -y ipvsadm

-A, --add-service：     为ipvs虚拟服务器添加一个虚拟服务，即添加一个需要被负载均衡的虚拟地址。虚拟地址需要是ip地址，端口号，协议的形式。
-E, --edit-service：     修改一个虚拟服务。
-D, --delete-service： 删除一个虚拟服务。
-C, --clear：               清除所有虚拟服务。
-R, --restore：            从标准输入获取ipvsadm命令。一般结合下边的-S使用。
-S, --save：                从标准输出输出虚拟服务器的规则。可以将虚拟服务器的规则保存，在以后通过-R直接读入，以实现自动化配置。
-a, --add-server：      为虚拟服务添加一个real server（RS）
-e, --edit-server：      修改RS
-d, --delete-server：  删除
-L, -l, --list：              列出虚拟服务表中的所有虚拟服务。可以指定地址。添加-c显示连接表。
-Z, --zero：               将所有数据相关的记录清零。这些记录一般用于调度策略。
--set tcp tcpfin udp：修改协议的超时时间。
--start-daemon state：设置虚拟服务器的备服务器，用来实现主备服务器冗余。（注：该功能只支持ipv4）
--stop-daemon：      停止备服务器。
```

配置vip 一般使用keepalived

```Bash
ip addr add 10.0.0.31/24 dev eth0 label eth0:0
ip addr add 10.0.0.32/24 dev eth0 label eth0:0
```

:point_right:添加lvs

```Bash
ipvsadm -C #清空所有规则
ipvsadm -A -t 10.0.0.31:80 -s rr

# 添加真实服务器(-g参数设置LVS工作模式为DR模式，-w设置权重)
ipvsadm -a -t 10.0.0.31:80 -r 10.0.0.7 -g -w 1
ipvsadm -a -t 10.0.0.31:80 -r 10.0.0.8 -g -w 2

ipvsadm -Ln
```

# LVS调度算法 

固定调度算法：rr，wrr，dh，sh 

- rr：轮询（round robin） 
- wrr：加权轮询（weight round robin）
- dh：目标地址散列调度算法 （destination hash） 
- sh：源地址散列调度算法（source hash） 

动态调度算法：wlc，lc，lblc，lblcr 

- lc：最少连接数（least-connection） 
- wlc：加权最少连接数（weight least-connection）