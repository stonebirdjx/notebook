<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [简介](#%E7%AE%80%E4%BB%8B)
- [:point_right:守护进程](#point_right%E5%AE%88%E6%8A%A4%E8%BF%9B%E7%A8%8B)
- [:point_right:安装keepalived](#point_right%E5%AE%89%E8%A3%85keepalived)
- [:point_right:keepalived命令行参数](#point_rightkeepalived%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%8F%82%E6%95%B0)
- [**:point_right:genhash实用程序**](#point_rightgenhash%E5%AE%9E%E7%94%A8%E7%A8%8B%E5%BA%8F)
  - [:point_right:健康检查](#point_right%E5%81%A5%E5%BA%B7%E6%A3%80%E6%9F%A5)
- [:point_right:**Keepalived配置**](#point_rightkeepalived%E9%85%8D%E7%BD%AE)
  - [**:point_right:全局定义概要**](#point_right%E5%85%A8%E5%B1%80%E5%AE%9A%E4%B9%89%E6%A6%82%E8%A6%81)
  - [**:point_right:虚拟服务器定义**](#point_right%E8%99%9A%E6%8B%9F%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%AE%9A%E4%B9%89)
  - [:point_right:**VRRP实例**](#point_rightvrrp%E5%AE%9E%E4%BE%8B)
- [**:point_right:IPVS调度算法**](#point_rightipvs%E8%B0%83%E5%BA%A6%E7%AE%97%E6%B3%95)
  - [:point_right:轮转(Round Robin,rr)](#point_right%E8%BD%AE%E8%BD%ACround-robinrr)
  - [:point_right:加权轮转(Weighted Round Robin, wrr)](#point_right%E5%8A%A0%E6%9D%83%E8%BD%AE%E8%BD%ACweighted-round-robin-wrr)
  - [:point_right:最少连接(Least Connection,lc)](#point_right%E6%9C%80%E5%B0%91%E8%BF%9E%E6%8E%A5least-connectionlc)
  - [:point_right:加权最少连接(Weighted Least Connection, wlc)](#point_right%E5%8A%A0%E6%9D%83%E6%9C%80%E5%B0%91%E8%BF%9E%E6%8E%A5weighted-least-connection-wlc)
  - [基于局部性的最少连接(Locality-Based Least Connection, lblc)](#%E5%9F%BA%E4%BA%8E%E5%B1%80%E9%83%A8%E6%80%A7%E7%9A%84%E6%9C%80%E5%B0%91%E8%BF%9E%E6%8E%A5locality-based-least-connection-lblc)
  - [带复制的基于局部性最少连接(Locality-Based Least Connection with Replication, lblcr)](#%E5%B8%A6%E5%A4%8D%E5%88%B6%E7%9A%84%E5%9F%BA%E4%BA%8E%E5%B1%80%E9%83%A8%E6%80%A7%E6%9C%80%E5%B0%91%E8%BF%9E%E6%8E%A5locality-based-least-connection-with-replication-lblcr)
  - [:point_right:目标地址哈希(Destination Hashing, dh)](#point_right%E7%9B%AE%E6%A0%87%E5%9C%B0%E5%9D%80%E5%93%88%E5%B8%8Cdestination-hashing-dh)
  - [:point_right:源地址哈希(Source Hashing, sh)](#point_right%E6%BA%90%E5%9C%B0%E5%9D%80%E5%93%88%E5%B8%8Csource-hashing-sh)
  - [最短预期延迟(Shortest Expected Delay, seq)](#%E6%9C%80%E7%9F%AD%E9%A2%84%E6%9C%9F%E5%BB%B6%E8%BF%9Fshortest-expected-delay-seq)
  - [**永不排队(Never** Queue, nq)](#%E6%B0%B8%E4%B8%8D%E6%8E%92%E9%98%9Fnever-queue-nq)
  - [溢出连接(Overflow-Connection, ovf)](#%E6%BA%A2%E5%87%BA%E8%BF%9E%E6%8E%A5overflow-connection-ovf)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 简介

Keepalived提供了兼具负载均衡和高可用性的框架。负载均衡框架依赖于众所周知且广泛使用的Linux Virtual Server(IPVS|LVS)内核模块，该模块提供第4层负载均衡。

高可用性是通过虚拟路由器冗余协议(VRRP)来实现的，VRRP是路由器故障转移的基础。

Keepalived是用纯ANSI/ISO C编写的。该软件围绕一个中央I/O多路复用器进行连接，以提供实时网络设计。

# :point_right:守护进程

为了确保健壮性和稳定性，守护进程被切分为3个不同进程：

- 一个极简的父进程，负责fork和监控子进程
- 两个子进程，一个负责VRRP框架，另一个负责健康检查

```Bash
PID    111    Keepalived        < - 监视子进程的父进程
       112    \ _ Keepalived    < - VRRP子进程
       113    \ _ Keepalived    < - 健康检查子进程
```

**:point_right:内核组件**

Keepalived使用了4个Linux内核组件：

1. LVS框架：使用getsockopt和setsockopt调用来获取和设置套接字上的选项
2. Netfilter框架：支持NAT和Masquerading的IPVS代码
3. Netlink接口：在网络接口上设置和删除VRRP虚拟IP
4. 组播：VRRP通告发送到保留的VRRP组播组(224.0.0.18)

# :point_right:安装keepalived

```Bash
# Redhot/centos
yum install curl gcc openssl-devel libnl3-devel net-snmp-devel
yum install keepalived

# debian /ubuntu
apt-get install curl gcc libssl-dev libnl-3-dev libnl-genl-3-dev libsnmp-dev
apt-get install keepalived
```

> 推荐使用容器

# :point_right:keepalived命令行参数

- -f, –use-file=FILE  	使用指定的配置文件，默认配置文件是“/etc/keepalived/keepalived.conf”。
- -P, –vrrp   只运行VRRP子系统。这对于不使用IPVS负载均衡的配置很有用。
- -C, –check    只运行健康检查子系统。这对于将IPVS负载均衡与单个导向器一起使用而没有故障切 换的配置很有用。
- -l, –log-console    将消息记录到本地控制台。默认行为是将消息记录到syslog。
- -D, –log-detail     详细的日志信息。
- -S, –log-facility=[0-7]    设置syslog设施为LOG_LOCAL [0-7]。默认的syslog设施是LOG_DAEMON。
- -V, –dont-release-vrrp    在守护进程停止时不要删除VRRP VIP和VROUTE。默认行为是在keepalived退出时删除 所有VIP和VROUTE.
- -I, –dont-release-ipvs    在守护进程停止时不要删除IPVS拓扑。默认行为是在keepalived退出时，它将从IPVS 虚拟服务器表中删除所有条目。
- -R, –dont-respawn    不要重新派生子进程。默认行为是如果任一进程退出，就重新启动VRRP和checker子 进程。
- -n, –dont-fork    不要派生守护进程。此选项将导致keepalived在前台运行。
- -d, –dump-conf    转储配置数据。
- -p, –pid=FILE     将指定的pidfile文件用于父级keepalived进程。Keepalived的默认pid文件是 “/var/run/keepalived.pid”。
- -r, –vrrp_pid=FILE    将指定的pidfile文件用于VRRP子进程。VRRP子进程的默认pid文件是 “/var/run/keepalived_vrrp.pid”。
- -c, –checkers_pid=FILE    将指定的pidfile文件用于checker子进程。checker子进程的默认pid文件是 “/var/run/keepalived_checkers.pid”。
- -x, –snmp    启用SNMP子系统。
- -v, –version   显示版本并退出。
- -h, –help    显示此帮助信息并退出。

# **:point_right:genhash实用程序**

genhash二进制文件用于生成只要字符串。genhash的命令行参数如下：

**–use-ssl, -S**使用SSL连接到服务器。**–server <host>, -s**指定要连接的IP地址。**–port <port>, -p**指定要连接的端口。**–****url** **<url>, -u**指定要生成哈希值的文件路径。**–use-virtualhost <host>, -V**指定要与HTTP请求头(host字段)一起发送的虚拟主机**–****hash** **<alg>, -H**指定哈希算法以生成目标页面的摘要。请参阅帮助信息，查看带有默认标记的可用列表。**–verbose, -v**输出详细信息。**–help, -h**显示此帮助信息并退出。**–release, -r**显示版本号并退出。

## :point_right:健康检查

```bash
genhash –s 192.168.100.2 –p 80 –u /testurl/test.jsp
```



# :point_right:**Keepalived配置**

## **:point_right:全局定义概要**

```Bash
global_defs {
    notification_email {
        email
        email
    }
    notification_email_from email
    smtp_server host
    smtp_connect_timeout num
    lvs_id string
}
```

- global_defs        标识全局定义配置块        Block
- notification_email        用于收取邮件通知的电子邮箱        List
- notification_email_from        处理“MAIL FROM:”SMTP命令时使用的电子邮箱        List
- smtp_server        用于发送邮件通知的SMTP服务器        alphanum
- smtp_connection_timeout        指定SMTP流处理的超时时间        numerical
- lvs_id        指定LVS导向器的名字        alphanum

## **:point_right:虚拟服务器定义**

```Bash
virtual_server (@IP PORT)|(fwmark num) {
    delay_loop num
    lb_algo rr|wrr|lc|wlc|sh|dh|lblc
    lb_kind NAT|DR|TUN
    (nat_mask @IP)
    persistence_timeout num
    persistence_granularity @IP
    virtualhost string
    protocol TCP|UDP

    sorry_server @IP PORT
    real_server @IP PORT {
        weight num
        TCP_CHECK {
            connect_port num
            connect_timeout num
        }
    }
    real_server @IP PORT {
        weight num
        MISC_CHECK {
            misc_path /path_to_script/script.sh
            (or misc_path “ /path_to_script/script.sh <arg_list>”)
        }
    }
}
real_server @IP PORT {
    weight num
    HTTP_GET|SSL_GET {
        url { # You can add multiple url block
            path alphanum
            digest alphanum
        }
        connect_port num
        connect_timeout num
        retry num
        delay_before_retry num
    }
}
```

- virtual_server        标识虚拟服务器定义块        Block
- fwmark        指定虚拟服务器是FWMARK         
- delay_loop        以秒为单位指定检查之间的间隔时间        numerical
- lb_algo        选择一个特定的调度程序(rr|wrr|lc|wlc…)        string
- lb_kind        选择一个特定的转发方法(NAT|DR|TUN)        string
- persistence_timeout        为持久连接指定超时时间        numerical
- persistence_granularity        为持久连接指定粒度掩码         
- virtualhost        指定用于HTTP|SSL_GET的虚拟主机        alphanum
- protocol        指定协议类型(TCP|UDP)        numerical
- sorry_server        当所有真实服务器都宕掉时添加到池中的服务器         
- real_server        指定一个真实服务器成员         
- weight        为真实服务器指定负载均衡的权重        numerical
- TCP_CHECK        使用TCP连接检查真实服务器的可用性         
- MISC_CHECK        使用用户定义的脚本检查真实服务器的可用性         
- misc_path        标识要运行脚本的完整路径        path
- HTTP_GET        使用HTTP GET请求检查真实服务器的可用性         
- SSL_GET        使用HTTPS GET请求检查真实服务器的可用性         
- url        标识url定义块        Block
- path        指定url路径        alphanum
- digest        指定特定url路径的摘要        alphanum
- connect_port        指定连接远程服务器的TCP端口        numerical
- connect_timeout        指定连接远程服务器的超时时间        numerical
- retry        最大重试次数        numerical
- delay_before_retry        两次连续重试之间的延迟        numerical

## :point_right:**VRRP实例**

```Bash
vrrp_sync_group string {
    group {
        string
        string
    }
    notify_master /path_to_script/script_master.sh
        (or notify_master “ /path_to_script/script_master.sh <arg_list>”)
    notify_backup /path_to_script/script_backup.sh
        (or notify_backup “/path_to_script/script_backup.sh <arg_list>”)
    notify_fault /path_to_script/script_fault.sh
        (or notify_fault “ /path_to_script/script_fault.sh <arg_list>”)
}
vrrp_instance string {
    state MASTER|BACKUP
    interface string
    mcast_src_ip @IP
    lvs_sync_daemon_interface string
    virtual_router_id num
    priority num
    advert_int num
    smtp_alert
    authentication {
        auth_type PASS|AH
        auth_pass string
    }
    virtual_ipaddress { # Block limited to 20 IP addresses
        @IP
        @IP
        @IP
    }
    virtual_ipaddress_excluded { # Unlimited IP addresses
        @IP
        @IP
        @IP
    }
    notify_master /path_to_script/script_master.sh
        (or notify_master “ /path_to_script/script_master.sh <arg_list>”)
    notify_backup /path_to_script/script_backup.sh
        (or notify_backup “ /path_to_script/script_backup.sh <arg_list>”)
    notify_fault /path_to_script/script_fault.sh
        (or notify_fault “ /path_to_script/script_fault.sh <arg_list>”)
}
```

- vrrp_instance        标识VRRP实例定义块        Block
- state        在标准使用中指定实例状态         
- interface        指定实例运行所要用到的网络接口        string
- mcast_src_ip        指定VRRP通告的IP头的源地址         
- lvs_sync_daemon_inteface        指定LVS sync_daemon运行所要用到的网络接口        string
- virtual_router_id        指定实例所属的VRRP路由器ID        numerical
- priority        指定实例在VRRP路由器中的优先级        numerical
- advert_int        以秒为单位指定通告的间隔时间(设置为1)        numerical
- smtp_alert        激活MASTER状态转换的SMTP通知         
- authentication        标识VRRP认证定义块        Block
- auth_type        指定要使用哪种身份认证(PASS|AH)         
- auth_pass        指定要使用的密码字符串        string
- virtual_ipaddress        标识VRRP VIP定义块        Block
- virtual_ipaddress_excluded        标识VRRP VIP排除定义块        Block
- notify_master        指定在切换到master时要执行的脚本        path
- notify_backup        指定在切换到backup时要执行的脚本        path
- notify_fault        指定在切换到故障状态时要执行的脚本        path
- vrrp_sync_group        标识VRRP同步组定义块        Block

# **:point_right:IPVS调度算法**

## :point_right:轮转(Round Robin,rr)

轮转调度算法将每个传入请求发送到其列表中的下一个服务器。因此，在三服务器集群（服务器A,B和C）中，请求1将转到服务器A，请求2将转到服务器B，请求3将转到服务器C，请求4将转到服务器A，这样就完成了服务器的循环或“轮转”。

## :point_right:加权轮转(Weighted Round Robin, wrr)

加权轮转调度旨在更好地处理具有不同处理能力的服务器。可以为每个服务器分配一个权重，权重就是用来表示处理能力的整数值。

真实服务器A,B和C分别具有权重4,3,2，在调度周期（`mod sum(Wi)`）中，良好的调度序列将是 `AABABCABC`。

## :point_right:最少连接(Least Connection,lc)

最少连接调度算法将网络连接定向到具有最少数量的已建立连接的服务器。这是动态调度算法之一，因为它需要动态计算每个服务器的实时连接。

## :point_right:加权最少连接(Weighted Least Connection, wlc)

加权最少连接调度是最少连接调度的超集，您可以在其中为每个真实服务器分配性能权重。具有较高权重值的服务器在任何时候都将获得更大比例的实时连接。

## 基于局部性的最少连接(Locality-Based Least Connection, lblc)

基于局部性的最少连接调度算法用于目标IP负载均衡。它通常用于缓存集群。如果服务器处于活动状态且负载不足，那么此算法通常会将发往IP地址的数据包定向到其所在服务器。

## 带复制的基于局部性最少连接(Locality-Based Least Connection with Replication, lblcr)

带复制的基于局部性最少连接调度算法也用于目标IP负载均衡。它通常用于缓存集群。它与 `LBLC` 调度的区别在于：负载均衡器需要维护从一个目标IP地址到一组服务器的映射，而LBLC调度维护从一个目标IP地址到一台服务器的映射。

## :point_right:目标地址哈希(Destination Hashing, dh)

目标地址哈希调度算法根据请求的目标IP地址查找静态分配的哈希表来为服务器分配网络连接。

## :point_right:源地址哈希(Source Hashing, sh)

源地址哈希调度算法根据请求的源IP地址查找静态分配的哈希表来为服务器分配网络连接。

## 最短预期延迟(Shortest Expected Delay, seq)

最短预期延迟调度算法以最短的预期延迟将网络连接分配给服务器。如果发送到第i个服务器，作业将经历的预期延迟为 `(Ci + 1)/ Ui`，其中Ci是第i个服务器上的连接数，Ui是第i个服务器的固定服务速率（权重）。

## **永不排队(Never** Queue, nq)

永不排队调度算法采用双速模型。当有空闲服务器可用时，作业将被发送到空闲服务器，而不是等待快速服务器。当没有空闲服务器时，作业将被发送到最短预期延迟的服务器（最短预期延迟调度算法）。

## 溢出连接(Overflow-Connection, ovf)

溢出连接调度算法根据活动连接数实现“溢出”负载均衡，将所有连接发送到具有最高权重的节点，并且如果连接数超过节点的权重，就溢出到下一个节点。请注意，**此调度算法可能不适用于****UDP****，因为它仅使用活动连接**。