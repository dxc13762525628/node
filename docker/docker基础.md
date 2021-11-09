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

## Docker镜像

## 容器数据卷

## DockerFile

## Docker网络原理

## IDEA整合Docker

## Docker Compose

## Docker Swarm

## CI\CD Jenkins
