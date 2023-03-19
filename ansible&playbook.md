<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [简介](#%E7%AE%80%E4%BB%8B)
- [安装](#%E5%AE%89%E8%A3%85)
- [:point_right:inventory 配置信息](#point_rightinventory-%E9%85%8D%E7%BD%AE%E4%BF%A1%E6%81%AF)
  - [变量](#%E5%8F%98%E9%87%8F)
  - [把一个组作为另一个组的子成员](#%E6%8A%8A%E4%B8%80%E4%B8%AA%E7%BB%84%E4%BD%9C%E4%B8%BA%E5%8F%A6%E4%B8%80%E4%B8%AA%E7%BB%84%E7%9A%84%E5%AD%90%E6%88%90%E5%91%98)
  - [:point_right:示例](#point_right%E7%A4%BA%E4%BE%8B)
- [:point_right:ansible命令行参数](#point_rightansible%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%8F%82%E6%95%B0)
- [常用模块](#%E5%B8%B8%E7%94%A8%E6%A8%A1%E5%9D%97)
  - [:point_right:ping - 测主机连通性](#point_rightping---%E6%B5%8B%E4%B8%BB%E6%9C%BA%E8%BF%9E%E9%80%9A%E6%80%A7)
  - [:point_right:raw、shell、command - 远程执行命令](#point_rightrawshellcommand---%E8%BF%9C%E7%A8%8B%E6%89%A7%E8%A1%8C%E5%91%BD%E4%BB%A4)
  - [:point_right:yum - RedHat 和 CentOS远程安装或卸载程序包](#point_rightyum---redhat-%E5%92%8C-centos%E8%BF%9C%E7%A8%8B%E5%AE%89%E8%A3%85%E6%88%96%E5%8D%B8%E8%BD%BD%E7%A8%8B%E5%BA%8F%E5%8C%85)
  - [apt - debian-ubuntu 远程安装程序包](#apt---debian-ubuntu-%E8%BF%9C%E7%A8%8B%E5%AE%89%E8%A3%85%E7%A8%8B%E5%BA%8F%E5%8C%85)
  - [pip - 管理python的依赖](#pip---%E7%AE%A1%E7%90%86python%E7%9A%84%E4%BE%9D%E8%B5%96)
  - [**:point_right:synchronize - 使用rsync同步文件，将主控方目录推送到指定受控节点的目录下**](#point_rightsynchronize---%E4%BD%BF%E7%94%A8rsync%E5%90%8C%E6%AD%A5%E6%96%87%E4%BB%B6%E5%B0%86%E4%B8%BB%E6%8E%A7%E6%96%B9%E7%9B%AE%E5%BD%95%E6%8E%A8%E9%80%81%E5%88%B0%E6%8C%87%E5%AE%9A%E5%8F%97%E6%8E%A7%E8%8A%82%E7%82%B9%E7%9A%84%E7%9B%AE%E5%BD%95%E4%B8%8B)
  - [:point_right:copy - 拷贝文件和目录模块](#point_rightcopy---%E6%8B%B7%E8%B4%9D%E6%96%87%E4%BB%B6%E5%92%8C%E7%9B%AE%E5%BD%95%E6%A8%A1%E5%9D%97)
  - [:point_right:cron - 定时任务模块](#point_rightcron---%E5%AE%9A%E6%97%B6%E4%BB%BB%E5%8A%A1%E6%A8%A1%E5%9D%97)
  - [debug - 运行打印变量值](#debug---%E8%BF%90%E8%A1%8C%E6%89%93%E5%8D%B0%E5%8F%98%E9%87%8F%E5%80%BC)
  - [file - 用于设定或修改文件的属性信息](#file---%E7%94%A8%E4%BA%8E%E8%AE%BE%E5%AE%9A%E6%88%96%E4%BF%AE%E6%94%B9%E6%96%87%E4%BB%B6%E7%9A%84%E5%B1%9E%E6%80%A7%E4%BF%A1%E6%81%AF)
  - [:point_right:setup - 获取远程主机信息](#point_rightsetup---%E8%8E%B7%E5%8F%96%E8%BF%9C%E7%A8%8B%E4%B8%BB%E6%9C%BA%E4%BF%A1%E6%81%AF)
  - [:point_right:stat - 返回文件属性](#point_rightstat---%E8%BF%94%E5%9B%9E%E6%96%87%E4%BB%B6%E5%B1%9E%E6%80%A7)
  - [:point_right:service、systemd - 系统服务管理](#point_rightservicesystemd---%E7%B3%BB%E7%BB%9F%E6%9C%8D%E5%8A%A1%E7%AE%A1%E7%90%86)
  - [timezone - 同步时区](#timezone---%E5%90%8C%E6%AD%A5%E6%97%B6%E5%8C%BA)
  - [unarchive - 解压压缩包](#unarchive---%E8%A7%A3%E5%8E%8B%E5%8E%8B%E7%BC%A9%E5%8C%85)
  - [:point_right:user - 用户管理](#point_rightuser---%E7%94%A8%E6%88%B7%E7%AE%A1%E7%90%86)
  - [:point_right:group - 组管理](#point_rightgroup---%E7%BB%84%E7%AE%A1%E7%90%86)
  - [:point_right:wait_for - 在规定时间内检测](#point_rightwait_for---%E5%9C%A8%E8%A7%84%E5%AE%9A%E6%97%B6%E9%97%B4%E5%86%85%E6%A3%80%E6%B5%8B)
- [:point_right:playbook](#point_rightplaybook)
  - [:point_right:项目结构组成](#point_right%E9%A1%B9%E7%9B%AE%E7%BB%93%E6%9E%84%E7%BB%84%E6%88%90)
    - [playbook.yml示例](#playbookyml%E7%A4%BA%E4%BE%8B)
    - [:point_right:语法检查](#point_right%E8%AF%AD%E6%B3%95%E6%A3%80%E6%9F%A5)
  - [:point_right:变量](#point_right%E5%8F%98%E9%87%8F)
    - [:point_right:定义变量](#point_right%E5%AE%9A%E4%B9%89%E5%8F%98%E9%87%8F)
    - [:point_right:使用变量](#point_right%E4%BD%BF%E7%94%A8%E5%8F%98%E9%87%8F)
    - [:point_right:使用Facts获取的信息](#point_right%E4%BD%BF%E7%94%A8facts%E8%8E%B7%E5%8F%96%E7%9A%84%E4%BF%A1%E6%81%AF)
    - [:point_right:注册变量](#point_right%E6%B3%A8%E5%86%8C%E5%8F%98%E9%87%8F)
    - [变量文件](#%E5%8F%98%E9%87%8F%E6%96%87%E4%BB%B6)
    - [:point_right:命令行中传递变量](#point_right%E5%91%BD%E4%BB%A4%E8%A1%8C%E4%B8%AD%E4%BC%A0%E9%80%92%E5%8F%98%E9%87%8F)
  - [:point_right:role 和 include 重用yml文件](#point_rightrole-%E5%92%8C-include-%E9%87%8D%E7%94%A8yml%E6%96%87%E4%BB%B6)
    - [:point_right:include](#point_rightinclude)
    - [:point_right:handler中include](#point_righthandler%E4%B8%ADinclude)
    - [include 引用 playbook的yml](#include-%E5%BC%95%E7%94%A8-playbook%E7%9A%84yml)
    - [Roles](#roles)
      - [:point_right:Roles项目结构](#point_rightroles%E9%A1%B9%E7%9B%AE%E7%BB%93%E6%9E%84)
      - [:point_right:使用参数化的 roles](#point_right%E4%BD%BF%E7%94%A8%E5%8F%82%E6%95%B0%E5%8C%96%E7%9A%84-roles)
      - [:point_right:when设置触发条件](#point_rightwhen%E8%AE%BE%E7%BD%AE%E8%A7%A6%E5%8F%91%E6%9D%A1%E4%BB%B6)
      - [分配tag](#%E5%88%86%E9%85%8Dtag)
  - [:point_right:条件选择](#point_right%E6%9D%A1%E4%BB%B6%E9%80%89%E6%8B%A9)
    - [:point_right:某个条件时执行](#point_right%E6%9F%90%E4%B8%AA%E6%9D%A1%E4%BB%B6%E6%97%B6%E6%89%A7%E8%A1%8C)
    - [**加载客户事件**](#%E5%8A%A0%E8%BD%BD%E5%AE%A2%E6%88%B7%E4%BA%8B%E4%BB%B6)
    - [**:point_right:在roles 和 includes 上面应用’when’语句**](#point_right%E5%9C%A8roles-%E5%92%8C-includes-%E4%B8%8A%E9%9D%A2%E5%BA%94%E7%94%A8when%E8%AF%AD%E5%8F%A5)
  - [循环体](#%E5%BE%AA%E7%8E%AF%E4%BD%93)
    - [:point_right:标准循环](#point_right%E6%A0%87%E5%87%86%E5%BE%AA%E7%8E%AF)
    - [:point_right:嵌套循环](#point_right%E5%B5%8C%E5%A5%97%E5%BE%AA%E7%8E%AF)
    - [:point_right:对map的循环](#point_right%E5%AF%B9map%E7%9A%84%E5%BE%AA%E7%8E%AF)
    - [对文件列表使用循环](#%E5%AF%B9%E6%96%87%E4%BB%B6%E5%88%97%E8%A1%A8%E4%BD%BF%E7%94%A8%E5%BE%AA%E7%8E%AF)
    - [对并行数据集使用循环](#%E5%AF%B9%E5%B9%B6%E8%A1%8C%E6%95%B0%E6%8D%AE%E9%9B%86%E4%BD%BF%E7%94%A8%E5%BE%AA%E7%8E%AF)
    - [对子元素使用循环](#%E5%AF%B9%E5%AD%90%E5%85%83%E7%B4%A0%E4%BD%BF%E7%94%A8%E5%BE%AA%E7%8E%AF)
    - [对整数序列使用循环](#%E5%AF%B9%E6%95%B4%E6%95%B0%E5%BA%8F%E5%88%97%E4%BD%BF%E7%94%A8%E5%BE%AA%E7%8E%AF)
    - [随机选择](#%E9%9A%8F%E6%9C%BA%E9%80%89%E6%8B%A9)
    - [Do - until 循环](#do---until-%E5%BE%AA%E7%8E%AF)
    - [查找第一个匹配的文件](#%E6%9F%A5%E6%89%BE%E7%AC%AC%E4%B8%80%E4%B8%AA%E5%8C%B9%E9%85%8D%E7%9A%84%E6%96%87%E4%BB%B6)
    - [迭代程序的执行结果](#%E8%BF%AD%E4%BB%A3%E7%A8%8B%E5%BA%8F%E7%9A%84%E6%89%A7%E8%A1%8C%E7%BB%93%E6%9E%9C)
    - [使用索引循环列表](#%E4%BD%BF%E7%94%A8%E7%B4%A2%E5%BC%95%E5%BE%AA%E7%8E%AF%E5%88%97%E8%A1%A8)
    - [循环配置文件](#%E5%BE%AA%E7%8E%AF%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6)
  - [最佳实践](#%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5)
    - [:point_right:优秀的项目结构](#point_right%E4%BC%98%E7%A7%80%E7%9A%84%E9%A1%B9%E7%9B%AE%E7%BB%93%E6%9E%84)
- [Playbooks: Special Topics](#playbooks-special-topics)
  - [:point_right:加速模式](#point_right%E5%8A%A0%E9%80%9F%E6%A8%A1%E5%BC%8F)
  - [:point_right:异步操作和轮询](#point_right%E5%BC%82%E6%AD%A5%E6%93%8D%E4%BD%9C%E5%92%8C%E8%BD%AE%E8%AF%A2)
  - [测试运行 check mode](#%E6%B5%8B%E8%AF%95%E8%BF%90%E8%A1%8C-check-mode)
  - [Playbooks 中的错误处理](#playbooks-%E4%B8%AD%E7%9A%84%E9%94%99%E8%AF%AF%E5%A4%84%E7%90%86)
    - [:point_right:忽略错误](#point_right%E5%BF%BD%E7%95%A5%E9%94%99%E8%AF%AF)
    - [控制对失败的定义](#%E6%8E%A7%E5%88%B6%E5%AF%B9%E5%A4%B1%E8%B4%A5%E7%9A%84%E5%AE%9A%E4%B9%89)
  - [:point_right:标签](#point_right%E6%A0%87%E7%AD%BE)
  - [Vault - 文件加密](#vault---%E6%96%87%E4%BB%B6%E5%8A%A0%E5%AF%86)
    - [:point_right:在Vault加密下运行Playbook](#point_right%E5%9C%A8vault%E5%8A%A0%E5%AF%86%E4%B8%8B%E8%BF%90%E8%A1%8Cplaybook)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 简介

ansible是一个自动化运维工具，基于Python开发，集合了众多运维工具（puppet、cfengine、chef、func、fabric）的优点，实现了批量系统配置、批量程序部署、批量运行命令等功能。

Ansible 提供了简单的 IT 自动化，可以结束重复性任务并让 DevOps 团队腾出时间来从事更具战略意义的工作。

# 安装

```bash
yum -y install ansible
apt install ansible

# 主节点配置免密
ssh-keygen -t rsa
ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.10.149
ssh-copy-id -i ~/.ssh/id_rsa.pub root@192.168.10.113
ssh-copy-id -i ~/.ssh/id_rsa.pub root@127.0.0.1

# 配置文件
/etc/ansible/ansible.cfg    # 主配置文件 
/etc/ansible/hosts          # Inventory 设备信息
```

> 通常我们使用 ssh 与托管节点通信，默认使用 sftp.如果 sftp 不可用，可在 ansible.cfg 配置文件中配置成 scp 的方式
>
> 配置文件详情：https://ansible-tran.readthedocs.io/en/latest/docs/intro_configuration.html
>
> 更推荐使用容器进行操作

# :point_right:inventory 配置信息

inventory 和 ini文件配置大致差不多 ,[官方例子](https://ansible-tran.readthedocs.io/en/latest/docs/intro_inventory.html#)

方括号[]中是组名,用于对系统进行分类,便于对不同系统进行个别的管理.

## 变量

```bash
# 主机变量
[atlanta]
host1 http_port=80 maxRequestsPerChild=808
host2 http_port=303 maxRequestsPerChild=808

# 组变量 如果多台主机有相同变量，推荐使用组变量
[atlanta]
host1
host2

[atlanta:vars]
ntp_server=ntp.atlanta.example.com
proxy=proxy.atlanta.example.com
```



## 把一个组作为另一个组的子成员

```bash
[atlanta]
host1
host2

[raleigh]
host2
host3

[southeast:children]
atlanta
raleigh

[southeast:vars]
some_server=foo.southeast.example.com
halon_system_timeout=30
self_destruct_countdown=60
escape_pods=2

[usa:children]
southeast
northeast
southwest
northwest
```

## :point_right:示例

```bash
[all:vars]
ansible_port=22
ansible_user=root
ansible_ssh_pass=root_passwd
# ansible_python_interpreter=/usr/bin/python3
# ansible_ssh_private_key_file=/kubespray/configs/ssh_cert/id_rsa

[bastion]
# bastion ansible_host=x.x.x.x ansible_user=some_user

[all]
kube-control-1 ansible_host=192.168.0.1
kube-control-2 ansible_host=192.168.0.2
kube-control-3 ansible_host=192.168.0.3
kube-node-1 ansible_host=192.168.0.4
kube-node-2 ansible_host=192.168.0.5
kube-node-3 ansible_host=192.168.0.6
kube-node-4 ansible_host=192.168.0.7

[kube_control_plane]
kube-control-1
kube-control-2
kube-control-3

[etcd]
kube-control-1
kube-control-2
kube-control-3

[kube_node]
kube-control-1
kube-control-2
kube-control-3
kube-node-1
kube-node-2
kube-node-3
kube-node-4

[gpu_node]

[calico_rr]

[k8s_cluster:children]
kube_control_plane
kube_node
calico_rr
```

- `ansible_ssh_host` : 将要连接的远程主机名.与你想要设定的主机的别名不同的话,可通过此变量设置.
- `ansible_ssh_port` : ssh端口号.如果不是默认的端口号,通过此变量设置.
- `ansible_ssh_user` : 默认的 ssh 用户名
- `ansible_ssh_pass` : ssh 密码(这种方式并不安全,我们强烈建议使用 --ask-pass 或 SSH 密钥)
- `ansible_sudo_pass` : sudo 密码(这种方式并不安全,我们强烈建议使用 --ask-sudo-pass)
- `ansible_sudo_exe` (new in version 1.8) : sudo 命令路径(适用于1.8及以上版本)
- `ansible_connection` : 与主机的连接类型.比如:local, ssh 或者 paramiko. Ansible 1.2 以前默认使用 paramiko.1.2 以后默认使用 'smart','smart' 方式会根据是否支持 ControlPersist, 来判断'ssh' 方式是否可行.
- `ansible_ssh_private_key_file` : ssh 使用的私钥文件.适用于有多个密钥,而你不想使用 SSH 代理的情况.
- `ansible_shell_type` : 目标系统的shell类型.默认情况下,命令的执行使用 'sh' 语法,可设置为 'csh' 或 'fish'.
- `ansible_python_interpreter` :  目标主机的 python 路径.适用于的情况: 系统中有多个 Python, 或者命令路径不是"/usr/bin/python",比如  \*BSD, 或者 /usr/bin/python（不使用 "/usr/bin/env" 机制）,
  - thon_interpreter 的工作方式相同,可设定如 ruby 或 perl 的路径....

# :point_right:ansible命令行参数

```Bash
usage: ansible [-h] [--version] [-v] [-b] [--become-method BECOME_METHOD]
               [--become-user BECOME_USER] [-K] [-i INVENTORY] [--list-hosts]
               [-l SUBSET] [-P POLL_INTERVAL] [-B SECONDS] [-o] [-t TREE] [-k]
               [--private-key PRIVATE_KEY_FILE] [-u REMOTE_USER]
               [-c CONNECTION] [-T TIMEOUT]
               [--ssh-common-args SSH_COMMON_ARGS]
               [--sftp-extra-args SFTP_EXTRA_ARGS]
               [--scp-extra-args SCP_EXTRA_ARGS]
               [--ssh-extra-args SSH_EXTRA_ARGS] [-C] [--syntax-check] [-D]
               [-e EXTRA_VARS] [--vault-id VAULT_IDS]
               [--ask-vault-pass | --vault-password-file VAULT_PASSWORD_FILES]
               [-f FORKS] [-M MODULE_PATH] [--playbook-dir BASEDIR]
               [-a MODULE_ARGS] [-m MODULE_NAME]
               pattern
               
Options:
  -a MODULE_ARGS, --args=MODULE_ARGS    
             #module arguments
             #指定执行模块使用的参数  
  --ask-vault-pass      
             #ask for vault password
             #加密playbook文件时提示输入密码
  -B SECONDS, --background=SECONDS
             #run asynchronously, failing after X seconds(default=N/A)
             #后台运行超时时间,异步运行，X秒之后失败
  -C, --check           
             #don't make any changes; instead, try to predict some of the changes that may occur
             #模拟执行，不会真正在机器上执行(查看执行会产生什么变化)
  -D, --diff            
             #when changing (small) files and templates, show the differences in those files; works great with --check
             #当更新的文件数及内容较少时，该选项可显示这些文件不同的地方，该选项结合-C用会有较好的效果
  -e EXTRA_VARS, --extra-vars=EXTRA_VARS
             #set additional variables as key=value or YAML/JSON
             #执行命令时添加额外参数变量
  -f FORKS, --forks=FORKS
             #specify number of parallel processes to use(default=5)
             #并行任务数。FORKS被指定为一个整数,默认是5
  -h, --help            
             #show this help message and exit
             #打开帮助文档API
  -i INVENTORY, --inventory-file=INVENTORY
             #specify inventory host path(default=/etc/ansible/hosts) or comma separated host list.
             #指定要读取的Inventory文件
  -l SUBSET, --limit=SUBSET
             #further limit selected hosts to an additional pattern
             #限定执行的主机范围
  --list-hosts          
             #outputs a list of matching hosts; does not execute anything else
             #列出执行匹配到的主机，但并不会执行
  -m MODULE_NAME, --module-name=MODULE_NAME
             #module name to execute (default=command)
             #指定执行使用的模块，默认使用 command 模块
  -M MODULE_PATH, --module-path=MODULE_PATH
             #specify path(s) to module library (default=None)
             #要执行的模块的路径
  --new-vault-password-file=NEW_VAULT_PASSWORD_FILE
             #new vault password file for rekey
             #    
  -o, --one-line        
             #condense output
             #压缩输出，摘要输出.尝试一切都在一行上输出
  --output=OUTPUT_FILE  
             #output file name for encrypt or decrypt; use - for stdout
             #
  -P POLL_INTERVAL, --poll=POLL_INTERVAL
             #set the poll interval if using -B (default=15)
             #设置轮询间隔，每隔数秒。需要- B
  --syntax-check        
             #perform a syntax check on the playbook, but do not execute it
             #检查Playbook中的语法书写
  -t TREE, --tree=TREE  
             #log output to this directory
             #将日志内容保存在该输出目录,结果保存在一个文件中在每台主机上
  --vault-password-file=VAULT_PASSWORD_FILE
             #vault password file
             #
  -v, --verbose         
             #verbose mode (-vvv for more, -vvvv to enable connection debugging)
             #执行详细输出
  --version             
             #show program's version number and exit
             #显示版本

  Connection Options:
    control as whom and how to connect to hosts

    -k, --ask-pass      
             #ask for connection password
             #
    --private-key=PRIVATE_KEY_FILE, --key-file=PRIVATE_KEY_FILE
             #use this file to authenticate the connection
             #
    -u REMOTE_USER, --user=REMOTE_USER
             #connect as this user (default=None)
             #指定远程主机以USERNAME运行命令
    -c CONNECTION, --connection=CONNECTION
             #connection type to use (default=smart)
             #指定连接方式，可用选项paramiko (SSH)、ssh、local，local方式常用于crontab和kickstarts
    -T TIMEOUT, --timeout=TIMEOUT
             #override the connection timeout in seconds(default=10)
             #SSH连接超时时间设定，默认10s
    --ssh-common-args=SSH_COMMON_ARGS
             #specify common arguments to pass to sftp/scp/ssh (e.g.ProxyCommand)
             #
    --sftp-extra-args=SFTP_EXTRA_ARGS
             #specify extra arguments to pass to sftp only (e.g. -f, -l)
             #
    --scp-extra-args=SCP_EXTRA_ARGS
             #specify extra arguments to pass to scp only (e.g. -l)
             #
    --ssh-extra-args=SSH_EXTRA_ARGS
             #specify extra arguments to pass to ssh only (e.g. -R)
             #

  Privilege Escalation Options:
    control how and which user you become as on target hosts

    -s, --sudo          
             #run operations with sudo (nopasswd) (deprecated, use become)
             #相当于Linux系统下的sudo命令
    -U SUDO_USER, --sudo-user=SUDO_USER
             #desired sudo user (default=root) (deprecated, use become)
             #使用sudo，相当于Linux下的sudo命令
    -S, --su            
             #run operations with su (deprecated, use become)
             #
    -R SU_USER, --su-user=SU_USER
             #run operations with su as this user (default=root) (deprecated, use become)
             #
   -b, --become        
             #run operations with become (does not imply password prompting)
             #
    --become-method=BECOME_METHOD
             #privilege escalation method to use (default=sudo),valid choices: [ sudo | su | pbrun | pfexec | doas |dzdo | ksu | runas ]
             #
    --become-user=BECOME_USER
             #run operations as this user (default=root)
             #
    --ask-sudo-pass     
             #ask for sudo password (deprecated, use become)
             #
    --ask-su-pass       
             #ask for su password (deprecated, use become)
             #
    -K, --ask-become-pass
             #ask for privilege escalation password
             #
               
# ansible <host-pattern> [-f forks] [-m module_name] [-a args]
# <host-pattern>  这次命令对哪些主机生效的
    # inventory group name
    # ip
    # all
# -f forks        一次处理多少个主机
# -m module_name  要使用的模块
# -a args         模块特有的参数

# 例子
ansible 192.168.10.113 -m command -a 'date'
ansible k8s_cluster:children -m command -a 'ping'
ansible all -m command -a 'ping'
```

# 常用模块

ansible的模块 在安装目录 /usr/local/lib/{pythonveison}/site-packages/ansible/modules/下

- ansible的模块，可以在github上获取：
  - 核心模块
    - [ansible/ansible-modules-core](https://github.com/ansible/ansible-modules-core)
  - 额外模块
    - [ansible/ansible-modules-extras](https://github.com/ansible/ansible-modules-extras)

帮助命令：`ansible-doc --help` 常用如下

```bash
# 列出ansible所有的模块
ansible-doc -l

# 查看指定模块具体使用
ansible-doc -s MODULE_NAME
```

- `ping` 模块: 检查指定节点机器是否还能连通，用法很简单，不涉及参数，主机如果在线，则回复pong 。
- `raw` 模块: 执行原始的命令，而不是通过模块子系统。
- `yum` 模块: RedHat和CentOS的软件包安装和管理工具。
- `apt` 模块: Ubuntu/Debian的软件包安装和管理工具。
- `pip` 模块 : 用于管理Python库依赖项，为了使用pip模块，必须提供参数name或者requirements。
- `synchronize` 模块: 使用rsync同步文件，将主控方目录推送到指定节点的目录下。
- `template` 模块: 基于模板方式生成一个文件复制到远程主机（template使用Jinjia2格式作为文件模版，进行文档内变量的替换的模块。
- `copy` 模块: 在远程主机执行复制操作文件。
- `user` 模块 与 `group` 模块: user模块是请求的是useradd, userdel, usermod三个指令，goup模块请求的是groupadd, groupdel, groupmod 三个指令。
- `service` 或 `systemd` 模块: 用于管理远程主机的服务。
- `get_url` 模块: 该模块主要用于从http、ftp、https服务器上下载文件（类似于wget）。
- `fetch` 模块: 它用于从远程机器获取文件，并将其本地存储在由主机名组织的文件树中。
- `file` 模块: 主要用于远程主机上的文件操作。
- `lineinfile` 模块: 远程主机上的文件编辑模块
- `unarchive`模块: 用于解压文件。
- `command`模块 和 shell模块: 用于在各被管理节点运行指定的命令. shell和command的区别：shell模块可以特殊字符，而command是不支持
- `hostname`模块: 修改远程主机名的模块。
- `script`模块: 在远程主机上执行主控端的脚本，相当于scp+shell组合。
- `stat`模块: 获取远程文件的状态信息，包括atime,ctime,mtime,md5,uid,gid等信息。
- `cron`模块: 远程主机crontab配置。
- `mount`模块: 挂载文件系统。
- `find`模块: 帮助在被管理主机中查找符合条件的文件，就像 find 命令一样。
- `selinux`模块：远程管理受控节点的selinux的模块

## :point_right:ping - 测主机连通性

```bash
ansible 192.168.10.113 -m ping
ansible -i /etc/ansible/hosts all -m ping
```

## :point_right:raw、shell、command - 远程执行命令

command  比较安全，没有shell注入的风险，但不能执行管道命令

```bash
ansible -i /etc/ansible/hosts all -m raw -a "cat /etc/passwd|grep kevin"
ansible -i /etc/ansible/hosts all -m shell -a "cat /etc/passwd|grep kevin"     
ansible -i /etc/ansible/hosts all -m command -a "hostname"
```

## :point_right:yum - RedHat 和 CentOS远程安装或卸载程序包

- `config_file` : yum的配置文件 （optional）
- `disable_gpg_check` : 关闭gpg_check （optional）
- `disablerepo` : 不启用某个源 （optional）
- `enablerepo` : 启用某个源（optional）
- `name` : 要进行操作的软件包的名字，默认最新的程序包，指明要安装的程序包，可以带上版本号，也可以传递一个url或者一个本地的rpm包的路径
- `state` : 表示是安装还是卸载的状态, 其中present、installed、latest 表示安装,  absent 、removed表示卸载删除;  present默认状态, laster表示安装最新版本.

```bash
# 安装httpd
[root@ansible-server ~]# ansible web-nodes -m yum -a 'name=httpd state=latest'
[root@ansible-server ~]# ansible web-nodes -m yum -a 'name=httpd state=present'
[root@ansible-server ~]# ansible web-nodes -m yum -a 'name=httpd state=installed'
 
[root@ansible-server ~]# ansible web-nodes -m yum -a 'name=http://nginx.org/packages/centos/6/noarch/RPMS/nginx-release-centos-6-0.el6.ngx.noarch.rpm state=present'
[root@ansible-server ~]# ansible web-nodes -m yum -a 'name="@Development tools" state=present'
 
# 卸载httpd
[root@ansible-server ~]# ansible web-nodes -m yum -a 'name=httpd state=absent'
[root@ansible-server ~]# ansible web-nodes -m yum -a 'name=httpd state=removed'
```

## apt - debian-ubuntu 远程安装程序包

- `deb`: 用于安装远程机器上的.deb后缀的软件包（optional）
- install_recommends: 这个参数可以控制远程电脑上是否只是下载软件包，还是下载后安装，默认参数为true,设置为false的时候只下载软件包，不安装
- update_cache: 当这个参数为yes的时候等于apt-get update（optional）
- name: apt要下载的软件包名字，支持name=git=1.6 这种制定版本的模式
- state: 状态（present，absent，latest）,表示是安装还是卸载. 其中present、installed、latest 表示安装, absent 、removed表示卸载删除; present默认状态, laster表示安装最新版本.

```bash
ansible all -m apt -a "name=nginx state=absent'
```

## pip - 管理python的依赖

- name: 软件包名称
- version: 软件版本
- virtualenv: virtualenv环境，若不存在，则会新建

```bash
ansible all -m apt -a "name=ipython version=1.2.1 virtualenv={{ venv_path }}'
```

## **:point_right:synchronize - 使用rsync同步文件，将主控方目录推送到指定受控节点的目录下**

- `delete`: 删除不存在的文件，delete=yes 使两边的内容一样（即以推送方为主），默认no
- `src`: 要同步到目的地的源主机上的路径; 路径可以是绝对的或相对的。如果路径使用”/”来结尾，则只复制目录里的内容，如果没有使用”/”来结尾，则包含目录在内的整个内容全部复制
- `dest`:目的地主机上将与源同步的路径; 路径可以是绝对的或相对的。
- `dest_port`:默认目录主机上的端口 ，默认是22，走的ssh协议。
- `mode`: push或pull，默认push，一般用于从本机向远程主机上传文件，pull 模式用于从远程主机上取文件。
- `rsync_opts`:通过传递数组来指定其他rsync选项。

```bash
ansible web-nodes -m synchronize -a 'src=/data/kevin dest=/home'
```

## :point_right:copy - 拷贝文件和目录模块

```bash
ansible host -m copy -a 'src=/path/to/file dest=/path/to/file [owner=name] [group=name] [mode=number]
ansible node2.test.com -m copy -a 'src=/tmp/hiyang dest=/root/ owner=root group=root mode=644'
或者
# ansible 192.168.1.41 -m copy -a "src=/root/.vimrc dest=/tmp/vimrc"
```

## :point_right:cron - 定时任务模块

执行周期的格式说明
`minute，hour，day，month，weekday`

```bash
ansible node2.test.com -m cron -a "name=test_cron job='/bin/date > /tmp/tmp.time' weekday=6"
ansible node2.test.com -m cron -a "name=test_cron job='/bin/date > /tmp/tmp.time' minute='*/10'"
# 查看
nsible node2.test.com -m shell -a 'crontab -l'

# 删除定时任务
ansible node2.test.com -m cron -a "name=test_cron state=absent"
```

## debug - 运行打印变量值

```bash
- name: show return value of cmd
  hosts: test
  tasks:
  - name: capture output of cmd
    command: id -un
    register: login
  - debug: var=login
  - debug: msg="{{ login.stdout }}"
```

## file - 用于设定或修改文件的属性信息

- group：默认为空
- owner：默认为空
- path：默认为空，别名：'dest', `name'
- recurse：yes, no 默认为no
- src：创建连接文件时有用
- state：file, link, directory, hard, touch, absent
  - file 默认属性，如果文件不存在则不创建，并且报错，用于修改已存在文件的属性
  - directory 如果目录不存在，则创建目录和子目录
  - absent 递归删除文件或目录
  - link 在创建软链接是有用
  - hard 创建硬链接

```bash
---
- name: file
  hosts: local
  tasks:
  - name: file
    file:
      src: '/tmp/{{ item.src }}'
      dest: '{{ item.dest }}'
      state: link
    with_items:
      - { src: 'x', dest: 'y' }
      - { src: 'z', dest: 'k' }
```

## :point_right:setup - 获取远程主机信息

```bash
# 查看计算机信息
ansible test.com -m setup
# 增加参数进行过滤
ansible 127.0.0.1 -m setup -a 'filter=ansible_all_ipv4_addresses'

# 变量可以在playbook中直接使用
```

## :point_right:stat - 返回文件属性

- **attributes**：返回指定文件的属性。
- **executable**：如果调用的用户在目标路径上有执行权限，则返回 true。
- **exists**：如果指定的路径存在，返回真。
- **gr_name**：返回文件所有者的组的名称。
- **islbk**：如果指定的文件是一个块状设备，则返回true。
- **ischr**： 如果指定的文件是一个字符文件，则返回真。
- **isreg**：如果指定的文件是一个常规文件，则返回真。
- **isdir**：如果指定的文件是一个目录，则返回真。
- **isnk**：如果目标文件是一个链接，则返回真。
- **mode**：以八进制符号返回文件权限。

```bash
- name: check file type
  hosts: all
  become: ye
  tasks:
  - name: get file info
    stat:
      path: /var/log/kern.log
    register: file_info
  - name: regular file?
    debug:
      msg:  specified path is a regular file
    when: file_info.stat.isreg
  - name: is a directory?
    debug:
      msg: specified path is a directory
    when: file_info.stat.isdir
```

## :point_right:service、systemd - 系统服务管理

如果使用systemctl 管理程序的话，可以使用systemd模块，systemctl 可以 控制程序启/停，reload，开机启动，观察程序状态（status）等，掌握使用后管理就更方便了

```bash
ansible 192.168.20.23 -m service -a 'name=nginx state=started enabled=yes'
ansible 192.168.20.23 -m systemd  -a 'name=nginx state=started enabled=yes'
ansible all -m systemd -a "name=NetworkManager enabled=no"
```

## timezone - 同步时区

## unarchive - 解压压缩包

## :point_right:user - 用户管理

- name参数：必须参数，用于指定要操作的用户名称，可以使用别名 user。

- group参数：此参数用于指定用户所在的基本组。

- gourps参数：此参数用于指定用户所在的附加组。注意，如果说用户已经存在并且已经拥有多个附加组，那么如果想要继续添加新的附加组，需要结合 append 参数使用，否则在默认情况下，当再次使用 groups 参数设置附加组时，用户原来的附加组会被覆盖。

- append参数：如果用户原本就存在多个附加组，那么当使用 groups 参数设置附加组时，当前设置会覆盖原来的附加组设置，如果不想覆盖原来的附加组设置，需要结合 append 参数，将 append 设置为 yes，表示追加附加组到现有的附加组设置，append 默认值为 no。

- shell参数：此参数用于指定用户的默认 shell。

- uid参数：此参数用于指定用户的 uid 号。

- expires参数：此参数用于指定用户的过期时间，相当于设置 /etc/shadow 文件中的的第8列，比如，你想要设置用户的过期日期为2018年12月31日，那么你首先要获取到2018年12月31日的 unix 时间戳，使用命令 “date -d 2018-12-31 +%s” 获取到的时间戳为1546185600，所以，当设置 expires=1546185600 时，表示用户的过期时间为2018年12月31日0点0分，设置成功后，查看远程主机的 /etc/shadow 文件，对应用户的第8八列的值将变成17895（表示1970年1月1日到2018年12月31日的天数，unix 时间戳的值会自动转换为天数，我们不用手动的进行换算），目前此参数只支持在 Linux 和 FreeBSD 系统中使用。

- comment参数：此参数用于指定用户的注释信息。

- state参数：此参数用于指定用户是否存在于远程主机中，可选值有 present、absent，默认值为 present，表示用户需要存在，当设置为 absent 时表示删除用户。

- remove参数：当 state 的值设置为 absent 时，表示要删除远程主机中的用户。但是在删除用户时，不会删除用户的家目录等信息，这是因为 remove 参数的默认值为 no，如果设置为yes，在删除用户的同时，会删除用户的家目录。当 state=absent 并且 remove=yes 时，相当于执行 “userdel --remove” 命令。

- password参数：此参数用于指定用户的密码。**但是这个密码不能是明文的密码**，而是一个对明文密码”加密后”的字符串，相当于 /etc/shadow 文件中的密码字段，是一个对明文密码进行哈希后的字符串，你可以在 python 的命令提示符下输入如下命令，生成明文密码对应的加密字符串。

  - import crypt; crypt.crypt('666666')
    输入上述命令后，即可得到明文密码666666对应的加密字符串。
- update_password参数：此参数有两个值可选，always 和 on_create，当此参数的值设置为always 时表示，如果 password 参数设置的值与用户当前的加密过的密码字符串不一致，则直接更新用户的密码，默认值即为 always，但是当此参数设置为 on_create 时，如果 password参数设置的值与用户当前的加密过的密码字符串不一致，则不会更新用户的密码字符串，保持之前的密码设定。如果是新创建的用户，即使此参数设置为 on_create，也会将用户的密码设置为 password 参数对应的值。

- generate_ssh_key参数：此参数默认值为 no，如果设置为 yes，表示为对应的用户生成 ssh 密钥对，默认在用户家目录的 ./ssh 目录中生成名为 id_rsa 的私钥和名为 id_rsa.pub 的公钥，如果同名的密钥已经存在与对应的目录中，原同名密钥并不会被覆盖(不做任何操作)。

- ssh_key_file参数：当 generate_ssh_key 参数的值为 yes 时，使用此参数自定义生成 ssh 私钥的路径和名称，对应公钥会在同路径下生成，公钥名以私钥名开头，以”.pub”结尾。

- ssh_key_comment参数：当 generate_ssh_key 参数的值为 yes 时，在创建证书时，使用此参数设置公钥中的注释信息。但是如果同名的密钥对已经存在，则并不会修改原来的注释信息，即不做任何操作。当不指定此参数时，默认的注释信息为”ansible-generated on 远程主机的主机名”。

- ssh_key_passphrase参数：当 generate_ssh_key 参数的值为 yes 时，在创建证书时，使用此参数设置私钥的密码。但是如果同名的密钥对已经存在，则并不会修改原来的密码，即不做任何操作。

- ssh_key_type参数：当 generate_ssh_key 参数的值为 yes 时，在创建证书时，使用此参数设置密钥对的类型。默认密钥类型为 rsa，但是如果同名的密钥对已经存在，并不会对同名密钥做任何操作。

```bash
ansible all -m user -a 'name=test groups=root,admin append=yes'
```

## :point_right:group - 组管理

- name	需要管理的组名，也就是要对那个组进行管理
- gid		设置组id
- state	执行状态
  - absent	删除
  - present	创建（默认）

```bash
ansible host -m group -a "name=stb gid=10000"
```

## :point_right:wait_for - 在规定时间内检测

- active_connection_states	Default:
  - [“ESTABLISHED”,“FIN_WAIT1”,“FIN_WAIT2”,“SYN_RECV”,“SYN_SENT”,“TIME_WAIT”]	被算作活动连接的TCP连接状态列表。
- connect_timeout	默认值：5秒	在关闭和重试之前等待连接发生的最大秒数。
- delay	默认值：0	开始轮询之前等待的秒数。
- exclude_hosts		尝试寻找活跃的TCP连接，耗尽状态时，可以忽略的主机或ip列表。
- host	默认值：“127.0.0.1”	等待可解析的主机名或IP地址。
- msg		这将覆盖不满足所需条件的正常错误消息。
- path		在继续之前，文件系统中必须存在的文件的路径。参数“Path”和“port”互斥，二者只可取其一。
- port		轮询的端口号。参数“Path”和“port”互斥，二者只可取其一。
- search_regex		可用于匹配文件或套接字连接中的字符串。默认为多行正则表达式。
- sleep	默认值：1秒	在两次检查之间休眠的秒数。
- state	当检查一个端口时，started将确保端口是开放的，stopped将检查状态是关闭的，drained将检查活动连接是耗尽的。当检查文件或搜索字符串是否存在时，将确保文件或字符串已存在再继续执行，absent将检查该文件是否存在或被删除。
  - present
  - absent
  - started
  - stopped
  - drained	
- timeout	默认值：300秒	等待的最大秒数，当与另一个条件一起使用时，它将强制出现错误。在没有其他条件的情况下，它相当于睡眠

```bash
ansible 127.0.0.1 -m wait_for -a "port=8082 delay=2 timeout=30"
```

# :point_right:playbook

## :point_right:项目结构组成

> :point_right:gather_facts:yes  setup模块返回的变量可以直接使用

```livecodeserver
inventory       #以下操作应用的主机
modules         #调用哪些模块做什么样的操作
ad hoc commands #在这些主机上运行哪些命令
playbooks  
   tasks       #任务,即调用模块完成的某操作
   variable    #变量
   templates   #模板
   handlers    #处理器，由某事件触发执行的操作
   roles       #角色
```

- **hosts** : 指定要执行指定任务的主机
- **remote_user** : 指定远程主机上的执行任务的用户,此处的意思是以root身份执行
- **user** : 与remote_user相同
- **sudo** : 如果设置为yes执行该任务组的用户在执行任务的时候,获取root权限
- **sudo_user** : 指定使用那个用户授权执行
- **connection** : 通过什么方式连接到远程主机,默认为ssh
- **gather_facts** : setup模块默认自动执行

### playbook.yml示例

```bash
---
#Installing MariaDB Binary Tarballs
- hosts: db_server
  remote_user: root
  gather_facts: no

  tasks:
    - name: create group
      group: name=mysql gid=27 system=yes
    - name: create user
      user: name=mysql uid=27 system=yes group=mysql shell=/sbin/nologin home=/data/mysql create_home=no
    - name: mkdir datadir
      file: path=/data/mysql owner=mysql group=mysql state=directory
    - name: unarchive package
      unarchive: src=/data/ansible/files/mariadb-10.2.27-linux-x86_64.tar.gz dest=/usr/local/ owner=root group=root
    - name: link
      file: src=/usr/local/mariadb-10.2.27-linux-x86_64 path=/usr/local/mysql state=link 
    - name: install database
      shell: chdir=/usr/local/mysql   ./scripts/mysql_install_db --datadir=/data/mysql --user=mysql
    - name: config file
      copy: src=/data/ansible/files/my.cnf  dest=/etc/ backup=yes
    - name: service script
      shell: /bin/cp  /usr/local/mysql/support-files/mysql.server  /etc/init.d/mysqld
    - name: start service
      service: name=mysqld state=started enabled=yes
    - name: PATH variable
      copy: content='PATH=/usr/local/mysql/bin:$PATH' dest=/etc/profile.d/mysql.sh
```

### :point_right:语法检查

```bash
ansible-playbook book.yml --syntax-check #检查yaml语法
ansible-playbook book.yml --list-tasks #检查tasks任务
ansible-playbook book.yml --list-hosts #检查生效的主机
ansible-playbook book.yml --start-at-task=”Copy Nginx.conf” #如果你想从指定的任务开始执行playbook,可以使用``–start-at``选项:
ansible-playbook playbook.yml --step #分步运行playbook，ansible在每个任务前会自动停止,并询问是否应该执行该任务.
```

## :point_right:变量

### :point_right:定义变量

变量名可以为字母,数字以及下划线.变量始终应该以字母开头. 

命令行加入

```bash
ansible-playbook -e var=value
ansible-playbook -e @env.yml # yaml 格式 name:stb
```

yml方式

```yaml
# playbook-yml
- hosts: db_server
  remote_user: root
  gather_facts: no
  vars:
    - var1: value1
    - var2: value2

# 定义yml 文件
- hosts: db_server
  remote_user: root
  gather_facts: no
  vars_files:
      - vars.yml

```

### :point_right:使用变量

Ansible允许你使用Jinja2模板系统在playbook中引用变量.

```yaml
template: src=foo.cfg.j2 dest={{ remote_install_path }}/foo.cfg

# YAML语法要求如果值以{{ foo }}开头的话我们需要将整行用双引号包起来
- hosts: app_servers
  vars:
       app_path: "{{ base_path }}/22"
```

### :point_right:使用Facts获取的信息

setup模块会获取以下信息

```bash
"ansible_all_ipv4_addresses": [
    "REDACTED IP ADDRESS"
],
"ansible_all_ipv6_addresses": [
    "REDACTED IPV6 ADDRESS"
],
"ansible_architecture": "x86_64",
"ansible_bios_date": "09/20/2012",
"ansible_bios_version": "6.00",
...

# 获取硬盘的模型
{{ ansible_devices.sda.model }}
# 访问复杂的数据
{{ ansible_eth0["ipv4"]["address"] }}
{{ foo[0] }}

# 如果需要关闭fact
- hosts: whatever
  gather_facts: no
```

### :point_right:注册变量

变量的另一个主要用途是在运行命令时,把命令结果存储到一个变量中.不同模块的执行结果是不同的

```yaml
- hosts: web_servers
  tasks:
     - shell: /usr/bin/foo
       register: foo_result
       ignore_errors: True
     - shell: /usr/bin/bar
       when: foo_result.rc == 5
```

### 变量文件

```yaml
- hosts: all
  remote_user: root
  vars:
    favcolor: blue
  vars_files:
    - /vars/external_vars.yml
```

### :point_right:命令行中传递变量

```yaml
ansible-playbook release.yml --extra-vars "version=1.23.45 other_variable=foo"

# json文件
--extra-vars "@some_file.json"

# yml文件
-e "@${ENV_FILE}"
```



## :point_right:role 和 include 重用yml文件

当刚开始可能会把 playbook 写成一个很大的文件，到后来可能你会希望这些文件是可以方便去重用的，所以需要重新去组织这些文件。

### :point_right:include

使用 include 语句引用 task 文件的方法，可允许你将一个配置策略分解到更小的文件中。使用

```bash
# possibly saved as tasks/foo.yml
- name: placeholder foo
  command: /bin/foo
- name: placeholder bar
  command: /bin/bar
  
# 引用
tasks:
  - include: tasks/foo.yml

# 你也可以给 include 传递变量。 这些变量就可以在 include 包含的文件中使用了 {{wp_user}}。 
tasks:
  - include: wordpress.yml wp_user=timmy
  - include: wordpress.yml wp_user=alice
  - include: wordpress.yml wp_user=bob

# 使用单独的vars也可以传递变量
tasks:
  - include: wordpress.yml
    vars:
        wp_user: timmy
        some_list_variable:
          - alpha
          - beta
          - gamma
```

### :point_right:handler中include

handler 在task之后执行，本身和task没啥区别

```yaml
---
# this might be in a file like handlers/handlers.yml
- name: restart apache
  service: name=apache state=restarted

handlers:
  - include: handlers/handlers.yml
```

### include 引用 playbook的yml

include 语句可以和其他非 include 的 tasks 和 handlers 混合使用。

Include 语句也可用来将一个 playbook 文件导入另一个 playbook 文件。这种方式允许你定义一个 顶层的 playbook，这个顶层 playbook 由其他 playbook 所组成。

```bash
- name: this is a play at the top level of a file
  hosts: all
  remote_user: root

  tasks:

  - name: say hi
    tags: foo
    shell: echo "hi..."

- include: load_balancers.yml
- include: webservers.yml
```

### Roles 

Roles 基于一个已知的文件结构，去自动的加载某些 vars_files，tasks 以及 handlers。基于 roles 对内容进行分组，使得我们可以容易地与其他用户分享 roles 。

#### :point_right:Roles项目结构

```bash
site.yml
webservers.yml
fooservers.yml
roles/
   common/
     files/
     templates/
     tasks/
     handlers/
     vars/
     defaults/
     meta/
   webservers/
     files/
     templates/
     tasks/
     handlers/
     vars/
     defaults/
     meta/
```

这个 playbook 为一个角色 ‘x’ 指定了如下的行为：

- 如果 roles/x/tasks/main.yml 存在, 其中列出的 tasks 将被添加到 play 中
- 如果 roles/x/handlers/main.yml 存在, 其中列出的 handlers 将被添加到 play 中
- 如果 roles/x/vars/main.yml 存在, 其中列出的 variables 将被添加到 play 中
- 如果 roles/x/meta/main.yml 存在, 其中列出的 “角色依赖” 将被添加到 roles 列表中 (1.3 and later)
- 所有 copy tasks 可以引用 roles/x/files/ 中的文件，不需要指明文件的路径。
- 所有 script tasks 可以引用 roles/x/files/ 中的脚本，不需要指明文件的路径。
- 所有 template tasks 可以引用 roles/x/templates/ 中的文件，不需要指明文件的路径。
- 所有 include tasks 可以引用 roles/x/tasks/ 中的文件，不需要指明文件的路径。

#### :point_right:使用参数化的 roles

```bash
- hosts: webservers
  roles:
    - common
    - { role: foo_app_instance, dir: '/opt/a',  port: 5000 }
    - { role: foo_app_instance, dir: '/opt/b',  port: 5001 }
```

#### :point_right:when设置触发条件

```bash
- hosts: webservers
  roles:
    - { role: some_role, when: "ansible_os_family == 'RedHat'" }
```

#### 分配tag

```bash
- hosts: webservers
  roles:
    - { role: foo, tags: ["bar", "baz"] }
```

如果你希望定义一些 tasks，让它们在 roles 之前以及之后执行，你可以这样做:

```yml
---
- hosts: webservers
  pre_tasks:
    - shell: echo 'hello'
  roles:
    - { role: some_role }
  tasks:
    - shell: echo 'still busy'
  post_tasks:
    - shell: echo 'goodbye'
```

## :point_right:条件选择

**When 语句**  

### :point_right:某个条件时执行

```bash
tasks:
  - name: "shutdown Debian flavored systems"
    command: /sbin/shutdown -t now
    when: ansible_os_family == "Debian"

tasks:
  - command: /bin/false
    register: result
    ignore_errors: True
  - command: /bin/something
    when: result|failed
  - command: /bin/something_else
    when: result|success
  - command: /bin/still/something_else
    when: result|skipped
```

### **加载客户事件**

加载客户自己的事件,事实上也很简单,将在:doc:developing_modules 详细介绍.只要调用客户自己的事件,进而把所有的模块放在任务列表顶端, 变量的返回值今后就可以访问了:

```bash
tasks:
    - name: gather site specific fact data
      action: site_facts
    - command: /usr/bin/thingy
      when: my_custom_fact_just_retrieved_from_the_remote_system == '1234'
```

### **:point_right:在roles 和 includes 上面应用’when’语句**

```bash
- include: tasks/sometasks.yml
  when: "'reticulating splines' in output"
  
- hosts: webservers
  roles:
     - { role: debian_stock_config, when: ansible_os_family == 'Debian' }
```

 ## 循环体

### :point_right:标准循环

变量使用with_items 成员

```yaml
- name: add several users
  user: name={{ item.name }} state=present groups={{ item.groups }}
  with_items:
    - { name: 'testuser1', groups: 'wheel' }
    - { name: 'testuser2', groups: 'root' }
```

### :point_right:嵌套循环

变量使用with_nested成员

```yaml
- name: give users access to multiple databases
  mysql_user: name={{ item[0] }} priv={{ item[1] }}.*:ALL append_privs=yes password=foo
  with_nested:
    - [ 'alice', 'bob' ]
    - [ 'clientdb', 'employeedb', 'providerdb' ]
```

### :point_right:对map的循环

变量使用with_dict成员

```yaml
# 比如你有user的变量
---
users:
  alice:
    name: Alice Appleworth
    telephone: 123-456-7890
  bob:
    name: Bob Bananarama
    telephone: 987-654-3210
    
tasks:
  - name: Print phone records
    debug: msg="User {{ item.key }} is {{ item.value.name }} ({{ item.value.telephone }})"
    with_dict: "{{users}}"
```

### 对文件列表使用循环

变量使用with_fileglob成员

```yaml
---
- hosts: all

  tasks:

    # first ensure our target directory exists
    - file: dest=/etc/fooapp state=directory

    # copy each file over that matches the given pattern
    - copy: src={{ item }} dest=/etc/fooapp/ owner=root mode=600
      with_fileglob:
        - /playbooks/files/fooapp/*
```

### 对并行数据集使用循环

变量使用with_together成员

```yaml
---
alpha: [ 'a', 'b', 'c', 'd' ]
numbers:  [ 1, 2, 3, 4 ]

tasks:
    - debug: msg="{{ item.0 }} and {{ item.1 }}"
      with_together:
        - "{{alpha}}"
        - "{{numbers}}"
```

### 对子元素使用循环

变量使用with_subelements成员

```bash
---
users:
  - name: alice
    authorized:
      - /tmp/alice/onekey.pub
      - /tmp/alice/twokey.pub
    mysql:
        password: mysql-password
        hosts:
          - "%"
          - "127.0.0.1"
          - "::1"
          - "localhost"
        privs:
          - "*.*:SELECT"
          - "DB1.*:ALL"
  - name: bob
    authorized:
      - /tmp/bob/id_rsa.pub
    mysql:
        password: other-mysql-password
        hosts:
          - "db1"
        privs:
          - "*.*:SELECT"
          - "DB2.*:ALL"
         
# 可以这些实现
- user: name={{ item.name }} state=present generate_ssh_key=yes
  with_items: "{{users}}"

- authorized_key: "user={{ item.0.name }} key='{{ lookup('file', item.1) }}'"
  with_subelements:
     - users
     - authorized
```

### 对整数序列使用循环

变量使用with_sequence成员

```yaml
---
- hosts: all

  tasks:

    # create groups
    - group: name=evens state=present
    - group: name=odds state=present

    # create some test users
    - user: name={{ item }} state=present groups=evens
      with_sequence: start=0 end=32 format=testuser%02x

    # create a series of directories with even numbers for some reason
    - file: dest=/var/stuff/{{ item }} state=directory
      with_sequence: start=4 end=16 stride=2

    # a simpler way to use the sequence plugin
    # create 4 groups
    - group: name=group{{ item }} state=present
      with_sequence: count=4
```

### 随机选择

变量使用with_random_choice成员

```bash
- debug: msg={{ item }}
  with_random_choice:
     - "go through the door"
     - "drink from the goblet"
     - "press the red button"
     - "do nothing"
```

### Do - until 循环

```yaml
- action: shell /usr/bin/foo
  register: result
  until: result.stdout.find("all systems go") != -1
  retries: 5
  delay: 10
```

### 查找第一个匹配的文件

变量使用with_first_found成员

```bash
- name: some configuration template
  template: src={{ item }} dest=/etc/file.cfg mode=0444 owner=root group=root
  with_first_found:
    - files:
       - "{{inventory_hostname}}/etc/file.cfg"
      paths:
       - ../../../templates.overwrites
       - ../../../templates
    - files:
        - etc/file.cfg
      paths:
        - templates
```

### 迭代程序的执行结果

```bash
- name: Example of looping over a REMOTE command result
  shell: /usr/bin/something
  register: command_result

- name: Do something with each result
  shell: /usr/bin/something_else --param {{ item }}
  with_items: "{{command_result.stdout_lines}}"
```

### 使用索引循环列表

with_indexed_items

```bash
- name: indexed loop demo
  debug: msg="at array position {{ item.0 }} there is a value {{ item.1 }}"
  with_indexed_items: "{{some_list}}"
```

### 循环配置文件

```bash
# ini 文件
[section1]
value1=section1/value1
value2=section1/value2

[section2]
value1=section2/value1
value2=section2/value2


# 使用
- debug: msg="{{item}}"
  with_ini: value[1-2] section=section1 file=lookup.ini re=true
```

## 最佳实践

### :point_right:优秀的项目结构

```bash
production                # inventory file for production servers 关于生产环境服务器的清单文件
stage                     # inventory file for stage environment 关于 stage 环境的清单文件

group_vars/
   group1                 # here we assign variables to particular groups 这里我们给特定的组赋值
   group2                 # ""
host_vars/
   hostname1              # if systems need specific variables, put them here 如果系统需要特定的变量,把它们放置在这里.
   hostname2              # ""

library/                  # if any custom modules, put them here (optional) 如果有自定义的模块,放在这里(可选)
filter_plugins/           # if any custom filter plugins, put them here (optional) 如果有自定义的过滤插件,放在这里(可选)

site.yml                  # master playbook 主 playbook
webservers.yml            # playbook for webserver tier Web 服务器的 playbook
dbservers.yml             # playbook for dbserver tier 数据库服务器的 playbook

roles/
    common/               # this hierarchy represents a "role" 这里的结构代表了一个 "role"
        tasks/            #
            main.yml      #  <-- tasks file can include smaller files if warranted
        handlers/         #
            main.yml      #  <-- handlers file
        templates/        #  <-- files for use with the template resource
            ntp.conf.j2   #  <------- templates end in .j2
        files/            #
            bar.txt       #  <-- files for use with the copy resource
            foo.sh        #  <-- script files for use with the script resource
        vars/             #
            main.yml      #  <-- variables associated with this role
        defaults/         #
            main.yml      #  <-- default lower priority variables for this role
        meta/             #
            main.yml      #  <-- role dependencies

    webtier/              # same kind of structure as "common" was above, done for the webtier role
    monitoring/           # ""
    fooapp/       
```

# Playbooks: Special Topics

## :point_right:加速模式

Ansible 1.5 或者 之后的版本,因为被称之为 “SSH pipelining” 的新特性

只需在你的 play 中添加 accelerate: true 即可使用加速模式

```yaml
---
- hosts: all
  accelerate: true
  tasks:
  - name: some task
    command: echo {{ item }}
    with_items:
    - foo
    - bar
    - baz
```

## :point_right:异步操作和轮询

默认情况下playbook中的任务执行时会一直保持连接,直到该任务在每个节点都执行完毕.有时这是不必要的,比如有些操作运行时间比SSH超时时间还要长.

你也可以对执行时间非常长（有可能遭遇超时）的操作使用异步模式.

为了异步启动一个任务,可以指定其最大超时时间以及轮询其状态的频率.如果你没有为 poll 指定值,那么默认的轮询频率是10秒钟:

```yaml
---
- hosts: all
  remote_user: root
  tasks:
  - name: simulate long running op (15 sec), wait for up to 45 sec, poll every 5 sec
    command: /bin/sleep 15
    async: 45
    poll: 5
```

另外,如果你不需要等待任务执行完毕,你可以指定 poll 值为0而启用 “启动并忽略”

```yaml
---
- hosts: all
  remote_user: root
  tasks:
  - name: simulate long running op, allow to run for 45 sec, fire and forget
    command: /bin/sleep 15
    async: 45
    poll: 0
```

当你想对 “启动并忽略” 做个变种,改为”启动并忽略,稍后再检查”,你可以使用以下方式执行任务:

```yaml
---
# Requires ansible 1.8+
- name: 'YUM - fire and forget task'
  yum: name=docker-io state=installed
  async: 1000
  poll: 0
  register: yum_sleeper

- name: 'YUM - check on fire and forget task'
  async_status: jid={{ yum_sleeper.ansible_job_id }}
  register: job_result
  until: job_result.finished
  retries: 30
```

## 测试运行 check mode

```bash
ansible-playbook foo.yml --check
```

## Playbooks 中的错误处理

### :point_right:忽略错误

```yaml
- name: this will not be counted as a failure
  command: /bin/false
  ignore_errors: yes
```

### 控制对失败的定义

```yaml
- name: this command prints FAILED when it fails
  command: /usr/bin/example-command -x -y -z
  register: command_result
  failed_when: "'FAILED' in command_result.stderr"
```

## :point_right:标签

如果你有一个大型的 playbook,那能够只运行其中特定部分的配置而无需运行整个 playbook 将会很有用.

```yaml
tasks:
    - yum: name={{ item }} state=installed
      with_items:
         - httpd
         - memcached
      tags:
         - packages
    - template: src=templates/src.j2 dest=/etc/foo.conf
      tags:
         - configuration
```

运行指定tag

`ansible-playbook example.yml --tags "configuration,packages"`

跳过指定tag执行

`ansible-playbook example.yml --skip-tags "notification"`

你同样也可以对 roles 应用 tags:

```
roles:
  - { role: webserver, port: 5000, tags: [ 'web', 'foo' ] }
```

你同样也可以对基本的 include 语句使用 tag:

```
- include: foo.yml tags=web,foo
```

## Vault - 文件加密

vault 可以加密任何 Ansible 使用的结构化数据文件. 甚至可以包括 “group_vars/” 或 “host_vars/” inventory 变量, “include_vars” 或 “vars_files” 加载的变量, 通过 ansible-playbook 命令行使用 “-e @file.yml” 或 “-e @file.json” 命令传输的变量文件. Role 变量和所有默认的变量都可以被 vault 加密.

**创建加密文件**

```bash
ansible-vault create foo.yml
```

**修改加密文件**

```bash
ansible-vault edit foo.yml
```

**如果你希望变更密码,使用如下 命令**:

```bash
ansible-vault rekey foo.yml bar.yml baz.yml
```

**加密普通文件**

```bash
ansible-vault encrypt foo.yml bar.yml baz.yml
```

**解密已经加密的文件**

```bash
ansible-vault decrypt foo.yml bar.yml baz.yml
```

**查阅已加密的文件**

```bash
ansible-vault view foo.yml bar.yml baz.yml
```

### :point_right:在Vault加密下运行Playbook

密码交互

```bash
ansible-playbook site.yml --ask-vault-pass
```

自动读取

```bash
ansible-playbook site.yml --vault-password-file ~/.vault_pass.txt

ansible-playbook site.yml --vault-password-file ~/.vault_pass.py
```
