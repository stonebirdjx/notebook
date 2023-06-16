<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [文件存储、块存储、对象存储、分布式存储](#%E6%96%87%E4%BB%B6%E5%AD%98%E5%82%A8%E5%9D%97%E5%AD%98%E5%82%A8%E5%AF%B9%E8%B1%A1%E5%AD%98%E5%82%A8%E5%88%86%E5%B8%83%E5%BC%8F%E5%AD%98%E5%82%A8)
- [4CS程序员](#4cs%E7%A8%8B%E5%BA%8F%E5%91%98)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 文件存储、块存储、对象存储、分布式存储

**文件存储**

文件存储也称为文件级存储或基于文件的存储，且正如您所想：数据会以单条信息的形式存储在文件夹中。

以文件为单位（如常见目录下的文件）。如**NFS，FTP**等。

**块存储**

块存储会将数据拆分成块，并单独存储各个块。每个数据块都有一个唯一标识符，所以存储系统能将较小的数据存放在最方便的位置。

文件系统在客户端，如**数据库**

**对象存储**

对象存储端的文件系统就是采用这种哈希表-键值（可以理解为查字典，最多两层目录）这种方式来提高读写速度的。

对象存储需要一个简单的 HTTP应用编程接口（API），以供大多数客户端（各种语言）使用。

非常适合于存储大容量非结构化的数据，例如图片、视频、日志文件、备份数据和容器/虚拟机镜像等,如**网盘**

> 对象存储注意解决共享问题

**分布式存储**

是将数据分散存储在多台独立的设备上提高了系统的可靠性、可用性和存取效率，还易于扩展。

一般分布式存储会包含文件存储、块儿存储、对象存储。如**ceph**。

# 4CS程序员

4CS程序员指的是cloud、cluster、container、code safe 的高级程序员