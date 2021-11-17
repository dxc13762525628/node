# Docker

docker官网地址:https://docs.docker.com/

仓库地址:https://hub.docker.com/

视频地址：https://www.bilibili.com/video/BV1og4y1q7M4?p=1 

## docker 概述

### Docker为什么会出现?

解决部署与开发的问题(环境+代码)打包

隔离:Docker的核心思想!每个箱子是相互隔离的

![](.\img\POPO20211108-130230.png)

本质：以前部署环境台复杂，需要有一个代码+环境打包的东西出现，这个就是docker

### Docker历史

2010,几个搞IT的年轻人，美国成立dotCloud,做一些pass云计算服务,将自己的技术(容器化技术),统一简化命名就是Docker,刚刚诞生的时候也是没行业注意，然后就想着<u>开源</u>,然后越开越多的人发现docker优点，到2014年4月1.0发布，火的原因就是轻巧

虚拟机:在window中装一个vm,在安装镜像,也是属于虚拟化技术

Docker:容器技术容器技术，也是一种虚拟化技术

官网很详细

### Docker能做什么

虚拟机技术缺点:

1 冗余十分多

2 资源占用多

3 启动慢

容器化技术:

1 资源占用小

2 启动块

3 各个容器之间相互隔离

传统虚拟机:虚拟出一套硬件，运行一个完整的操作系统，然后再这个系统上安装和运行

容器内的应用直接运行在宿主机中，没有自己的内核和硬件

容器与容器之间相互隔离，每个容器有自己的文件系统

#### DevOps

应用更快速的交付和部署

更便捷的升级和扩缩容

更简单的系统运维

更高级的计算机资源利用

## Docker 安装

### Docker的基本组成

![](.\img\POPO20211108-200243.png)

#### 镜像(image):

docker就像是一个模板，可以通过模板在创建容器服务，一个镜像可以创建多个服务

#### 容器(container):

Docker利用容器技术可以独立运行一组应用，通过镜像来创建

可以理解未一个简易的linux系统

#### 仓库(reposiyory):

存放镜像的地方

分为共有和私有的仓库

Docker Hub(默认比较慢)

阿里云等容器服务（配置镜像加速）

### 安装

#### 环境准备

会点linux

centos(其他也行)

xshell

#### 开始安装

```shell
# 环境查看
uname -r
# 查看系统配置
cat /etc/os-release
# 安装 先查看帮组文档https://docs.docker.com/engine/install/centos/
#第一步 先卸载旧版本
sudo yum remove docker \
                docker-client \
                docker-client-latest \
                docker-common \
                docker-latest \
                docker-latest-logrotate \
                docker-logrotate \
                docker-engine
# 使用仓库安装

# 1.安装需要的安装包
sudo yum install -y yum-utils
# 2.设置镜像的仓库
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo # 默认国外
# 2.使用国内的(推荐使用)
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
# 3.更新软件包索引
yum makecache fast
# 4.安装docker相关的内容 docker-ce 社区版 docker-ee企业版
yum install docker-ce docker-ce-cli containerd.io
# 5.启动docker
systemctl start docker 
# 6.查看是否启动成功
docker version
# 7. 测试helloworld
docker run hello-world
# 8. 查看下载的hello-world镜像是否存在 存在则成功了
docker images
```

### 卸载docker

```shell
# 卸载以来
sudo yum remove docker-ce docker-ce-cli containerd.io
# 删除资源 
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

### 阿里云镜像加速

```python
登录阿里云-->找到镜像加速器-->配置使用
```

### docker run做的事情

```python
开始
先到本地找这个镜像是否有，有使用本机的
没有就去中央仓库找，有就拉去到本地
在去运行
```

### 底层(工作)原理

```python
docker 是一个client/server结构的系统，Docekr的守护进程运行在主机上，通过Socket从客户端访问，
s端接收到c端的指令,并执行
docker 有着更少的抽象层
docker 利用的是主机的内核
```

![](.\img\docker运行原理.png)

## Docker基本命令

### 帮助命令

```python
docker version		版本
docker info			详情的信息
docker 命令 --help   docker 所有的命令
```

地址:https://docs.docker.com/engine/reference/commandline/

### 镜像命令

#### docker images  

查看主机的所有镜像

```shell
root@mtl-unknown659404:/home/duanxingcai/update_script# docker images
REPOSITORY                    TAG                            IMAGE ID       CREATED        SIZE
composetest_web               latest                         b06b246937c1   2 days ago     183MB

# 解释
REPOSITORY			仓库源
TAG					版本
IMAGE ID			镜像的id
CREATED				创建时间
SIZE				大小

# 可选项
-a					# 列出所有镜像
-q					# 只显示镜像的id
```

#### docker search

搜索镜像

```shell
root@mtl-unknown674522:/home/duanxingcai# docker search mysql --filter=STARS=3000
NAME                              DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                             MySQL is a widely used, open-source relation…   11661     [OK]

# 筛选
--filter
```

#### docker pull:[tag]

下载命令

```shell
root@mtl-unknown659404:/home/duanxingcai# docker pull mysql
Using default tag: latest      # 下载不写tag默认最新
latest: Pulling from library/mysql
b380bbd43752: Already exists   # 分层下载 docker的核心
f23cbf2ecc5d: Already exists
30cfc6c29c0a: Already exists
b38609286cbe: Already exists
8211d9e66cd6: Already exists
2313f9eeca4a: Already exists
7eb487d00da0: Already exists
4d7421c8152e: Pull complete
77f3d8811a28: Pull complete
cce755338cba: Pull complete
69b753046b9f: Pull complete
b2e64b0ab53c: Pull complete
Digest: sha256:6d7d4524463fe6e2b893ffc2b89543c81dec7ef82fb2020a1b27606666464d87 # 签名 防伪标志
Status: Downloaded newer image for mysql:latest	
docker.io/library/mysql:latest # 真实地址

```

#### docker rmi

删除镜像

```shell
root@mtl-unknown659404:/home/duanxingcai# docker rmi b06b246937c1(容器id)
Untagged: composetest_web:latest
Deleted: sha256:b06b246937c1fe601c84ef6170b25e6e721e64fc36cd835b6adf20478b444667
Deleted: sha256:3a8cf349ecbfe4927451de447ff9cf4748991b647602116a282947c76c8aa667
Deleted: sha256:c9639f654a812c72bfa7f911adead938fc3d10afdf08207ca521286bb2311e40
Deleted: sha256:d70966e3232da4ee946fb8b339e9d1d080950d054f6b3712722d31fa850dbcc9
Deleted: sha256:c9bb3c018c4c6ab46b1cb315cf45123741189e74363775acdcf6e4860d6dff50
Deleted: sha256:b32a97d0c435df82af0afcc1200a312d05eb3c07113689d2697b16710e05d961
Deleted: sha256:6ba97b1792add59485ae97dc849a12042d2b0635c00593a18c71358c5b8e4ae6
Deleted: sha256:aa5597527bece112a59aeb5725cd1ceb58ddbf164f2281629da0f18466b93d46
Deleted: sha256:cea6af8639c792df33e9aba82c87ee2921f30e55626c2b3a0e0e5c0620e7aafe
Deleted: sha256:d3fd4836fb369af6f153e945d00423a1b534e013a6b098dfc8bbc6bd545c76ea
Deleted: sha256:221835d99144de7c4a28dad25de2a064ef8ced101c09a68e1e6b2e58c006ac05
Deleted: sha256:8c5583df27870ceb3c21f5f1bd07319e8f886eb3aa1a066295b04e331a8b2bd0
Deleted: sha256:7b6a7acaac1c415014b75ef09053734b70a08b998a8d2143d92154e92bf6c2e2
Deleted: sha256:43c759b4147dac2a7c22f03890ca2b86d9b21a3b86035f2b0cfa02e4a14a2bce

# 删除全部镜像
docker rmi -f $(docker images -qa)
```

### 容器命令

有了容器才有镜像

```she
root@mtl-unknown659404:/home/wb.duanxingcai/update_script# docker pull centos
Using default tag: latest
latest: Pulling from library/centos
a1d0c7532777: Pull complete
Digest: sha256:a27fd8080b517143cbbbab9dfb7c8571c40d67d534bbdee55bd6c473f432b177
Status: Downloaded newer image for centos:latest
docker.io/library/centos:latest
```

#### docker run 

启动容器

```shell
docker run [可选参数] image

# 参数说明
--name = 'Name'   # 容器名字 用于区分容器
-d                # 后台启动
-it               # 使用交互方式运行，进入容器查看内容
-p                # 指定端口，端口映射(主机端口:容器端口)
-P				  # 随机指定端口

# 启动并进入容器
root@mtl-unknown659404:/home/duanxingcai/update_script# docker run -it centos /bin/bash
[root@ed19ad779bd7 /]# ls #内部centos
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@ed19ad779bd7 /]# exit  # 从容器中退到主机
exit
```

#### docker ps

查看所有正在运行的容器

```shell
CONTAINER ID   IMAGE                                      COMMAND                  CREATED        STATUS             PORTS                          NAMES
782ed87417d7   redis                         "/docker-entrypoint.…"   4 weeks ago    Up About an hour   80/tcp, 0.0.0.0:1011->81/tcp   redis

#  参数
-a 		# 查询所有的
-n 		# 最近创建的容器
-aq		# 所有的容器编号
```

#### exit

退出容器

```shell
exit          # 退出并停止
ctrl + P +Q   # 退出不停止
```

#### docker rm

删除容器

``` shell
root@mtl-unknown659404:/home/duanxingcai# docker rm keen_knuth
keen_knuth

# 
docker rm 容器id（名字）        		 	#删除一个容器
docker rm -f $(docker ps -aq) 			#强制全部删除
docker ps -a -q|xargs docker rm         # 全部删除
```

#### docker start/restart/stop

```shell
docker start 容器id		# 启动容器
docker restart 容器id		# 重启
docker stop 容器id	    # 停止
docker kill 容器id		# 杀掉
```

### 常用的其他命令

#### docker run -d

后台启动

```shell
root@mtl-unknown659404:/home/duanxingcai# docker run -d centos /bin/bash
c3591f865030638c5086385d7b266f8e35617bee4e2c94e1b8ce9779c48c1358
# docke ps 发现停止 原因后台运行时，没有前台进程，会立刻停止 
```

#### docker logs

查看日志

```shell
docker logs --help
# 查看最近的10条日志
root@mtl-unknown659404:/home/duanxingcai# docker logs -f -t --tail 10 a9459d51e6df
2021-11-09T09:11:16.032042212Z 1:M 09 Nov 2021 09:11:16.031 * 1 changes in 3600 seconds. Saving...
2021-11-09T09:11:16.035570155Z 1:M 09 Nov 2021 09:11:16.032 * Background saving started by pid 58
2021-11-09T09:11:16.042593956Z 58:C 09 Nov 2021 09:11:16.042 * DB saved on disk
2021-11-09T09:11:16.043663030Z 58:C 09 Nov 2021 09:11:16.043 * RDB: 2 MB of memory used by copy-on-write
2021-11-09T09:11:16.133033973Z 1:M 09 Nov 2021 09:11:16.132 * Background saving terminated with success
2021-11-09T10:11:17.028137561Z 1:M 09 Nov 2021 10:11:17.027 * 1 changes in 3600 seconds. Saving...
2021-11-09T10:11:17.028897598Z 1:M 09 Nov 2021 10:11:17.028 * Background saving started by pid 59
2021-11-09T10:11:17.038055815Z 59:C 09 Nov 2021 10:11:17.037 * DB saved on disk
2021-11-09T10:11:17.038579029Z 59:C 09 Nov 2021 10:11:17.038 * RDB: 0 MB of memory used by copy-on-write
2021-11-09T10:11:17.129418779Z 1:M 09 Nov 2021 10:11:17.129 * Background saving terminated with success

```

#### docker top

查看容器中的进程信息

```shell
# 命令
docker top 容器id

root@mtl-unknown659404:/home/duanxingcai# docker top a9459d51e6df(容器id)
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
systemd+            19545               19512               0                   10月21               pts/0               00:48:10            redis-server *:6379
```

#### docker inspect

查看镜像的原数据

```shell
# 命令
docker inspect 容器id

root@mtl-unknown659404:/home/duanxingcai# docker inspect a9459d51e6df
[
    {
        "Id": "a9459d51e6dfdbc7051b344723ad444639e67f87ca957c4106b79b0fd99c2fb6",
        "Created": "2021-08-17T02:31:05.710685955Z",
        "Path": "docker-entrypoint.sh",
        "Args": [
            "redis-server"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 19545,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2021-10-21T06:01:42.959655809Z",
            "FinishedAt": "2021-10-21T05:58:46.649500616Z"
        },
        ......
```

#### docker exec 

进入当前正在运行的容器

```shell
# 命令
docker exec -it 容器id /bin/bash

root@mtl-unknown659404:/home/duanxingcai# docker exec -it redis /bin/bash
root@a9459d51e6df:/data#  # 容器内部


# 方式二
docker attach 容器id
# 进入正在运行的容器内部 不是进入命令行
root@mtl-unknown659404:/home/duanxingcai# docker attach redis /bin/bash
# 正在执行当前的代码

# docker exec			进入容器后来时一个新的终端
# docker attach    	    进入容器正在执行的终端，不会启动新的进程
```

#### docker cp

拷贝容器与主机的东西

```shell
docker cp 容器id:容器内路径 目的主机路径

# 从宿主机拷贝到容器内部
root@mtl-unknown659404:/home/duanxingcai# ls
co_test  co_test.zip  update_script
root@mtl-unknown659404:/home/duanxingcai# docker cp co_test redis:/
root@mtl-unknown659404:/home/duanxingcai# docker exec -it redis /bin/bash
root@a9459d51e6df:/data# ls
dump.rdb
root@a9459d51e6df:/data# cd ..
root@a9459d51e6df:/# ls
bin  boot  co_test  data  dev  etc  home  lib  lib64  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
root@a9459d51e6df:/#

# 从容器内拷贝到宿主机
root@mtl-unknown659404:/home/duanxingcai# docker cp redis:/test.py /home/duanxingcai/
root@mtl-unknown659404:/home/duanxingcai# ls
co_test  co_test.zip  test.py  update_script

# 拷贝是手动过程，未来使用容器卷打通，自动拷贝
```

### 练习

#### docker 安装nginx

```shell
# 搜索镜像
docker search nginx
# 下载镜像
docker pull nginx
# 启动容器
docker run -d --name nginx01 -p 3344:80 nginx
# 查询启动容器
docker ps
# 本机自测
root@mtl-unknown659404:/home/wb.duanxingcai# curl localhost:3344
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
# 查询nginx配置文件
whereis nginx
```

一个小问题:容器外部修改，内部也跟着修改

#### docker 安装tomcat

```shell
# 1. 官方使用
docker run -it --rm tomcat:9.0 
# 我们之前的启动都是后台，停止之后容器还在运行， 官方是用完就直接删除，测试用完及删除

# 正常逻辑
# 1. 下载
docker pull tomcat:9.0
# 启动运行
docker run -d --name tomcat-test -p 3355:8080 tomcat:9.0
# 进入容器
docker exec -it tomcat-test /bin/bash
# linux命令少了，没有webapps 因为默认是最小的镜像，把所有不必要的全丢了
# 方式一
root@825dcddc0fe1:/usr/local/tomcat# cp -r webapps.dist/* webapps
# 访问就可以访问到tomcat了
```

#### docker 部署es+kibana

```shell
# es 暴露的端口很多 还很耗内存 数据需要挂载到安全目录
# 官方命令
# --net somenetwork 网络配置
docker run -d --name elasticsearch --net somenetwork -p 9200:9200 -p 9300:9300 -e "discovery.tyoe=single-node" elasticsearch:7.6.2

# 启动
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.tyoe=single-node" elasticsearch:7.6.2
# 查看cpu状态
docker stats
# 添加内存限制 -e参数
docker run -d --name elasticsearch -p 9200:9200 -p 9300:9300 -e "discovery.tyoe=single-node" -e ES_JAVA_OPTS="-Xms64m -Xms512m" elasticsearch:7.6.2
# 再次查看就发现内存变小了
```

### 可视化

portainer(镜像管理工具)

```shell
# 图形化的 登录才行
docker run -d -p 8088:9000 --restart=always -v /var/run/docker.sock:/var/run/docker.sock --privileged=true protainer/protainer
```

Rancher(CI/CD再用)

## Docker镜像

### 镜像是什么

镜像是卿连吉，可执行的独立软件包，用来打包软件运行环境和基于环境开发的软件，它包含运行某个软件所需的所有内容，包括代码，运行时，库，环境变量等

### docker 镜像加载原理

联合文件系统,一种分层，轻量级，并且高性能的文件系统，一次同时加载多个文件系统，从外面看是一个，内部是一层一层叠加起来形成最终的文件和目录

### commit镜像

```shell
docker commit 提交容器成为新的副本
# 命令与git类型
docker commit -m 提交的描述信息 -a 作者 容器id 目标镜像名:[tag]
```

## 容器数据卷

### 什么事容器数据卷

#### docker的理念回顾

将应用和环境打包成一个镜像

数据会随着容器的删除而删除，需要数据可以持久化，需要将数据进行持久化，需要容器和宿主机之间可以共享，这就是卷技术

其实就是将容器内的目录，挂载到linux上，其实就是同步机制,容器间也是数据共享的

### 使用数据卷

```shell
# 直接使用命令挂载 -v 就是双向绑定
# 删除也是双向的 如果容器停止了也是可以同步的
docker run -it -v 主机目录:容器内部目录
```

#### 测试使用

```shell
root@mtl-unknown659404:/home/wb.duanxingcai# docker run -it -v /home/wb.duanxingcai/censhi:/home centos /bin/bash
[root@7b8ed85f676c /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@7b8ed85f676c /]# cd home/
[root@7b8ed85f676c home]# ls
[root@7b8ed85f676c home]# touch demo.sh
[root@7b8ed85f676c home]# ls
demo.sh
[root@7b8ed85f676c home]# root@mtl-unknown659404:/home/wb.duanxingcai# LS
bash: LS：未找到命令
root@mtl-unknown659404:/home/wb.duanxingcai# ls
censhi  update_script
root@mtl-unknown659404:/home/wb.duanxingcai# cd censhi/
root@mtl-unknown659404:/home/wb.duanxingcai/censhi# ls
demo.sh
root@mtl-unknown659404:/home/wb.duanxingcai/censhi# touch demo01.sh
root@mtl-unknown659404:/home/wb.duanxingcai/censhi# docker ps
CONTAINER ID   IMAGE                                      COMMAND                  CREATED          STATUS             PORTS                          NAMES
7b8ed85f676c   centos                                     "/bin/bash"              37 seconds ago   Up 35 seconds                                     ecstatic_cerf
782ed87417d7   cotest_client_test                         "/docker-entrypoint.…"   5 weeks ago      Up About an hour   80/tcp, 0.0.0.0:1011->81/tcp   cotest_client_test
47072b7959cf   cotest_server_test                         "/bin/bash"              5 weeks ago      Up About an hour   0.0.0.0:1012->8004/tcp         cotest_service_test
e1566a5829c8   mtl_datacenter_client_test                 "/docker-entrypoint.…"   7 weeks ago      Up 3 days          0.0.0.0:9015->80/tcp           mtl_datacenter_client_test
266a2ac69438   mtl_datacenter_service_test                "/bin/bash"              7 weeks ago      Up 3 weeks         0.0.0.0:9014->8003/tcp         mtl_datacenter_service_test
849b93dfed9f   minio/minio:RELEASE.2021-06-17T00-10-46Z   "/usr/bin/docker-ent…"   7 weeks ago      Up 3 weeks         0.0.0.0:9000->9000/tcp         minio
255b8277f013   mongo                                      "docker-entrypoint.s…"   7 weeks ago      Up 3 weeks         0.0.0.0:27017->27017/tcp       mongo
d9989d723810   cotest_service                             "/bin/bash"              2 months ago     Up 3 days          0.0.0.0:8017->8001/tcp         cotest_service
eb0b6291f576   co_test_client:v0.0.1                      "/docker-entrypoint.…"   2 months ago     Up 6 hours         0.0.0.0:8016->80/tcp           cotest_client
a9459d51e6df   redis                                      "docker-entrypoint.s…"   3 months ago     Up 3 weeks         0.0.0.0:6379->6379/tcp         redis
root@mtl-unknown659404:/home/wb.duanxingcai/censhi# docker exec -it ecstatic_cerf /bin/bash
[root@7b8ed85f676c /]# cd home/
[root@7b8ed85f676c home]# ls
demo.sh  demo01.sh
```

修改只要再本地修改即可，容器内部会自动同步

### mysql数据同步

数据持久化都是需要

#### 获取镜像

```shell
# 拉取
docker pull mysql
# 启动 需要配置密码
# 官方测试
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7

# 自己测试
root@mtl-unknown659404:/home/wb.duanxingcai# docker run -d -p 3310:3306 -v /home/wb.duanxingcai/mysql/conf:/etc/mysql/conf.d -v /home/wb.duanxingcai/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7
# 启动成功后测试链接

# 测试数据库，发现映射路径是有对应的数据库的那就成功了
```

### 具名挂载和匿名挂载

```shell
# 匿名挂载 docker 会自动生成宿主机路径
-v 容器内路径！ 

# 查看所有卷本地镜像
root@mtl-unknown659404:/home/wb.duanxingcai# docker volume ls
DRIVER    VOLUME NAME
local     ed3a0fda4cec4e44f1b12b0667cb509ba62d3ea8248982b735dde452256964f4 # 匿名的卷 没有写容器外的路径
local     efe201c48fc28039bce9264699656c126a9573dcc3a43b93bbf38b147228f827
local     f4ee680d7feafb42cb18f0bea8aadaa10ebda37f9f38612573d0d1417e5dd2ce
local     f26c3c2105f0aec0763aed4b05ccbb3ff1dcfdea0b8bc823fef3dbecd4cbeb45
local     f55aa24e2951d33fbf6391c834a9911621dff71e7feccdb700b961154e6fb222
local     my_worldpress_db_data                                            # 具名挂载  
local     my_worldpress_wordpress_data

# 具名挂载
# -v 卷名:容器内路径
# 查看这个卷
docker volume inspect 卷名
root@mtl-unknown659404:/home/wb.duanxingcai# docker volume inspect my_worldpress_db_data
[
    {
        "CreatedAt": "2021-11-06T18:52:26+08:00",
        "Driver": "local",
        "Labels": {
            "com.docker.compose.project": "my_worldpress",
            "com.docker.compose.version": "1.29.2",
            "com.docker.compose.volume": "db_data"
        },
        "Mountpoint": "/home/docker/volumes/my_worldpress_db_data/_data",
        "Name": "my_worldpress_db_data",
        "Options": null,
        "Scope": "local"
    }
]

```

大多数使用的是具名挂载

```shell
# 区别
-v 容器内路径 # 匿名挂载
-v 卷名:容器内路径 # 具名挂载
-v 宿主机路径:容器内路径 # 指定路径挂载
```

#### 拓展

```shell
# 通过-v 容器内路径，ro,rw改变读写权限
ro 只读    # 只能外部改变
rw 可读可写 # 默认 可读可写

# 一旦设置这个容器权限，容器对挂载出来的内容就有限定了
docker run -d -P --name nginx02 -v juming-nginx:/etc/nginx:ro(rw) nginx
# ro/rw
```

### 初识dockerfile 

用来构建docker镜像的文件,通过这个脚本可以生成一个镜像

镜像是一层一层的，脚本就是一个一个的

```shell
# dockerfile文件 指令全是大写
FROM centos
VOLUME ["volume01","vloume02"]   # 挂载数据

CMD echo "-------------------"
CMD /bin/bash

# 这里的每个命令就是层的感觉
# 实际构建
oot@mtl-unknown659404:/home/wb.duanxingcai/docker-test-volume# docker build -f dockerfile1 -t test .
Sending build context to Docker daemon  6.656kB
Step 1/4 : FROM centos
 ---> 5d0da3dc9764
Step 2/4 : VOLUME ["volume01","vloume02"]  # 匿名挂载
 ---> Running in 4fbe58da543f
Removing intermediate container 4fbe58da543f
 ---> d66854494691
Step 3/4 : CMD echo "-------------------"
 ---> Running in 346161d290d1
Removing intermediate container 346161d290d1
 ---> 413075db533d
Step 4/4 : CMD /bin/bash
 ---> Running in 8d5140a0090e
Removing intermediate container 8d5140a0090e
 ---> 65085e75aad8
Successfully built 65085e75aad8
Successfully tagged test:latest
```

```shell
# 启动自己生成的容器
root@mtl-unknown659404:/home/wb.duanxingcai/docker-test-volume# docker run -it test /bin/bash
[root@4498c0b1df2f /]# ls -l
total 0
lrwxrwxrwx   1 root root   7 Nov  3  2020 bin -> usr/bin
drwxr-xr-x   5 root root 360 Nov 15 12:05 dev
drwxr-xr-x   1 root root  66 Nov 15 12:04 etc
drwxr-xr-x   2 root root   6 Nov  3  2020 home
lrwxrwxrwx   1 root root   7 Nov  3  2020 lib -> usr/lib
lrwxrwxrwx   1 root root   9 Nov  3  2020 lib64 -> usr/lib64
drwx------   2 root root   6 Sep 15 14:17 lost+found
drwxr-xr-x   2 root root   6 Nov  3  2020 media
drwxr-xr-x   2 root root   6 Nov  3  2020 mnt
drwxr-xr-x   2 root root   6 Nov  3  2020 opt
dr-xr-xr-x 208 root root   0 Nov 15 12:04 proc
dr-xr-x---   2 root root 162 Sep 15 14:17 root
drwxr-xr-x  11 root root 163 Sep 15 14:17 run
lrwxrwxrwx   1 root root   8 Nov  3  2020 sbin -> usr/sbin
drwxr-xr-x   2 root root   6 Nov  3  2020 srv
dr-xr-xr-x  13 root root   0 Nov 15 12:04 sys
drwxrwxrwt   7 root root 171 Sep 15 14:17 tmp
drwxr-xr-x  12 root root 144 Sep 15 14:17 usr
drwxr-xr-x  20 root root 262 Sep 15 14:17 var
drwxr-xr-x   2 root root   6 Nov 15 12:04 vloume02    # 生成镜像的时候自动挂载的，数据卷目录
drwxr-xr-x   2 root root   6 Nov 15 12:04 volume01
# 这个卷和外部是有同步的目录 
# 匿名挂载肯定是hash码

#查看卷挂载的路径
docker inspect 容器名字
root@mtl-unknown659404:/home/wb.duanxingcai/docker-test-volume# docker inspect charming_margulis
[
    {
		....
		 "Mounts": [
            {
                "Type": "volume",
                "Name": "8fdf7d8d23352552635acb44a7e79ea82780437db8e8bc55e04a81600f2b8e2c",
                "Source": "/home/docker/volumes/8fdf7d8d23352552635acb44a7e79ea82780437db8e8bc55e04a81600f2b8e2c/_data",
                "Destination": "vloume02",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            },
            {
                "Type": "volume",
                "Name": "eb8db928accfbb954a4878d65dd9c1a0edfe1894c63dd23f98adfe961e40e8bf",
                "Source": "/home/docker/volumes/eb8db928accfbb954a4878d65dd9c1a0edfe1894c63dd23f98adfe961e40e8bf/_data",
                "Destination": "volume01",
                "Driver": "local",
                "Mode": "",
                "RW": true,
                "Propagation": ""
            }
        ],
```

这种方式用的多，假设没有挂载卷，需要自己配置，如果dockerfile时，启动容器不需要-v了

### 数据卷容器

两个mysql同步数据

```shell
# --volume-from
# 测试使用 启动3个容器测试

root@mtl-unknown659404:/home/wb.duanxingcai/docker-test-volume# docker run -it --name docker01 test
[root@3c75c4c26037 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  vloume02  volume01
[root@3c75c4c26037 /]# root@mtl-unknown659404:/home/wb.duanxingcai/docker-test-volume#
root@mtl-unknown659404:/home/wb.duanxingcai/docker-test-volume# docker run -it --name docker02 --volume-from docker01 test
unknown flag: --volume-from
See 'docker run --help'.
root@mtl-unknown659404:/home/wb.duanxingcai/docker-test-volume# docker run -it --name docker02 --volumes-from docker01 test
[root@02388c9144ba /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var  vloume02  volume01
# 中间去docker01的/volume01下中创建一个文件demo01.sh
[root@02388c9144ba /]# cd volume01/
[root@02388c9144ba volume01]# ls
demo01.sh
[root@02388c9144ba volume01]# root@mtl-unknown659404:/home/wb.duanxingcai/docker-test-volume#
root@mtl-unknown659404:/home/wb.duanxingcai/docker-test-volume# docker run -it --name docker03 --volumes-from docker01 test
[root@76f5e05be8ef /]# cd volume01/
[root@76f5e05be8ef volume01]# ls
demo01.sh
[root@76f5e05be8ef volume01]# touch docker03.sh
[root@76f5e05be8ef volume01]#
# 去docekr01中去查看文件
```

```shell
# 删除任何一个容器，不影响其余容器的数据，就是数据备份的感觉
```

mysql的操作

```shell
root@mtl-unknown659404:/home/wb.duanxingcai# docker run -d -p 3310:3306 -v /home/wb.duanxingcai/mysql/conf:/etc/mysql/conf.d -v /home/wb.duanxingcai/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql:5.7

root@mtl-unknown659404:/home/wb.duanxingcai# docker run -d -p 3310:3306 -e MYSQL_ROOT_PASSWORD=123456 --name mysql00 --volumes-from mysql01 mysql:5.7

# 这个时候两个容器数据就是同步的
```

结论:容器之间配置信息的传递，数据卷容器的生命周期，一直持续到没有容器使用为止

一旦持久化到了本地，容器删除，本地数据是不会被删除

## DockerFile

用来构建docker镜像的文件,命令参数脚本

构建步骤

1. 编写一个dockerfile文件
2. docker build构建镜像
3. docker run 运行一个镜像
4. docker push 发布镜像

### DockerFile构建过程

#### 基础知识

每个保留关键字(都必须是大写字母)

执行从上到下

#好表示注释

每个指令都会创建一个镜像层并提交

dockerfile是面向开发的，我们以后要发布项目，做镜像，就需要编写dockerfile文件，这个文件简单

DockerFile：构建文件，定义了一切的步骤，源代码

DockerImages:DockerFile构建生成的镜像，最终发布和运行的产品

Docker容器：docker镜像运行提高服务的

### DockerFile的指令

```python
FROM    			# 基础镜像,一切从这开始
MAINTAINER			# 镜像是谁写的，姓名+邮箱
RUN					# 镜像构建的时候需要运行的命令
ADD					# 添加内容,COPY文件，会自动解压
WORKER				# 工作目录,启动会默认进入的目录
VOLUME				# 容器卷，挂载的目录位置
EXPOSE				# 指定对外暴露的端口
CMD					# 指定这个容器启动的时候要运行的命令,只有最后一个会生效，可被替代
ENTRYPOINT			# 指定这个容器启动的时候要运行的命令，可追加命令
ONBUILD				# 当构构建一个被继承DockerFile这个时候，会运行ONBUILD的指令
COPY				# 将文件拷贝到镜像中
ENV					# 构建的时候设置环境变量
```

### 实战测试

#### 自己的centos

```python
# 编写文件
FROM centos
MAINTAINER xxx<xxx@163.com>

ENV MYPATH /usr/local
WORKDIR $MYPATH

RUN yum -y install vim
RUN yum -y install net-tools

EXPOSE 81
CMD echo $MYPATH
CMD echo "---------"
CMD /bin/bash

# 构建自己的镜像
# -f 是dockerfile的文件路径 -t是镜像的名字
docker build -f mydockerfile -t mycentos-1 .

# 测试运行
docker run -it mycentos-1
# 此时已经有了自己的命令操作

# 查看镜像构建过程
root@mtl-unknown659404:/home/wb.duanxingcai/dockerfile# docker history 43d3db323c6d
IMAGE          CREATED         CREATED BY                                      SIZE      COMMENT
43d3db323c6d   5 minutes ago   /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "/bin…   0B
5048b5e42441   5 minutes ago   /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "echo…   0B
020c28329eaf   5 minutes ago   /bin/sh -c #(nop)  CMD ["/bin/sh" "-c" "echo…   0B
bb126bfac927   5 minutes ago   /bin/sh -c #(nop)  EXPOSE 81                    0B
38d14e09f111   5 minutes ago   /bin/sh -c yum -y install net-tools             25.7MB
f080fcbc3015   5 minutes ago   /bin/sh -c yum -y install vim                   62.1MB
1f1dc341e8db   5 minutes ago   /bin/sh -c #(nop) WORKDIR /usr/local            0B
f71f1e0e5991   5 minutes ago   /bin/sh -c #(nop)  ENV MYPATH=/usr/local        0B
8c53eda216e2   5 minutes ago   /bin/sh -c #(nop)  MAINTAINER xxx<xxx@163.co…   0B
5d0da3dc9764   2 months ago    /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B
<missing>      2 months ago    /bin/sh -c #(nop)  LABEL org.label-schema.sc…   0B
<missing>      2 months ago    /bin/sh -c #(nop) ADD file:805cb5e15fb6e0bb0…   231MB
# history是可以查询镜像的制作过程
```

> ### CMD和ENTRYPOINT的区别

测试cmd

```shell
# 编写dockerfile文件
FROM centos
CMD ["ls","-a"]

# 构建镜像
docker build -f dockerfile-cmd-test -t mycentos-3 .
# 启动
docker run mycentos-3
.
..
.dockerenv
bin
.....

# 追加一个命令-l ls -al
oot@mtl-unknown659404:/home/wb.duanxingcai/dockerfile# docker run mycentos-3 -l
docker: Error response from daemon: OCI runtime create failed: container_linux.go:380: starting container process caused: exec: "-l": executable file not found in $PATH: unknown.
ERRO[0001] error waiting for container: context canceled

# cmd的情况下 -l替换了 a 没有ls-l所有报错

# 正确做法
oot@mtl-unknown659404:/home/wb.duanxingcai/dockerfile# docker run mycentos-3 ls -al
total 0
drwxr-xr-x   1 root root   6 Nov 17 12:02 .
drwxr-xr-x   1 root root   6 Nov 17 12:02 ..
-rwxr-xr-x   1 root root   0 Nov 17 12:02 .dockerenv
lrwxrwxrwx   1 root root   7 Nov  3  2020 bin -> usr/bin
drwxr-xr-x   5 root root 340 Nov 17 12:02 dev
drwxr-xr-x   1 root root  66 Nov 17 12:02 etc
drwxr-xr-x   2 root root   6 Nov  3  2020 home
....
```

测试ENTRYPOINT

```shell
# 编写dockerfile dockerfile-cmd-entorypoint
root@mtl-unknown659404:/home/wb.duanxingcai/dockerfile# cat dockerfile-cmd-entorypoint
FROM centos
ENTRYPOINT ["ls","-a"]
# 构建镜像
root@mtl-unknown659404:/home/wb.duanxingcai/dockerfile# docker build -f dockerfile-cmd-entorypoint -t mycentos4 .
Sending build context to Docker daemon  18.94kB
Step 1/2 : FROM centos
 ---> 5d0da3dc9764
Step 2/2 : ENTRYPOINT ["ls","-a"]
 ---> Running in 038245cd6e42
Removing intermediate container 038245cd6e42
 ---> a70a4dc910db
Successfully built a70a4dc910db
Successfully tagged mycentos4:latest
# 运行容器
root@mtl-unknown659404:/home/wb.duanxingcai/dockerfile# docker run a70a4dc910db
srv
sys
tmp
usr
var
# 追加命令 是直接拼接的
root@mtl-unknown659404:/home/wb.duanxingcai/dockerfile# docker run a70a4dc910db -l
total 0
drwxr-xr-x   1 root root   6 Nov 17 12:05 .
drwxr-xr-x   1 root root   6 Nov 17 12:05 ..
-rwxr-xr-x   1 root root   0 Nov 17 12:05 .dockerenv
lrwxrwxrwx   1 root root   7 Nov  3  2020 bin -> usr/bin
drwxr-xr-x   5 root root 340 Nov 17 12:05 dev

```

> #### dockerfile中很多相同

### 做一个Tomcat镜像

1 准备镜像文件tomcat压缩包，jdk的压缩包

```shell
root@mtl-unknown659404:/home/wb.duanxingcai/dockerfile# vim Dockerfile
root@mtl-unknown659404:/home/wb.duanxingcai/dockerfile# cat Dockerfile
FROM centos
MAINTAINET kuansheng<12456798@q.com>
COPY readme.txt /usr/local/readme.txt
ADD jdkXXXX.tar.gz     /usr/local/              # jdk的压缩包
ADD apatchXxxxxx.tar.gz   /usr/local/			# tomacat的压缩包

RUN yum -y install vim

ENV MYPATH /usr/local
WORKDIR $MYPATH

ENV JAVA_HOME /usr/local/jdk1.8.0_11
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apaxx-tomcat
ENV CATALINA_BASH /usr/local/xxxx
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin

EXPOSE 8080

CMD /usr/local/apa-tomcaxxxx/bin/startup.sh && tail -F /url/local/apaxx/bin/logs/cataline.out
```

2 构建镜像

```shell
docker build -t xxx .
```

3 启动项目

```shell
docker run -itd -p xxx:8080 -v 目录挂载 --name xxx xxx
```



## Docker网络原理

## IDEA整合Docker

## Docker Compose

## Docker Swarm

## CI\CD Jenkins
