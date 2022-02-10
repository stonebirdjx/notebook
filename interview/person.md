# 互联网安全备案

互联网安全备案
	账号：hjxtest
	密码：sF+aPK+eirLzgt8

# 固戍宽带账号：
SZLAN13619563100@16900.gd
123456

# 云服务器

IP: 42.193.185.126

密码：RnKQ5j7!RvH_2Yk

ssh -p22222 gordon@106.13.178.91   

gordon/intel@2020   root/intel@2020

同步时钟

ntpdate  cn.pool.ntp.org

docker run -d --name=grafana -p 3000:3000 grafana/grafana

docker run -d -p 9090:9090 --name=prometheus prom/prometheus

# go 环境变量

export GOROOT=/usr/local/go
export GOPATH=/go
export PATH=$PATH:/usr/local/go/bin

# openssl 生成证书

安装64位的openssl、配置好环境变量

生成key：openssl genrsa -des3 -out lee.key 1024 
	需要输入密码

创建csr证书 openssl req -new -key lee.key -out lee.csr
	输入的信息中最重要的为 Common Name，这里输入的域名即为我们要使用https访问的域名。其他随意输入

copy lee.key lee.key.org
openssl rsa -in lee.key.org -out lee.key 

生成:openssl x509 -req -days 365 -in lee.csr -signkey lee.key -out lee.crt

nginx.conf配置项（conf目录下）ssl_certificate      ssl\hjxssl.crt;
        					  ssl_certificate_key  ssl\hjxssl.key;



# KeyManager秘钥库

7EDB-56B9-FCA7-E842 9FAE-D110-19BD-9DA4 DACC-F538-F75C-C31A 789E-C23E-87C9-A37A



# Prometheus

进入目录

D:\prometheus-2.32.1.windows-amd64

prometheus.exe

http://localhost:9090/graph



# grafana

D:\grafana-8.3.3\bin

grafana-server.exe

admin / d!e_m#9h_y4zaFv
