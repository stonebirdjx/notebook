# docker

```shell
docker login  #登录自己的信息到dockerhub
docker ps 	#列出容器
	-a :显示所有的容器，包括未运行的。
    -f :根据条件过滤显示的内容。
    --format :指定返回值的模板文件。
    -l :显示最近创建的容器。
    -n :列出最近创建的n个容器。
    --no-trunc :不截断输出。
    -q :静默模式，只显示容器编号。
    -s :显示总的文件大小。d
docker images #查看所有镜像
docker run  #创建一个新的容器并运行
    -a stdin: 指定标准输入输出内容类型，可选 STDIN/STDOUT/STDERR 三项；
    -d: 后台运行容器，并返回容器ID；
    -i: 以交互模式运行容器，通常与 -t 同时使用；
    -P: 随机端口映射，容器内部端口随机映射到主机的端口
    -p: 指定端口映射，格式为：主机(宿主)端口:容器端口
    -t: 为容器重新分配一个伪输入终端，通常与 -i 同时使用；
    --name="nginx-lb": 为容器指定一个名称；
    --dns 8.8.8.8: 指定容器使用的DNS服务器，默认和宿主一致；
    --dns-search example.com: 指定容器DNS搜索域名，默认和宿主一致；
    -h "mars": 指定容器的hostname；
    -e username="ritchie": 设置环境变量；
    --env-file=[]: 从指定文件读入环境变量；
    --cpuset="0-2" or --cpuset="0,1,2": 绑定容器到指定CPU运行；
    -m :设置容器使用内存最大值；
    --net="bridge": 指定容器的网络连接类型，支持 bridge/host/none/container: 四种类型；
    --link=[]: 添加链接到另一个容器；
    --expose=[]: 开放一个端口或一组端口；
    --volume , -v: 绑定一个卷

```



# Postgres

```shell
# 拉取官方镜像
docker pull postgres

#运行 postgres
docker run -d --name postgres --restart always -e POSTGRES_PASSWORD='db10$ZTE' -e ALLOW_IP_RANGE=0.0.0.0/0 -v /home/postgres/data:/var/lib/postgresql -p 5432:5432  1245863260/postgres:v1

#进入容器
docker exec -it containerid /bin/bash
docker exec -ti b1b96142bd14 /bin/bash

#进去pg
psql -h 127.0.0.1 -U postgres
# 创建数据库
CREATE DATABASE stonebird;
\c stonebird
# 创建SCHEMA  \dn 查看所以schema
create schema gin;
grant usage on schema gin to gin;
grant all privileges on schema gin to gin;


```

