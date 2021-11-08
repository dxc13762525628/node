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

```python
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

```python
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



## Docker基本命令

## Docker镜像

## 容器数据卷

## DockerFile

## Docker网络原理

## IDEA整合Docker

## Docker Compose

## Docker Swarm

## CI\CD Jenkins
