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
ARG					# 构建的时候传递的参数 --build-arf 参数名=xxx(文件)
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

### 发布自己的镜像

> Docker-Hub

1 地址https://hup.docker.com/ 注册自己的账号

2 确定这个账号可以登录

3 在我们的服务器提交镜像

```shell
# 先登录
docker login -u username -p password
# 登出
docker logout
# 在push 
docker push 镜像:[tag] 
# 给镜像加标签、
docker tag 镜像id name:[tag]
```

> 阿里云上

1 登录阿里云,找到容器镜像服务

2 创建命令空间

3 创建容器镜像

4 创建镜像仓库,仓库选择本地

5 使用操作指南

> 备份与恢复

```shell
# 备份
docker save 镜像 -o 路径
# 恢复 
docker load -o 路径
```



## Docker网络原理

### 理解Docker0网络

> 测试

![](.\img\POPO20211120-124846.png)



```shell
# 启动容器
root@mtl-unknown659404:/home/wb.duanxingcai# docker run -d -P --name tomcat01 tomcat
0c1f65e5cbd67291ba3913bfc374026a5afea50e8958c967e97e676cd5aa9942

# 查看容器网络地址
root@mtl-unknown659404:/home/wb.duanxingcai# docker exec -it tomcat01 ip addr

# 查看地址
docker inspect
root@mtl-unknown659404:/home/wb.duanxingcai# docker inspect tomcat01
....
"Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "938e53d6b996f6584828989948b394ecac751f6a2dd607e440f33a7e902b66eb",
                    "EndpointID": "5d247689505ab3512793f7feb618cc5ae2dad1aa8dd27a26e59c2b93e6a58a5b",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.11",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:0b",
                    "DriverOpts": null
                }


# linux是可以ping通docker容器地址
root@mtl-unknown659404:/home/wb.duanxingcai# ping 172.17.0.11
PING 172.17.0.11 (172.17.0.11) 56(84) bytes of data.
64 bytes from 172.17.0.11: icmp_seq=1 ttl=64 time=0.257 ms
64 bytes from 172.17.0.11: icmp_seq=2 ttl=64 time=0.077 ms
^C
--- 172.17.0.11 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 17ms
rtt min/avg/max/mdev = 0.077/0.167/0.257/0.090 ms

```

> 原理

1 我们每启动一个容器，docker就会给docker容器分配一个ip,我们只要安装docker，就会有一个网卡docker0桥接技术,使用的技术是evth-pair技术

```shell
root@mtl-unknown659404:/home/wb.duanxingcai# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1400 qdisc mq state UP group default qlen 1000
    link/ether 52:54:00:18:06:ca brd ff:ff:ff:ff:ff:ff
    inet 10.215.0.116/28 brd 10.215.0.127 scope global dynamic eth0
       valid_lft 63570sec preferred_lft 63570sec
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1400 qdisc noqueue state UP group default
    link/ether 02:42:12:c7:29:f1 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
523: br-84d63be51efa: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
    link/ether 02:42:a8:1e:3e:0b brd ff:ff:ff:ff:ff:ff
    inet 172.19.0.1/16 brd 172.19.255.255 scope global br-84d63be51efa
       valid_lft forever preferred_lft forever
# 以下是容器内部的网络相关配置
407: vethd9e3941@if406: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1400 qdisc noqueue master docker0 state UP group default
    link/ether 02:71:38:51:63:b2 brd ff:ff:ff:ff:ff:ff link-netnsid 4
409: veth0770aad@if408: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1400 qdisc noqueue master docker0 state UP group default
    link/ether 3e:91:be:be:e9:c5 brd ff:ff:ff:ff:ff:ff link-netnsid 5
413: veth985726c@if412: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1400 qdisc noqueue master docker0 state UP group default
    link/ether 4a:94:10:de:dd:58 brd ff:ff:ff:ff:ff:ff link-netnsid 7
419: veth6db24fe@if418: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1400 qdisc noqueue master docker0 state UP group default
    link/ether 96:86:b2:10:6a:8d brd ff:ff:ff:ff:ff:ff link-netnsid 6
713: veth8836e56@if712: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1400 qdisc noqueue master docker0 state UP group default
    link/ether 26:c1:e4:79:31:7e brd ff:ff:ff:ff:ff:ff link-netnsid 8
715: veth3ff1ede@if714: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1400 qdisc noqueue master docker0 state UP group default
    link/ether fa:e1:05:be:f8:7c brd ff:ff:ff:ff:ff:ff link-netnsid 2
717: veth63b68ac@if716: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1400 qdisc noqueue master docker0 state UP group default
    link/ether e6:40:62:a6:16:68 brd ff:ff:ff:ff:ff:ff link-netnsid 3
719: vethdbcf035@if718: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1400 qdisc noqueue master docker0 state UP group default
    link/ether 22:c3:e4:fe:3f:b8 brd ff:ff:ff:ff:ff:ff link-netnsid 0
721: vethe739e06@if720: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1400 qdisc noqueue master docker0 state UP group default
    link/ether a2:2a:a2:61:2d:53 brd ff:ff:ff:ff:ff:ff link-netnsid 1
725: veth8bd609d@if724: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1400 qdisc noqueue master docker0 state UP group default
    link/ether 7e:06:ea:ed:0b:e6 brd ff:ff:ff:ff:ff:ff link-netnsid 9
506: br-fc854ae4c3b8: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
    link/ether 02:42:60:8e:c1:dd brd ff:ff:ff:ff:ff:ff
    inet 172.18.0.1/16 brd 172.18.255.255 scope global br-fc854ae4c3b8
       valid_lft forever preferred_lft forever

```

```python
"""
我们发现这个容器带来的网卡是一对一对的
evth-pair 就是一对的虚拟设备接口，他们都是成对出现的，一段连着协议，一段彼此先来
正因为有这个特性，evth-pair 充当一个桥梁，连着各种虚拟网络设备
OpenStac Docker 容器之间的链接，Ovs之间的连接都是使用evth-pair技术
"""
```

```python
"""
两个容器之间的网络操作，地址相连接需要使用,docker内部的地址，获取移除服务器的防火墙也行
容器与容器之间相关通信的,两个容器之间有个中间的桥(docker0)，两个网络都接着桥(docker0)，然后再桥(docker0)上通
docker0就相当于路由器
"""
```

![](.\img\POPO20211120-131227.png)

> 小结 

Docker使用的是linux的桥接,docker 的所有接口都是虚拟的，因为转发效率高

![](.\img\POPO20211120-131719.png)

> #### 场景 

通过名字来访问服务

```shell
# 启动两个tomcat01和tomcat02
# 直接ping是不通的
docker exec -it timcat02 ping tomcat01 
# 需要加参数--link  不建议使用
docker run -d -P --name tomcat03 --link tomcat02 tomcat
# 此时在ping 就可以ping通了
docker exec -it timcat03 ping tomcat02 
# 反向是不能ping通的
docker exec -it timcat02 ping tomcat03

```

探究

```shell
# 查看容器内部配置
dockerexec -it tomcat03 cat /etc/hosts
# 这里面有绑定容器的操作
```

现在不建议使用--link了

自定义网络，不适用docker0

docker0不支持容器名链接访问

### 容器互联/自定义网络

> 查看所有网络

```shell
root@mtl-unknown659404:/home/wb.duanxingcai# docker network ls
NETWORK ID     NAME                    DRIVER    SCOPE
938e53d6b996   bridge                  bridge    local
fc854ae4c3b8   composetest_default     bridge    local
daa720d57435   host                    host      local
84d63be51efa   my_worldpress_default   bridge    local
e0d76bb82b39   none                    null      local

```

#### 网络模式

bridge:桥接,docker搭桥(默认|自己创建的也是)

none:不配置网络

host:和宿主机共享网络

container:容器内网络连通(用的少|局限很大)

#### 测试

```shell
# 我们直接启动的命令是有默认参数--net bright,也就是我们的docker0
docker run -d -P --name tomcat01 tomcat
# 等价于
docker run -d -P --name tomcat01 --net bridge tomcat
# 但是不适用与服务名的通信

# 自定义网络
# --driver bridge 桥接方式
# --subnet 192.168.0.0/16 ip数量65535
# --gateway 192.168.0.1 网关ip
root@mtl-unknown659404:/home/wb.duanxingcai# docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
b7dcda9ff9824886f1155a919b7b0130e430bc1564114bf3ac871985456f0f7d
root@mtl-unknown659404:/home/wb.duanxingcai# docker network ls
NETWORK ID     NAME                    DRIVER    SCOPE
938e53d6b996   bridge                  bridge    local
fc854ae4c3b8   composetest_default     bridge    local
daa720d57435   host                    host      local
84d63be51efa   my_worldpress_default   bridge    local
b7dcda9ff982   mynet                   bridge    local
e0d76bb82b39   none                    null      local

# 查看自己网络配置
root@mtl-unknown659404:/home/wb.duanxingcai# docker network inspect  mynet
[
    {
        "Name": "mynet",
        "Id": "b7dcda9ff9824886f1155a919b7b0130e430bc1564114bf3ac871985456f0f7d",
        "Created": "2021-11-20T14:36:16.038852899+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/16",
                    "Gateway": "192.168.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]

# 放入两个容器
root@mtl-unknown659404:/home/wb.duanxingcai# docker run -d -P --name tomcat-net-01 --net mynet tomcat
1db050f45ea62eb2f68ec04c3939ce615b039a24278227c6d493666806552faf
root@mtl-unknown659404:/home/wb.duanxingcai# docker run -d -P --name tomcat-net-02 --net mynet tomcat
0b2f3762a03490793f1ae3143190086c0370f76f2698ba974b5f78fa472677ff
root@mtl-unknown659404:/home/wb.duanxingcai# docker network inspect  mynet
[
    {
        "Name": "mynet",
        "Id": "b7dcda9ff9824886f1155a919b7b0130e430bc1564114bf3ac871985456f0f7d",
        "Created": "2021-11-20T14:36:16.038852899+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/16",
                    "Gateway": "192.168.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        # 可以看到有网络
        "Containers": {
            "0b2f3762a03490793f1ae3143190086c0370f76f2698ba974b5f78fa472677ff": {
                "Name": "tomcat-net-02",
                "EndpointID": "8283878473bfd056a93d56df1926189178fb90dea90772f62c29dd2c22a3f903",
                "MacAddress": "02:42:c0:a8:00:03",
                "IPv4Address": "192.168.0.3/16",
                "IPv6Address": ""
            },
            "1db050f45ea62eb2f68ec04c3939ce615b039a24278227c6d493666806552faf": {
                "Name": "tomcat-net-01",
                "EndpointID": "7de5191830f531e1f83d3ea6d3bb9f1221e5c0bb0d55b42428a2ce2de959b4e1",
                "MacAddress": "02:42:c0:a8:00:02",
                "IPv4Address": "192.168.0.2/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]

# 这时在去服务ping就好了
docker exec -it tomcat-net-01 ping tomcat-net-02
# 这样就可以ping通了
自定义的就可以使用啦
```

自定义已经帮助我们维护好了，推荐使用

#### 网络连通的操作

```shell
# 创建两个容器 在docker0
root@mtl-unknown659404:/home/wb.duanxingcai# docker run -d -P --name tomcat-01 tomcat
8028d035b5d96fa51a36b9c665cd058a8d3778ab81fbdef606ada5dc6e73b7ac
root@mtl-unknown659404:/home/wb.duanxingcai# docker run -d -P --name tomcat-02 tomcat
bcf92a0f6a7b1542c8c38ae4b662dffe92bf74fe2fb43c0bc00078b65d483fa8

# 刚刚在mynet也有两个容器
root@mtl-unknown659404:/home/wb.duanxingcai# docker run -d -P --name tomcat-net-01 --net mynet tomcat
1db050f45ea62eb2f68ec04c3939ce615b039a24278227c6d493666806552faf
root@mtl-unknown659404:/home/wb.duanxingcai# docker run -d -P --name tomcat-net-02 --net mynet tomcat
0b2f3762a03490793f1ae3143190086c0370f76f2698ba974b5f78fa472677ff

# 实现两个不同网段之间的连通
# tomcat-01   <-->tomcat-net-02
root@mtl-unknown659404:/home/wb.duanxingcai# docker network connect mynet tomcat-01
root@mtl-unknown659404:/home/wb.duanxingcai# docker network inspect mynet
[
    {
        "Name": "mynet",
        "Id": "b7dcda9ff9824886f1155a919b7b0130e430bc1564114bf3ac871985456f0f7d",
        "Created": "2021-11-20T14:36:16.038852899+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/16",
                    "Gateway": "192.168.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "0b2f3762a03490793f1ae3143190086c0370f76f2698ba974b5f78fa472677ff": {
                "Name": "tomcat-net-02",
                "EndpointID": "8283878473bfd056a93d56df1926189178fb90dea90772f62c29dd2c22a3f903",
                "MacAddress": "02:42:c0:a8:00:03",
                "IPv4Address": "192.168.0.3/16",
                "IPv6Address": ""
            },
            "1db050f45ea62eb2f68ec04c3939ce615b039a24278227c6d493666806552faf": {
                "Name": "tomcat-net-01",
                "EndpointID": "7de5191830f531e1f83d3ea6d3bb9f1221e5c0bb0d55b42428a2ce2de959b4e1",
                "MacAddress": "02:42:c0:a8:00:02",
                "IPv4Address": "192.168.0.2/16",
                "IPv6Address": ""
            },
            # 将刚刚的连的容器加入进来
            "8028d035b5d96fa51a36b9c665cd058a8d3778ab81fbdef606ada5dc6e73b7ac": {
                "Name": "tomcat-01",
                "EndpointID": "61facba6cf62e6b86515f11e1ae0bbb847e4b600fcf412548632ab56444463b7",
                "MacAddress": "02:42:c0:a8:00:04",
                "IPv4Address": "192.168.0.4/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
# tomcat-01 此时的容器有两个ip一个是在docker0下,一个是在mynet下的ip
# 类似阿里云的公网和私网Ip
# 此时 tomcat-01可以ping通mynet下的所有容器
```

### 实战--部署redis集群

![](\img\POPO20211120-145635.png)

创建6个redis

```shell
# 创建配置文件及卷
for port in $(seq 1 6); \
do \
mkdir -p /mydata/redis/node-${port}/conf
touch /mydata/redis/node-${port}/conf/redis.conf
cat  EOF /mydata/redis/node-${port}/conf/redis.conf
port 6379 
bind 0.0.0.0
cluster-enabled yes 
cluster-config-file nodes.conf
cluster-node-timeout 5000
cluster-announce-ip 172.38.0.1${port}
cluster-announce-port 6379
cluster-announce-bus-port 16379
appendonly yes
EOF
done

# 模板
docker run -p 637${port}:6379 -p 1637${port}:16379 --name redis-${port} \
    -v /mydata/redis/node-${port}/data:/data \
    -v /mydata/redis/node-${port}/conf/redis.conf:/etc/redis/redis.conf \
    -d --net redis --ip 172.38.0.1${port} redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf


# 启动容器 6 台
docker run -p 6371:6379 -p 16371:16379 --name redis-1 \
    -v /mydata/redis/node-1/data:/data \
    -v /mydata/redis/node-1/conf/redis.conf:/etc/redis/redis.conf \
    -d --net redis --ip 172.38.0.11 redis:5.0.9-alpine3.11 redis-server /etc/redis/redis.conf

# 进入容器内部的data下创建集群
# 创建集群
redis-cli --cluster create 172.38.0.11:6379 172.38.0.12:6379 172.38.0.13:6379 172.38.0.14:6379 172.38.0.15:6379 172.38.0.16:6379    --cluster-replicas 1
```



## IDEA整合Docker

#### Dockerfile

```yaml
FROM java:8
COPY *.jar /app.jar
CMD ["--server.port=8080"]
EXPOSE 8080
ENTRYPOINT ["java","-jar","/app.jar"]
```

## 小结

![](.\img\POPO20211123-190245.png)

## Docker Compose

地址:https://docs.docker.com/compose/

### 简介

docker官方的开源项目，实现对容器集群的快速编排，主要功能就是定义和运行多个docker容器的应用

原来的操作，自己手动创建容器，启动容器，重启服务等

docker 

dockerFile build --> run 手动操作，单个容器

Docker Compose 来轻松高效的管理容器，就是可以定义多个容器

> ### 官网介绍

定义，运行容器

YAML file 配置文件

简单的命令

Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration. To learn more about all the features of Compose, see [the list of features](https://docs.docker.com/compose/#features).

所有的环境都可以使用

Compose works in all environments: production, staging, development, testing, as well as CI workflows. You can learn more about each case in [Common Use Cases](https://docs.docker.com/compose/#common-use-cases).

Using Compose is basically a three-step process:

#### 三步骤

1. Define your app’s environment with a `Dockerfile` so it can be reproduced anywhere.
   1. Dockerfile保证我们的项目在任何地方都可以运行
2. Define the services that make up your app in `docker-compose.yml` so they can be run together in an isolated environment.
   1. services 服务
   2. docker-compose.yml 这个文件书写方式
3. Run `docker compose up` and the [Docker compose command](https://docs.docker.com/compose/cli-command/) starts and runs your entire app. You can alternatively run `docker-compose up` using the docker-compose binary.
   1. 启动docker-compose
   2. 作用批量容器编排

> ### 自己理解

Compose是Docker是官方的开源项目，需要安装

Dockerfile让程序在任何地方运行。web服务，redis,mysql.nginx等多个容器启停麻烦

Compose

```yml
version: "3.9"  # optional since v1.27.0
services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/code
      - logvolume01:/var/log
    links:
      - redis
  redis:
    image: redis
volumes:
  logvolume01: {}
```

docker-compose up 100个服务，可以启动文件中的所有服务

Compose:重要的两个概念

服务，services,其实就是一个一个的容器，应用

项目,一组关联的容器，mysql,web,nginx,redis等服务

### 安装

```shell
# 1. 下载可能下载慢
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
# 1.1 国内镜像 块些
sudo curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.5/docker-compose-`uname -s`-`uname -m` ->/usr/local/bin/docker-compos
# 2. 授权
sudo chmod +x /usr/local/bin/docker-compose
# 3. 设置软连接
 sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
# 4. 测试
root@mtl-unknown659404:/home/wb.duanxingcai/update_script# docker-compose --version
docker-compose version 1.29.2, build 5becea4c

```

### 命令模板

地址:https://vuepress.mirror.docker-practice.com/

服务(service):一个服务就是一个容器

项目(Project):有多个服务共同组成具有相同业务逻辑单元，在docker-compose.yml中定义

#### build

用来指定docker指定的目录, 将dockerfile打包成镜像，然后运行镜像,

```yml
version: "3.8"

services:

  demo01:
    build:  # 在启动服务时 先将build命令中指定dockerfile打包成镜像，在运行该镜像
      context: demo   # 指定上下文路径 其实就是dockerfile所在的目录 相对于docker-compose的位置,也可以是绝对路径
      dockerfile: Dockerfile  # 明确指定dockerfile的文件名字 如果是Dockerfile则可以不用写
    container_name: demo  # 镜像构建好 打包成容器的名字
    ports:
    - "8080:8080"
    networks:
      - hello
```



#### command

容器启动后默认执行的命令

```yml
command: echo "hello world" # cmd
```

#### container_name

指定容器名称,默认会使用`项目名称_服务名称_序号`

```yml
container_name: docker-web-container
```

使用容器名称后,该服务无法进行扩展(scale),Docker不允许多个容器具有相同的名称

#### deoends_on

解决容器启动的依赖，启动的先后问题,先启动redis,db再启动web

```yml
version: "3"

services:
 web:
  build: .
  depends_on:
   - db
   - redis
  
 redis:
  image: redis
 
 db:
  image: postgres
  
```



#### env_file

从文件中获取环境变量，可以单独的文件路径或者列表

如果通过docker-compose -f FILE方式来指定Compose模板文件，则env_file中变量的路径会基于模板文件路径

如果有环境变量名称与environment指令冲突，则按照惯例，以后者为准

```yml
env_file: .env

env_file:
 - ./common.env
 - ./apps/web.env
 - ./opt/secrets.env
```

环境变量每一行必须符合格式,支持#开头的注释

```yml
PROG_ENV=development
MYSQL_ROOT_PASSWORD=root
```



#### environment

设置环境变量。你可以使用数组或者字典两种格式

只给定名称的变量会自动获取运行Compose主机对应变量的值,可以用来防止泄露不必要的数据

```yml
environment:
 RACK_ENV: development
 SESSION_SECRET:
 
environment:
 - RACK_ENV=development
 - SESSION_SECRET
```

对于值是no,yes等一定要使用字符串，避免出现不必要的问题

#### healthcheck

通过命令检查容器是否运行健康 类似心跳检查

```yml
healthckeck:
 test: ["CMD"，"curl","-f","http://localhost"]
 interval: 1m30s
 timeout: 10s
 retries: 3
```



#### image

知道为镜像或者镜像ID，如果本地不存在，compose会去拉去这个镜像

```yaml
image: python3.6
image: eb40f396381c
```

#### networks

配置链接的网络

```yml
version: "3.8"

services:
  some-service:
  	networks:
  	 - some-network
  	 - other-network
  	 
networks:
 some-network:
 other-network:
   external: # 使用自定义桥名
     false  # true 确定使用自定义桥名 启动服务之前的手动创建桥名 不推荐使用 docker network create hello
```



#### ports

暴露端口信息 其实就是完成端口映射 ，端口可以写多个

宿主机端口:容器端口 或者仅仅知道容器端口，宿主机可以会随机选择

```yml
ports: 
 - "8000"
 - "6379:6379"
 - "0.0.0.0:8000:8000"
```

#### sysctls

配置容器内核参数

```yml
sysctls:
 net.core.somaxconn: 1024
 net.ipv4.tcp_syncookies: 0

sysctls:
 - net.core.somaxconn=1024
 - net.ipv4.tcp_syncookies=0
```



#### ulimits

指定容器ulimits的限制

指定最大进程数65535,文件句柄20000和40000

```yml
ulimits:
 nproc: 65536
 nofile:
  soft: 20000
  hard: 40000 
```



#### volumns

数据卷挂载路径，可以设置为 宿主机路径:容器内部路径，或者数据卷名称,并且可以设置访问模式

```yml
volumns:
 - /var/lib/mysql # 自动映射
 - cache:/tmp/cache
 - ~/configs:/etc/configs/:ro # 只读 绝对路径映射
```

如果路径为数据卷名称，必须在文件中配置数据卷

```yml
version: "3.8"

services:
  tomcat: # 服务名称
    image: tomcat:8.0-jre8 # 镜像名称
    ports: # 端口映射
    - "8080:8080"
    volumes:
    - mysql_data:/var/lib/mysql

volumes: # 指定数据卷 这是使用了卷名 
  mysql_data: # 只需要声明一下就好了 最终卷名是 项目名_mysql_data
      external: # 使用自定义卷名
      true  # 确定使用自定义卷名 启动服务之前的手动创建卷名 不推荐使用 docker volume create mysql_data
```

### 常用命令

地址:https://vuepress.mirror.docker-practice.com/

1 命令对象与格式

2 命令选项

3 命令使用说明

#### up

docker-compose up

构建 运行 启动服务

#### down

docker-compose down

停止 up命令启动的服务，并移除网络

#### exec

docker-compose exec (里面的服务名)

进入指定的容器

#### ps

docker-compose ps

列出所有的容器

#### restart

docker-compose restart

重启项目中你容器

#### rm

docker-compose rm 

移除项目中的容器

#### start

docker-compose start

启动项目中的容器

#### stop

docker-compose stop

停止项目中的容器

#### top

容器内部的信息

#### pause

暂停服务

#### unpause

恢复服务

#### logs

查看日志

### 体验

地址:https://docs.docker.com/compose/gettingstarted/

官网案例 python应用 ,计数器，redis

1 应用 app.py

创建文件夹

```shell
 mkdir composetest
 cd composetest
```

写服务

```python
import time

import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)

@app.route('/')
def hello():
    count = get_hit_count()
    return 'Hello World! I have been seen {} times.\n'.format(count)
```

依赖包

```python
flask
redis
```

2 Dockerfile应用打包为镜像

```dockerfile
# syntax=docker/dockerfile:1
FROM python:3.7-alpine
WORKDIR /code
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
EXPOSE 5000
COPY . .
CMD ["flask", "run"]
```



3 Docker-compose yaml文件（定义所有的服务，需要的环境,web,redis）完整的上线服务

```yml
version: "3.9"
services:
  web:
    build: .
    ports:
      - "5000:5000"
  redis:
    image: "redis:alpine"
```



4 启动compose项目（docker-compose up）

```shell
Successfully built 5d886456c571
Successfully tagged composetest_web:latest
WARNING: Image for service web was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating composetest_web_1   ... done
Creating composetest_redis_1 ... done
Attaching to composetest_web_1, composetest_redis_1
redis_1  | 1:C 23 Nov 2021 11:39:56.781 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
redis_1  | 1:C 23 Nov 2021 11:39:56.781 # Redis version=6.2.6, bits=64, commit=00000000, modified=0, pid=1, just started
redis_1  | 1:C 23 Nov 2021 11:39:56.781 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
redis_1  | 1:M 23 Nov 2021 11:39:56.782 * monotonic clock: POSIX clock_gettime
redis_1  | 1:M 23 Nov 2021 11:39:56.785 # Warning: Could not create server TCP listening socket ::*:6379: unable to bind socket, errno: 97
redis_1  | 1:M 23 Nov 2021 11:39:56.786 * Running mode=standalone, port=6379.
redis_1  | 1:M 23 Nov 2021 11:39:56.786 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
redis_1  | 1:M 23 Nov 2021 11:39:56.786 # Server initialized
redis_1  | 1:M 23 Nov 2021 11:39:56.786 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and            then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
redis_1  | 1:M 23 Nov 2021 11:39:56.787 * Ready to accept connections
web_1    |  * Serving Flask app 'app.py' (lazy loading)
web_1    |  * Environment: production
web_1    |    WARNING: This is a development server. Do not use it in a production deployment.
web_1    |    Use a production WSGI server instead.
web_1    |  * Debug mode: off
web_1    |  * Running on all addresses.
web_1    |    WARNING: This is a development server. Do not use it in a production deployment.
web_1    |  * Running on http://172.20.0.2:5000/ (Press CTRL+C to quit)
web_1    | 10.219.100.249 - - [23/Nov/2021 11:40:15] "GET / HTTP/1.1" 200 -
web_1    | 10.219.100.249 - - [23/Nov/2021 11:40:15] "GET /favicon.ico HTTP/1.1" 404 -
web_1    | 10.219.100.249 - - [23/Nov/2021 11:40:18] "GET / HTTP/1.1" 200 -

```



流程；

1 创建网络

2 执行Doceker-compose.yml

3 启动服务

Docker-compose up

Creating composetest_web_1   ... done
Creating composetest_redis_1 ... done

>  docker ps

```shell
root@mtl-unknown659404:/home/wb.duanxingcai# docker ps
CONTAINER ID   IMAGE                                      COMMAND                  CREATED         STATUS             PORTS                          NAMES
13c92a071991   composetest_web                            "flask run"              9 minutes ago   Up 9 minutes       0.0.0.0:5000->5000/tcp         composetest_web_1
b66b657bcbc8   redis:alpine                               "docker-entrypoint.s…"   9 minutes ago   Up 9 minutes       6379/tcp                       composetest_redis_1

```

> docker images

```shell
root@mtl-unknown659404:/home/wb.duanxingcai# docker images
REPOSITORY                    TAG                            IMAGE ID       CREATED          SIZE
composetest_web               latest                         5d886456c571   10 minutes ago   183MB
```

> docker service ls

```shell
root@mtl-unknown659404:/home/wb.duanxingcai# docker service ls  #表示不是一个
Error response from daemon: This node is not a swarm manager. Use "docker swarm init" or "docker swarm join" to connect this node to swarm and try again.
```

```python
默认的服务名 文件名_服务名_num
多个服务器,集群，A B _num 副本数量
```

集群状态.服务都不可能只有一个实例，弹性

网络规则

```shell
root@mtl-unknown659404:/home/wb.duanxingcai# docker network ls
NETWORK ID     NAME                  DRIVER    SCOPE
938e53d6b996   bridge                bridge    local
dc0e6b553ff8   composetest_default   bridge    local  # 自定义的网络 
daa720d57435   host                  host      local
e0d76bb82b39   none                  null      local
```

docker-compose里面启动的服务都在一个网络里面，那就可以通过服务名字来访问

```shell
root@mtl-unknown659404:/home/wb.duanxingcai# docker network inspect composetest_default
[
    {
        "Name": "composetest_default",
        "Id": "dc0e6b553ff84fd1a30fbd0e9e4032a79f945a0d36e1f7a5dbadb99d61302c78",
        "Created": "2021-11-23T19:39:33.355567375+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.20.0.0/16",
                    "Gateway": "172.20.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": true,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        # 该网络下的服务
        "Containers": {
            "13c92a0719916dc7085213a8b9fd4e0b3a92a71afe6d59d5072a5209ea4c4a7f": {
                "Name": "composetest_web_1",
                "EndpointID": "5f11cc7c7ac027db2fa3a874cb1e070c08c48b76bc5fa38c27b991ab91cd1293",
                "MacAddress": "02:42:ac:14:00:02",
                "IPv4Address": "172.20.0.2/16",
                "IPv6Address": ""
            },
            "b66b657bcbc8e2ff135e5bbd8f82f554818fe052c09d515bb6de3752fabf6a36": {
                "Name": "composetest_redis_1",
                "EndpointID": "1ab5f994ee078399b061ceef2f9fc89e0377248bf357a71acd263e6b7bc4ed3d",
                "MacAddress": "02:42:ac:14:00:03",
                "IPv4Address": "172.20.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {
            "com.docker.compose.network": "default",
            "com.docker.compose.project": "composetest",
            "com.docker.compose.version": "1.29.2"
        }
    }
]

```

在同一网络下，我们可以直接服务名访问

停止:docker-compose down或者ctrl+c

```shell
root@mtl-unknown659404:/home/wb.duanxingcai/composetest# docker-compose down
Stopping composetest_web_1   ... done
Stopping composetest_redis_1 ... done
Removing composetest_web_1   ... done
Removing composetest_redis_1 ... done
Removing network composetest_default
```

以前是单个docker run启动容器

docker-compose可以一键启动服务，一键停止服务

#### Docker小结

1 Docker镜像

2 DockerFile构建镜像

3 Docker-compose启动项目

4 Docker网络相关

### yaml规则

docker-compose.yaml核心

地址：https://docs.docker.com/compose/compose-file/

```shell
# 3层
version:"" # 版本
service: # 服务
	服务1: web
		# 服务配置
		build
		images
		network
		...
	服务2:redis
	服务3:mysql
# 其他配置 网络/卷
volumns:
networks:
configs:
		
```

#### demo

```yaml
version: "3.9"
services:

  redis:
    image: redis:alpine
    ports:
      - "6379"
    networks:
      - frontend
    deploy:
      replicas: 2
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure

  db:
    image: postgres:9.4
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - backend
    deploy:
      placement:
        max_replicas_per_node: 1
        constraints:
          - "node.role==manager"

  vote:
    image: dockersamples/examplevotingapp_vote:before
    ports:
      - "5000:80"
    networks:
      - frontend
    depends_on:
      - redis
    deploy:
      replicas: 2
      update_config:
        parallelism: 2
      restart_policy:
        condition: on-failure

  result:
    image: dockersamples/examplevotingapp_result:before
    ports:
      - "5001:80"
    networks:
      - backend
    depends_on:
      - db
    deploy:
      replicas: 1
      update_config:
        parallelism: 2
        delay: 10s
      restart_policy:
        condition: on-failure

  worker:
    image: dockersamples/examplevotingapp_worker
    networks:
      - frontend
      - backend
    deploy:
      mode: replicated
      replicas: 1
      labels: [APP=VOTING]
      restart_policy:
        condition: on-failure
        delay: 10s
        max_attempts: 3
        window: 120s
      placement:
        constraints:
          - "node.role==manager"

  visualizer:
    image: dockersamples/visualizer:stable
    ports:
      - "8080:8080"
    stop_grace_period: 1m30s
    volumes:
      - "/var/run/docker.sock:/var/run/docker.sock"
    deploy:
      placement:
        constraints:
          - "node.role==manager"

networks:
  frontend:
  backend:

volumes:
  db-data:
```

多写，多看yaml文件

### 开源项目

#### 博客

下载博客，配置数据库相关，配置......

compose直接部署

地址：https://docs.docker.com/samples/wordpress/

1 下载项目

```shell
mkdir my_wordpress/
cd my_wordpress/
```

2 准备DockerFile文件(镜像可以不使用)

3 编写docker-compose.yml

```shell
version: "3.9"
    
services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    volumes:
      - wordpress_data:/var/www/html
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
volumes:
  db_data: {}
  wordpress_data: {}
```



4 直接启动

```shell
docker-compose up 
docker-compose up -d 后台启动
docker-compose dowm 停止服务
docker-compose build 重新构建
```

### 实战

1 编写服务

2 dockerfile构建镜像

3 docker-compose.yml编排项目

4 丢到服务器 docker-compose up

小结：

未来项目只要有docker-compose文件，按照这个规则，启动编排容器,有则可以直接启动

#### 启动一个后端服务的docker-compose.yml

```yml
version: "3.9"  # 版本

services: # 服务

  service:  # 后端服务
    build:  # 使用Dockerfile创建
      context: . # dockerfile文件所在的路径
      dockerfile: Dockerfile-service # dockerfile文件的名字
      target: service_demo # 镜像名字
    container_name: mtl_public_service_1 # 镜像启动后的容器名字
    ports: # 端口映射
      - "1129:8006"
    volumes: # 容器卷
      - /home/duanxingcai/mtl_public/:/www/
    networks: # 链接的网络
      - mtl_public
    depends_on: # 控制服务启动的孙旭
      - redis
      - mongo
      - minio

  client:   # 前端服务
    build:  # 使用dockerfile构建镜像
      context: ..  # dockerfile的文件位置
      dockerfile: Dockerfile-client # dockerfile的文件名字
      target: client_demo # 镜像名字
    container_name: mtl_public_client_1 # 启动后的容器名字
    ports: # 端口映射
      - "1130:86"
    volumes: # 数据卷挂载
      - /home/duanxingcai/dist:/usr/share/nginx/csm/
    networks: # 网络配置
      - mtl_public

  redis: # redis服务
    container_name: mtl_redis # 容器名字
    image: library/redis:latest # 使用的镜像版本
    restart: always # 总是重启
    command: redis-server /etc/redis.conf # 启动容器
    ports: # 端口映射
      - "6380:6379"
    volumes: # 数据卷挂载
      - /home/duanxingcai/redis_data:/data
      - /home/duanxingcai/redis_data/redis.conf:/etc/redis.conf
    networks: # 网络配置
      - mtl_public

  mongo: # 服务名字
    image: mongo:latest # 镜像名字
    restart: always # 总是重启
    container_name: mtl_mongo # 容器名字
    volumes: # 数据卷挂载
      - /home/duanxingcai/mongo_data/db:/data/db
      - /home/duanxingcai/mongo_data/log:/var/log/mongodb
    ports: # 端口映射
      - "27018:27017"
    networks: # 网络配置
      - mtl_public
#    environment: # 环境配置
#      MONGO_INITDB_ROOT_USERNAME: admin
#      MONGO_INITDB_ROOT_PASSWORD: admin


  minio: # 服务名字
    container_name: mtl_minio # 容器名字
    image: minio/minio:RELEASE.2021-06-17T00-10-46Z # 镜像版本
    command: server /data # 启动容器
    restart: always # 总是重启
    ports: # 端口映射
      - "9001:9000"
    volumes: # 数据卷挂载
      - /home/duanxingcai/minio_data:/data
      - /home/duanxingcai/minio_data/config:/root/.minio
    networks: # 网络配置
      - mtl_public
#    environment: # 环境配置
#      MINIO_ACCESS_KEY: "username"
#      MINIO_SECRET_KEY: "password"

networks: # 定义网络
  mtl_public:

```

#### 目录层级

```shell
root@mtl-unknown659404:/home/duanxingcai# ls
Dockerfile-client  conf.d  dist  minio_data  mongo_data  mtl_public  redis_data  update_script
root@mtl-unknown659404:/home/duanxingcai# cd mtl_public/
root@mtl-unknown659404:/home/duanxingcai/mtl_public# ls
Dockerfile-service  common  docker-compose.yml  manage.py  mtl_public  pid.wsgi  project_manage  requirements.txt  static  user  utils  uwsgi.ini  uwsgi.log
```



### 总结

#### 工程、服务、容器

工程就是一个项目

服务就是后端，前端，redis,celery等

容器是一个一个服务

## Docker可视化工具

#### 1 安装Portainer

```shell
# 拉取
docker pull portainer/portainer
# 启动
docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer
```



## Docker Swarm

集群的方式部署，单机是自己的操作4台阿里云服务器, 2 4g

> ### 第一步买4台服务器 2核4G

创建实例

![](.\img\POPO20211129-145056.png)

选择付费模式和地区

![](.\img\POPO20211129-145311.png)

购买类型大小及数量

![](.\img\POPO20211129-145450.png)

选择镜像及版本

![](.\img\POPO20211129-145633.png)

配置安全组

![](.\img\POPO20211129-145802.png)

配置自定义密码

![](.\img\POPO20211129-145918.png)

分组不用动直接下一步再到确认订单就好了

![](.\img\POPO20211129-150059.png)

### 4台机器安装docker

```python
# xshell有个发送键到所有会话
查看安装方案
```

### 概述

地址：https://docs.docker.com/engine/swarm/

#### 工作模式

Docker Engine 1.12 introduces swarm mode that enables you to create a cluster of one or more Docker Engines called a swarm. A swarm consists of one or more nodes: physical or virtual machines running Docker Engine 1.12 or later in swarm mode.

There are two types of nodes: [**managers**](https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/#manager-nodes) and [**workers**](https://docs.docker.com/engine/swarm/how-swarm-mode-works/nodes/#worker-nodes).

有管理节点和工作节点

操作都在manager里面

Raft一致性算法

![](.\img\POPO20211129-151313.png)

### 集群搭建

```shell
# 搭建之后会多一个网络
root@mtl-unknown659404:/home/wb.duanxingcai# docker network ls
NETWORK ID     NAME                    DRIVER    SCOPE
938e53d6b996   bridge                  bridge    local
daa720d57435   host                    host      local
e0d76bb82b39   none                    null      local
```

基本操作

![](.\img\POPO20211129-151814.png)

集群初始化,就是告诉别人我在哪,包括私网和公网

![](.\img\POPO20211129-151938.png)

私网地址

![](.\img\POPO20211129-152116.png)

```shell
# 初始化一个集群
docker swarm init --advertise-addr 172.24.82.179
# 得到命令 join
docker swarm join
# 获取令牌 一个是manage/worker
# 加入一个从节点
docker swarm join --token SWARM-XXXXX
# 生成一个命令
docker swarm join worke
docker swarm join manager
# 查看节点
docker node ls
```

![](.\img\POPO20211129-152349.png)

加入一个节点

![](.\img\POPO20211129-152618.png)

共加入了3节点，3主一从

### Raft一致性算法

双主双从，其他节点能不能用，

Raft就是保证大多数节点>1才可以使用，所以至少>3

实验:

1 将一个主节点的docker宕机，那么双主双从的情况下取法选举出另一个主节点，所以无法使用

```shell
docker swarm leave # 离开节点
```

![](.\img\POPO20211129-154125.png)

2 如果是三台主机，挂了一台，还有2台可以选举出一台领导主机，还可以继续使用

### 体会

弹性，负载，扩缩容

告别docker run

docker-compose 启动项目

> ### 集群:swarm docker service

k8s service pods

集群:swarm docker serivce

容器=>服务

容器=>服务=>副本

redis服务=>10个副本

![](.\img\POPO20211129-155858.png)

灰度发布:金丝雀发布!

----

```shell
# 创建一个服务
docker service create -p 8888:80 --name my-nginx nginx
# 区别 docker run是一个容器启动 没有扩缩容功能
docker run 
# 一个服务启动 具有扩缩容功能
docker service 
```

![](.\img\POPO20211129-160155.png)



查看基本信息,副本信息 REOLICAS:副本信息

![](.\img\POPO20211129-160357.png)

创建三个副本/也就是动态扩缩容

![](.\img\POPO20211129-160721.png)



服务在任意的节点都可以访问，服务可以在动态的扩展，伸缩

服务的高可用，任何企业，云

```shell
# 另一个扩缩容命令 扩缩容5份
docker service scale xxx=5
# 移除
docker service rm xxx
```

### 概念总结

#### swarm

集群的管理和编排,docker可以初始化一个swarm集群，其他节点可以加入(管理，工作者)

> 常用命令

```shell
docker swarm init				初始化集群
docker swarm join-token worker	查看工作节点的 token
docker swarm join-token manager	查看管理节点的 token
docker swarm join				加入集群
```



#### Node

其实就是一个docker节点，多个节点就组成了一个网络

> 常用命令

```shell
# 查看集群所有节点
docker node ls
# 查看当前节点所有任务
docker node ps	
# 删除节点（-f强制删除）
docker node rm 节点名称|节点ID	
# 查看节点详情
docker node inspect 节点名称|节点ID	
# 节点降级，由管理节点降级为工作节点
docker node demote 节点名称|节点ID
# 节点升级，由工作节点升级为管理节点
docker node promote 节点名称|节点ID
# 更新节点
docker node update 节点名称|节点ID	
```



#### service

任务，可以在管理节点和工作节点来运行，核心,用户访问就是他

> 常用命令

```shell
# 创建服务 后面与普通创建是一样的没有-c参数
docker service create	
# 查看所有服务
docker service ls	
# 查看服务详情
docker service inspect 服务名称|服务ID	
# 查看服务日志
docker service logs 服务名称|服务ID	
# 删除服务（-f强制删除）
docker service rm 服务名称|服务ID	
# 设置服务数量
docker service scale 服务名称|服务ID=n	
# 更新服务
docker service update 服务名称|服务ID	
```



#### Task

容器内的命令，细节任务

![](.\img\POPO20211129-162058.png)

#### 内部原理

![](.\img\POPO20211129-162154.png)

命令->manager->api->调度->工作节点(创建容器,自动创建)

> ### 服务副本与全局服务

![](.\img\POPO20211129-162600.png)

调整service以什么方式运行

```shell
--mode string
Service mode(replicated or global)(default "replicated")
# 指定副本下
docker service create --mode replicated --name mytom tomcat:7
# 指定全局下
docker service create --mode global --name haha alpine ping baidu.com

```

拓展:网络模式:"PublicMode":"ingress"

Swarm:

Overlay:

ingress:特殊的Overlay网络，负载均衡功能

虽然docker在4台机器上，单实际是同一个,相互绑定的ip

![](.\img\POPO20211129-163358.png)

## Docker Stack

docker-compose 单机部署项目

Docker Stack部署，集群部署

```shell
# 单机
docker-compose up -d xxx.yml
# 集群
docker stack deploy xxx.yml


root@mtl-unknown659404:/home/wb.duanxingcai# docker stack --help

Usage:  docker stack [OPTIONS] COMMAND

Manage Docker stacks

Options:
      --orchestrator string   Orchestrator to use (swarm|kubernetes|all)

Commands:
  deploy      Deploy a new stack or update an existing stack  # 后面接一个文件
  ls          List stacks
  ps          List the tasks in the stack
  rm          Remove one or more stacks
  services    List the services in the stack

Run 'docker stack COMMAND --help' for more information on a command.
```

### 案例

```yml
version: "3.9"

services:

  service:
    image: mtl_demo_service:latest
    volumes:
    - /home/wb.duanxingcai/mtl_public:/www/
    networks:
      - mtl_public
    ports:
    - "1129:8006"
    deploy:
      replicas: 1
      # 将挂载的目录放在管理节点上
      placement:
        constraints:
          - node.role == manager

  client:
    image: mtl_demo_client:latest
    volumes:
    - /home/wb.duanxingcai/dist/:/usr/share/nginx/csm/
    networks:
      - mtl_public
    ports:
    - "1130:86"
    deploy:
      replicas: 1
      placement:
        constraints:
          - node.role == manager
networks:
  mtl_public:
```

### 注意事项

```python
# 使用docker service scale xxx=2
出现异常 "invalid mount config for type…"
# 每台服务器都必须有该镜像，不然只能在有这个镜像的地方创建服务
# 数据卷挂载也是，当有数据卷挂载时需要提前创建要挂载的目录
# 镜像不能想docker-compose一样有build命令 需要想有镜像才行
```

### 常用命令

启动项目

```shell
docker stack -c deploy docker-compose.yml name
```

查看服务

```shell
docker service ps 服务名
```

删除服务

```shell
docker service rm 服务名
```

更新镜像

```shell
docker service update 服务名

# 更新服務
docker service update --image 新镜像 正在启动的镜像
# 更新端口
docker service update --publish-rm 8080:5000 --publish-add 8088:5000 web
docker service ps web
docker service  web
```

扩缩容

```shell
# 两个副本
docker service scale 服务名=2
```

回退

```shell
docker service rollback 镜像名字
```





## Docker Secret

安全，证书相关

```shell
root@mtl-unknown659404:/home/wb.duanxingcai# docker secret --help

Usage:  docker secret COMMAND

Manage Docker secrets

Commands:
  create      Create a secret from a file or STDIN as content
  inspect     Display detailed information on one or more secrets
  ls          List secrets
  rm          Remove one or more secrets

Run 'docker secret COMMAND --help' for more information on a command.

```

## Docker Config

docker 配置相关

```shell
root@mtl-unknown659404:/home/wb.duanxingcai# docker config --help

Usage:  docker config COMMAND

Manage Docker configs

Commands:
  create      Create a config from a file or STDIN
  inspect     Display detailed information on one or more configs
  ls          List configs
  rm          Remove one or more configs

Run 'docker config COMMAND --help' for more information on a command.

```

Go语音，必须了解，java

## CI\CD Jenkins

![img](chrome://fileicon/?path=C%3A%5CUsers%5Cwb.duanxingcai%5CDownloads%5C2b5672e7POPO20211120-131719.png&scale=1x)

[2b5672e7POPO20211120-131719.png](http://mtl.minio.service.netease.com/minio/download/public/2b5672e7POPO20211120-131719.png?token=eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJhY2Nlc3NLZXkiOiJtaW5pb2FkbWluIiwiZXhwIjoxNjQxMjgzNzI4LCJzdWIiOiJtaW5pb2FkbWluIn0.Dsm60RH-1slLNFiHBMe8p9m29u6RCPh6G6ASQtFwecnkMpn_Gl7IIzemghBiBNDeFSyM3EROof3A9bVv5vL4wg)

[http://mtl.minio.service.netease.com/minio/download/public/2b5672e7POPO20211120-131719.png?token=eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJhY2Nlc3NLZXkiOiJtaW5pb2FkbWluIiwiZXhwIjoxNjQxMjgzNzI4LCJzdWIiOiJtaW5pb2FkbWluIn0.Dsm60RH-1slLNFiHBMe8p9m29u6RCPh6G6ASQtFwecnkMpn_Gl7IIzemghBiBNDeFSyM3EROof3A9bVv5v](http://mtl.minio.service.netease.com/minio/download/public/2b5672e7POPO20211120-131719.png?token=eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJhY2Nlc3NLZXkiOiJtaW5pb2FkbWluIiwiZXhwIjoxNjQxMjgzNzI4LCJzdWIiOiJtaW5pb2FkbWluIn0.Dsm60RH-1slLNFiHBMe8p9m29u6RCPh6G6ASQtFwecnkMpn_Gl7IIzemghBiBNDeFSyM3EROof3A9bVv5vL4wg)

在文件夹中显示

