<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [简介](#%E7%AE%80%E4%BB%8B)
- [最低硬件和操作系统要求](#%E6%9C%80%E4%BD%8E%E7%A1%AC%E4%BB%B6%E5%92%8C%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F%E8%A6%81%E6%B1%82)
- [安装ceph](#%E5%AE%89%E8%A3%85ceph)
  - [cephadmin](#cephadmin)
    - [配置hosts](#%E9%85%8D%E7%BD%AEhosts)
    - [安装cephadmin](#%E5%AE%89%E8%A3%85cephadmin)
    - [引导新集群](#%E5%BC%95%E5%AF%BC%E6%96%B0%E9%9B%86%E7%BE%A4)
    - [cephadm shell](#cephadm-shell)
    - [**添加主机到集群**](#%E6%B7%BB%E5%8A%A0%E4%B8%BB%E6%9C%BA%E5%88%B0%E9%9B%86%E7%BE%A4)
    - [创建mon和mgr](#%E5%88%9B%E5%BB%BAmon%E5%92%8Cmgr)
    - [部署OSD](#%E9%83%A8%E7%BD%B2osd)
    - [**部署MDS**](#%E9%83%A8%E7%BD%B2mds)
    - [部署RGW](#%E9%83%A8%E7%BD%B2rgw)
    - [cephadm使用帮助](#cephadm%E4%BD%BF%E7%94%A8%E5%B8%AE%E5%8A%A9)
  - [:point_right:ceph-ansible -- 推荐使用](#point_rightceph-ansible----%E6%8E%A8%E8%8D%90%E4%BD%BF%E7%94%A8)
  - [手动安装](#%E6%89%8B%E5%8A%A8%E5%AE%89%E8%A3%85)
- [文件系统](#%E6%96%87%E4%BB%B6%E7%B3%BB%E7%BB%9F)
- [:point_right:块设备](#point_right%E5%9D%97%E8%AE%BE%E5%A4%87)
- [:point_right:对象存储](#point_right%E5%AF%B9%E8%B1%A1%E5%AD%98%E5%82%A8)
- [rock-ceph](#rock-ceph)
- [常用命令](#%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4)
  - [:point_right:管理进程](#point_right%E7%AE%A1%E7%90%86%E8%BF%9B%E7%A8%8B)
  - [:point_right:检查集群健康状态](#point_right%E6%A3%80%E6%9F%A5%E9%9B%86%E7%BE%A4%E5%81%A5%E5%BA%B7%E7%8A%B6%E6%80%81)
  - [:point_right:容量使用情况](#point_right%E5%AE%B9%E9%87%8F%E4%BD%BF%E7%94%A8%E6%83%85%E5%86%B5)
  - [设置PG数量](#%E8%AE%BE%E7%BD%AEpg%E6%95%B0%E9%87%8F)
  - [查看PG信息](#%E6%9F%A5%E7%9C%8Bpg%E4%BF%A1%E6%81%AF)
  - [集群PG统计信息](#%E9%9B%86%E7%BE%A4pg%E7%BB%9F%E8%AE%A1%E4%BF%A1%E6%81%AF)
  - [:point_right:Monitor状态](#point_rightmonitor%E7%8A%B6%E6%80%81)
  - [:point_right:新增mon节点](#point_right%E6%96%B0%E5%A2%9Emon%E8%8A%82%E7%82%B9)
  - [:point_right:删除mon节点](#point_right%E5%88%A0%E9%99%A4mon%E8%8A%82%E7%82%B9)
  - [:point_right:OSD状态](#point_rightosd%E7%8A%B6%E6%80%81)
  - [OSD配置](#osd%E9%85%8D%E7%BD%AE)
  - [:point_right:新增OSD节点](#point_right%E6%96%B0%E5%A2%9Eosd%E8%8A%82%E7%82%B9)
  - [:point_right:删除OSD](#point_right%E5%88%A0%E9%99%A4osd)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 简介

为云平台提供Ceph 对象存储和/或 Ceph 块设备服务、部署Ceph 文件系统还是将 Ceph 用于其他目的，所有 Ceph 存储集群部署都从设置每个 Ceph 节点、您的网络和 Ceph存储集群。一个 Ceph 存储集群至少需要一个 Ceph Monitor、Ceph Manager 和 Ceph OSD（Object Storage Daemon）。运行 Ceph 文件系统客户端时也需要 Ceph 元数据服务器。

**Monitors**: Ceph Monitor ( `ceph-mon`) 维护集群状态图，包括监视器图、管理器图、OSD 图、MDS 图和 CRUSH 图。这些映射是 Ceph 守护进程相互协调所需的关键集群状态。监视器还负责管理守护进程和客户端之间的身份验证。通常至少需要三个监视器才能实现冗余和高可用性。

**Manager**: Ceph 管理器守护进程 ( `ceph-mgr`) 负责跟踪运行时指标和 Ceph 集群的当前状态，包括存储利用率、当前性能指标和系统负载。Ceph Manager 守护进程还托管基于 python 的模块来管理和公开 Ceph 集群信息，包括基于 Web 的Ceph Dashboard和 REST API。高可用性通常至少需要两个管理器。

**Ceph** **OSDs**: 一个对象存储守护进程（Ceph OSD， `ceph-osd`）存储数据，处理数据复制，恢复，重新平衡，并通过检查其他 Ceph OSD 守护进程的心跳向 Ceph Monitors 和 Managers 提供一些监控信息。通常至少需要三个 Ceph OSD 才能实现冗余和高可用性。

**MDS**：Ceph 元数据服务器（MDS `ceph-mds`）代表Ceph 文件系统存储元数据（即 Ceph 块设备和 Ceph 对象存储不使用 MDS）。Ceph 元数据服务器允许 POSIX 文件系统用户执行基本命令（如 ls、find等），而不会给 Ceph 存储集群带来巨大负担。

Ceph 将数据作为对象存储在逻辑存储池中。使用 `CRUSH`算法，Ceph 计算出哪个归置组 (PG) 应该包含该对象，以及哪个 OSD 应该存储该归置组。CRUSH 算法使 Ceph 存储集群能够动态扩展、重新平衡和恢复。

> Ceph 能够提供企业中常见的三种存储需求：对象存储、块存储、文件存储

# 最低硬件和操作系统要求

| 模块 | CPU  | 内存 | 磁盘 |
| ---- | ---- | ---- | ---- |
| osd  | 1+   | 4G+  | 1    |
| mon  | 2    | 4G+  | 60GB |
| Mds  | 2    | 2G+  |      |

> 可构建和维护 PB 级数据，每进程每 TB 数据需要约 1GB 内存
>
> 一般一块硬盘一个OSD，我们建议用容量大于 1TB 的硬盘，一般使用裸盘 `lsblk -f   块设备的FSTYPE 为空`
>
> 一般一块硬盘一个mds
>
> 操作系统推荐centos

# 安装ceph

## cephadmin

需要`docker`和`python3`

### 配置hosts

```Bash
# cat /etc/hosts
192.168.174.200 cephadm-deploy
192.168.174.103 ceph-node01
192.168.174.104 ceph-node02
192.168.174.105 ceph-node03
192.168.174.120 ceph-node04

# 分别设置主机名
hostnamectl set-hostname node1
hostnamectl set-hostname node2
hostnamectl set-hostname node3

# 关闭防火墙 和 selinux
systemctl stop firewalld && systemctl disable firewalld
setenforce 0 && sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config

# 主机同步
systemctl restart chronyd.service && systemctl enable chronyd.service

# 安装docker
yum -y install docker-ce 
systemctl restart docker
systemctl enable docker
```

### 安装cephadmin

```Bash
curl --silent --remote-name --location https://github.com/ceph/ceph/raw/pacific/src/cephadm/cephadm

chmod +x cephadm

./cephadm add-repo --release pacific

./cephadm install
```

### 引导新集群

```Bash
mkdir -p /etc/ceph

# bootstrap
cephadm bootstrap --mon-ip 192.168.2.16

# 可以在节点上安装包含所有 ceph 命令的包，包括 、（用于安装 CephFS 文件系统）等
cephadm add-repo --release octopus
cephadm install ceph-common
```

- 为本地主机上的新群集创建monitor和manager守护程序。
- 为 Ceph 群集生成新的 SSH 密钥，并将其添加到root用户的文件`/root/.ssh/authorized_keys`
- 将与新群集通信所需的最小配置文件保存到 `/etc/ceph/ceph.conf`
- 将`client.admin`管理（特权！）密钥的副本写入`/etc/ceph/ceph.client.admin.keyring`
- 将公钥的副本写入`/etc/ceph/ceph.pub`

 写在 `/ete/ceph/ceph.conf`

### cephadm shell

cephadm shell命令在安装了所有Ceph包的容器中启动bash shell。

```Bash
ceph -s
```

### **添加主机到集群**

```Bash
# 将公钥添加到新主机
ssh-copy-id -f -i /etc/ceph/ceph.pub node2
ssh-copy-id -f -i /etc/ceph/ceph.pub node3


# 加入新节点
ceph orch host add node2
ceph orch host add node3


# 查看主机和标签
ceph orch host ls
```

### 创建mon和mgr

```Bash
ceph orch apply mon --placement="3 ceph-1 ceph-2 ceph-3"
ceph orch apply mgr --placement="3 ceph-1 ceph-2 ceph-3"

ceph orch ls 
```

### 部署OSD

- 设备必须没有分区。
- 设备不得具有任何 LVM 状态。
- 不得安装设备。
- 设备不能包含文件系统。
- 设备不得包含 Ceph BlueStore OSD。
- 设备必须大于 5 GB。

```Bash
# 显示集群中的存储设备清单
ceph orch device ls

# 在未使用的设备上自动创建osd
ceph orch apply osd --all-available-devices

# 从特定主机上的特定设备创建 OSD
# ceph orch daemon add osd host1:/dev/sdb
```

### **部署MDS**

```Bash
ceph orch apply mds *<fs-name>* --placement="*<num-daemons>* [*<host1>* ...]"

# CephFS 需要两个 Pools，cephfs-data 和 cephfs-metadata,分别存储文件数据和文件元数据
ceph osd pool create cephfs_data 64 64
ceph osd pool create cephfs_metadata 64 64

ceph fs new cephfs cephfs_metadata cephfs_data
ceph orch apply mds cephfs --placement="3 node1 node2 node3"

# 检查fs
ceph fs status cephfs
```

### 部署RGW

RGW是Ceph对象存储网关服务RADOS Gateway的简称，是一套基于LIBRADOS接口封装而实现的FastCGI服务，对外提供RESTful风格的对象存储数据访问和管理接口。

```Bash
ceph orch apply rgw myorg cn-east-1 --placement="3 node1 node2 node3"
```

### cephadm使用帮助

```Bash
~# cephadm -h
usage: cephadm [-h] [--image IMAGE] [--docker] [--data-dir DATA_DIR] [--log-dir LOG_DIR] [--logrotate-dir LOGROTATE_DIR] [--sysctl-dir SYSCTL_DIR] [--unit-dir UNIT_DIR] [--verbose] [--timeout TIMEOUT]
               [--retry RETRY] [--env ENV] [--no-container-init]
               {version,pull,inspect-image,ls,list-networks,adopt,rm-daemon,rm-cluster,run,shell,enter,ceph-volume,zap-osds,unit,logs,bootstrap,deploy,check-host,prepare-host,add-repo,rm-repo,install,registry-login,gather-facts,exporter,host-maintenance}
               ...

Bootstrap Ceph daemons with systemd and containers.

positional arguments:

{version,pull,inspect-image,ls,list-networks,adopt,rm-daemon,rm-cluster,run,shell,enter,ceph-volume,zap-osds,unit,logs,bootstrap,deploy,check-host,prepare-host,add-repo,rm-repo,install,registry-login,gather-facts,exporter,host-maintenance}

sub-command
version             #从容器中获取ceph版本
pull                #pull 最新 image版本
inspect-image       #检查本地image
ls                  #列出此主机上的守护程序实例
list-networks       #列出IP网络
adopt               #采用使用不同工具部署的守护程序
rm-daemon           #删除守护程序实例
rm-cluster          #删除群集的所有守护进程
run                 #在容器中，在前台运行ceph守护进程
shell               #在守护进程容器中运行交互式shell
enter               #在运行的守护程序容器中运行交互式shell
ceph-volume         #在容器内运行ceph volume
zap-osds            #zap与特定fsid关联的所有OSD
unit                #操作守护进程的systemd单元
logs                #打印守护程序容器的日志
bootstrap           #引导群集（mon+mgr守护进程）
deploy              #部署守护进程
check-host          #检查主机配置
prepare-host        #准备主机供cephadm使用
add-repo            #配置包存储库
rm-repo             #删除包存储库配置
install             #安装ceph软件包
registry-login      #将主机登录到经过身份验证的注册表
gather-facts        #收集并返回主机相关信息（JSON格式）
exporter            #在exporter模式（web服务）下启动cephadm，提供主机/守护程序/磁盘元数据
host-maintenance    #管理主机的维护状态
optional arguments:
-h, --help            #显示此帮助消息并退出
--image IMAGE         #container image. 也可以通过“CEPHADM_IMAGE”环境变量进行设置（默认值：无）
--docker              #使用docker而不是podman（默认值：False）
--data-dir DATA_DIR   #守护程序数据的基本目录（默认值：/var/lib/ceph）
--log-dir LOG_DIR     #守护程序日志的基本目录（默认值：/var/log/ceph）
--logrotate-dir LOGROTATE_DIR
#logrotate配置文件的位置（默认值：/etc/logrotate.d）
--sysctl-dir SYSCTL_DIR
#sysctl配置文件的位置（默认值：/usr/lib/sysctl.d）
--unit-dir UNIT_DIR   #systemd装置的基本目录（默认值：/etc/systemd/system）
--verbose, -v         #显示调试级别日志消息（默认值：False）
--timeout TIMEOUT     #以秒为单位的超时（默认值：None）
--retry RETRY         #最大重试次数（默认值：15）
--env ENV, -e ENV     #设置环境变量（默认值：[]）
--no-container-init   #不使用“---init”运行podman/docker（默认值：False）
```

## :point_right:ceph-ansible -- 推荐使用

## 手动安装

https://docs.ceph.com/en/quincy/install/index_manual/#install-manual

安装安装一个管理节点和三个存储节点

# 文件系统

https://docs.ceph.com/en/quincy/cephfs/#getting-started-with-cephfs

对于大多数 Ceph 部署,设置 CephFS 文件系统非常简单

```Bash
# ceph osd pool create cephfs_data
# ceph osd pool create cephfs_metadata
# ceph fs volume create <fs name>

ceph fs new cephfs cephfs_metadata cephfs_data
# 创建文件系统后，您的 MDS 将能够进入活动状态。
ceph mds stat
```

# :point_right:块设备

```Bash
RDB块设备扩容
rbd create -p ceph-demo --image rbd-demo.img --size 10G
```

# :point_right:对象存储

[Ceph](https://docs.ceph.com/en/latest/) is a distributed network storage and file system with distributed metadata management and POSIX semantics. See also the [Ceph Glossary](https://docs.ceph.com/en/latest/glossary/). Here are a few of the important terms to understand:

- [Ceph Monitor](https://docs.ceph.com/en/latest/glossary/#term-Ceph-Monitor) (MON)
- [Ceph Manager](https://docs.ceph.com/en/latest/glossary/#term-Ceph-Manager) (MGR)
- [Ceph Metadata Server](https://docs.ceph.com/en/latest/glossary/#term-MDS) (MDS)
- [Object Storage Device](https://docs.ceph.com/en/latest/glossary/#term-OSD) (OSD)
- [RADOS Block Device](https://docs.ceph.com/en/latest/glossary/#term-Ceph-Block-Device) (RBD)
- [Ceph Object Gateway](https://docs.ceph.com/en/latest/glossary/#term-Ceph-Object-Gateway) (RGW)

# rock-ceph

牛逼

# 常用命令

## :point_right:管理进程

*所有类型的进程进行启动、关闭、重启等操作目的*

```Bash
# 未使用"-a"选项,以上命令只会对当前节点内的守护进程生效
ceph [-a] start|stop|restart

# 管理osd进程
ceph restart osd
```

## :point_right:检查集群健康状态

```Bash
ceph health [detail] # 可以加上"detail"选项帮助排查问题

# 集群概况
ceph -s
ceph -w

# 容量使用情况
```

## :point_right:容量使用情况

```Bash
ceph df
```

## 设置PG数量

要设置某个POOL的PG数量(pg_num),必须在创建POOL时便指定

```Bash
# ceph osd pool create "pool-name" pg_num [pgp_num]
sudo ceph osd pool create image 256 256
```

在后续增加PG数量时,还必须增加用于归置PG的PGP数量(pgp_num),PGP的数量应该与PG的数量相等。但在新增POOL时可以不指定pgp_num,默认会与pg_num保持一致。

```Bash
# ceph osd pool set "pool-name" pg_num [pgp_num
sudo ceph osd pool set image 512 512
```

## 查看PG信息

```Bash
# ceph osd pool get "pool-name" pg_num/pgp_num
ceph osd pool get image pg_num
ceph osd pool get image pgp_num
```

## 集群PG统计信息

```Bash
sudo ceph pg dump [--format json]

# 取状态不正常的PG的状态
ceph pg dump_stuck  inactive|unclean|stale|undersized|degraded [--format <format>]
```

## :point_right:Monitor状态

```Bash
ceph mon stat

# 进一步了解monmap
ceph mon dump

# 监视器
ceph quorum_status
```

## :point_right:新增mon节点

```Bash
# ceph-deploy mon create {host-name [host-name]...}
ceph-deploy mon create newhostname
```

## :point_right:删除mon节点

```Bash
# ceph-deploy mon destroy {host-name [host-name]...}
ceph-deploy mon destroy oldhostname
```

## :point_right:OSD状态

```Bash
ceph osd stat
```

## OSD配置

```Bash
ceph osd df

ceph osd tree

# 概况
ceph osd dump
```

## :point_right:新增OSD节点

```Bash
# 需要ceph-deploy 没有请安装，yum install ceph-deploy
# 编辑/etc/hosts目录，将新增节点的主机名、IP
# 在新增节点上安装ceph软件 yum -y install ceph redhat-lsb

# 推送相关密钥及配置文件至新增节点
ceph-deploy admin new-node

# 创建集群关系key。
ceph-deploy gatherkeys new-node

# 检查新增OSD节点的磁盘
ceph-deploy disk list new-node

# 创建所要新增OSD节点上的osd
ceph-deploy osd create new-node:new-disk

# 少数情况下，需要手动激活新增的osd后，集群才能正常识别新增的osd。
# ceph-disk activate-all
```

## :point_right:删除OSD

```Bash
# 停止目标osd
ceph stop osd.x

# 将该osd的集群标记为out
ceph osd out osd.x

# 将该osd从Ceph crush中移除
ceph osd crush remove osd.x

# 从集群中完全删除该osd的记录。
ceph osd rm osd.x

# 删除该osd的认证信息
ceph auth del osd.x
ceph -s 查看集群状态
ceph osd status 查看osd状态
ceph pg stat 查看pg状态
ceph osd pool set pool pg_num 64 设置pg数量
ceph osd pool set pool pgp_num 64 设置pgp数量，在集群规模较小，pg数量过少会导致监控警告，此两条命令需一起使用
```