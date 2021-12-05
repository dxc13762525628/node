# k8s

## 核心概念

### Pod

最小部署单元

是一组容器的集合

一个Pod中的容器是共享网络的

生命周期是短暂的

### Controller

确保预期的Pod副本数量

> #### 无状态应用部署

可以随便用

> #### 有状态应用部署

网络ip唯一

确定所有的node运行同一个pod

一次性任务和定时任务

### Service

定义一组pod的访问规则

## 搭建环境

### 平台规划

>  单master集群

![](.\img\POPO20211201-123107.png)

> 多master集群

![](.\img\POPO20211201-123222.png)

### 配置要求

> 测试环境

master 

节点至少2核 内存至少4G 硬盘至少20G

node

4核 8G 40G

> 生产环境

master 

节点至少4核 内存至少8G 硬盘至少40G

node

8核 16G 80G

### 集群部署方式

> kubeadm 方式

快速部署集群的方式

1 创建Master节点 kubeadm init

2 将Node节点 加入到集群当中去 $ kubeadm join <master 节点的ip和端口>

> 环境准备

 所有机器可以互通

可以访问外网

禁止swp分区

> ## 主要流程

```shell
# 1. 准备3台机器 centos7
# 2. 对安装之后的系统进行初始化
# 3. 临时关闭防火墙
systemctl stop firewalld
# 3.1 永久关闭
systemctl disable firewalld

# 4 临时关闭
setenforce 0
# 4.1 永久关闭
sed -i 's/enforcing/disabled/' /etc/selinux/config

# 5 临时关闭swap
swapoff -a
# 5.1 永久关闭
sed -ri 's/.*swap.*/#&/' /etc/fstab

#根据规划设置主机名
hostnamectl set-hostname <hostname>

# 在master添加hosts
cat >> /etc/hosts << EOF
192.168.44.140 master
192.168.44.141 node1
192.168.44.142 node2
EOF

# 将桥接的ipv4流量传递到iptables的链
cat > /etc/sysctl.d/k8s.conf << EOF
net.bridge.bridge-nf-call-ip6talbes =1
net.bridge.bridge-nf-call-iptalbes =1
EOF
sysctl --system # 使其生效

# 时间同步
yum install ntpdate -y
ntpdate time.windows.com
```

> ### 安装docker/kubernets/kubeadm 

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

添加镜像

```shell
登录阿里云-->找到镜像加速器-->配置使用

```

添加阿里云的YUM源

```shell
$ cat > /etc/yum.repos.d/kubernetes.repo << EOF
[kubernetes]
name = kubernetes
xxxxx# 具体看阿里云
EOF
```

> 安装Kubernetes和kubelet和kubec

```shell
yum install -y kubelet-1.18.0 kubeadm-1.18.0 kubectl-1.18.0
systemctl enable kubelet
```

> ### 部署Kubernetes Master

在一台(192.168.44.141)主机执行

```shell
$ kubeadm init \
--apiserver-advertise-address=192.168.44.141\
--image-repository registry.aliyuncs.com/google_containers\
--kubernetes-version v1.18.0\
--service-cidr=10.96.0.0/12\    # 两个ip与当前不冲突就好了
--pod-network-cidr=10.244.0.0/16
```

由于默认拉取镜像地址k8s.gcr.io国内无法访问，这是使用阿里云地址

使用kubectl工具

```shell
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
$ kubectl get nodes
```

> #### 加入kubernetes Node

在192.168.44.142执行

向集群节点，执行kubeadm init输出的kubeadm join命令

```shell
# 创建token
kubeadm token create --print-join-command
```

> ### 部署CNI网络插件

```she
wget https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kubeflannel.yml

```

默认镜像无法访问，sed命令修改为docker hub镜像仓库

```shell
wget https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kubeflannel.yml

kubectl get pods -n kube-system
```

> ### 测试集群

创建一个pod，验证是否正常运行

```shell
$ kubectl create deployment nginx --image=nginx
$ kubectl expose deployment nginx --port=80 --type=NodePort
$ kubectl get pod.svc
```

访问地址:http://NodeIP:port



> ## 二进制包方式



> ### 操作系统初始化

```shell
# 1. 准备3台机器 centos7
# 2. 对安装之后的系统进行初始化
# 3. 临时关闭防火墙
systemctl stop firewalld
# 3.1 永久关闭
systemctl disable firewalld

# 4 临时关闭
setenforce 0
# 4.1 永久关闭
sed -i 's/enforcing/disabled/' /etc/selinux/config

# 5 临时关闭swap
swapoff -a
# 5.1 永久关闭
sed -ri 's/.*swap.*/#&/' /etc/fstab

#根据规划设置主机名
hostnamectl set-hostname <hostname>

# 在master添加hosts
cat >> /etc/hosts << EOF
192.168.44.140 master
192.168.44.141 node1
192.168.44.142 node2
EOF

# 将桥接的ipv4流量传递到iptables的链
cat > /etc/sysctl.d/k8s.conf << EOF
net.bridge.bridge-nf-call-ip6talbes =1
net.bridge.bridge-nf-call-iptalbes =1
EOF
sysctl --system # 使其生效

# 时间同步
yum install ntpdate -y
ntpdate time.windows.com
```

> ### 为etcd和apiserver自签证书(cfssl)



> ### 部署etcd集群



> ### 部署master组件



> ### 部署Node节点



> ### 部署集群的网络插件



## Kubernetes集群命令行工具kubectl

### kubectl

> 语法格式

```shell
kubectl [command ] [type] [name] [flags]

command: create,get,describe delete
type: 指定资源类，大小写敏感
name:资源的名字
flages:可选参数
# eg
$ kubectl get node
$ kubectl --help
```

![](.\img\POPO20211203-163248.png)

### YAML文件

对资源对象的编排和部署

通过缩进表示层级关系

不能使用tab进行缩进，只能使用空格

一般开头是2个空格

字符后缩进一个空格，比如冒号，逗号

---表示一个新文件的开始

注释是#表示

一般两部分组成

![](.\img\POPO20211203-170102.png)

> 常见的语法

![](.\img\POPO20211203-170408.png)

> 编写方式

1 使用kubectl create 命令生效yaml文件

```shell
# 不真正执行 只是输出打印一个yaml文件
kubectl create deployment web --image=nginx -o yaml --dry-run > my.yaml
```



2 使用kubectl get导出yaml文件

```shell
# 导出正在运行的服务
kubectl get deploy nginx -o=yaml --export > my.yaml
```

## Pod

### 概述

pod是k8s中可以创建和管理的最小单元，不是容器

k8s不会直接处理容器而是pod,一个pod是一组容器的集合

一个pod中的容器是共享一个网络命名空间的

pod是一个短暂的

存在的意义

1 创建容器使用docker,一个docker对应一个容器,一个容器是一个单进程

2 pod是多进程设计，运行多个应用程序，一个pod有多个容器，一个容器里运行一个应用程序

3 pod存在也是为亲密性应用，多个容器之间进行交互，容器网络之间的调用，两个应用之间的频繁用

### 实现机制

##### 共享网络

```shell
# 同一个pod中的容器之间只在一个ip网段
# 通过一个pause容器，把其他业务容器加入到pause容器里面，让所有业务容器在同一个名称空间中，可以实现网络共享
```



##### 共享存储

```shell
# 其实就是数据卷 进行数据持久化
```

![](.\img\POPO20211204-124138.png) 

### Pod拉取策略

> ### 三种策略



![](.\img\POPO20211204-124627.png)

### Pod资源限制

> ### 节点资源最大限制

本质还是docker做到的

![](.\img\POPO20211204-125035.png)

### Pod重启机制

> #### 三大策略

![](.\img\POPO20211204-125442.png)

### Pod的健康检查

> #### 两种检查

![](.\img\POPO20211204-133111.png)

### Pod的基本创建流程

![](.\img\POPO20211204-134049.png)

### Pod调度

影响调度的两个因素，资源的限制和节点选择器

#### pod的资源限制

#### pod的节点选择器标签

```shell
# 创建标签
kubectl label node node1 env_role=dev

```

![](.\img\POPO20211204-140505.png)

#### 节点亲和性

```shell
# nodeAffinity和之间nodeSelector基本一样
1. 硬亲和性
	# 约束条件必须满足,必须是dev和test才行
	
2. 软亲和性 
    # 尝试满足，不保证一定满足
    
# 支持常见的操作符
In NotIn Exists Gt Lt DoesNotExists
```

![](.\img\POPO20211204-141254.png)

#### 污点，污点容忍

> ###  污点

节点不做普通分配调度,是节点属性,是节点属性

##### 场景

专用节点

配置特点硬件节点

基于Taint驱逐

```shell
# 查看污点情况
kubectl describe node k8smaster | grep Taint
# 有三个值
# Noschedule： 一定不被调度
# PreferNoSchdule ： 尽量不被调度
# NoExecute: 不会被调度，并且还会驱逐Node已有Pod

# 添加污点
kubectl taint node [node] key=value(污点的三个值)

# 删除污点(-表示就是删除的意思)
kubectl taint node key=value- 
```

> ### 污点的容忍

yaml添加篇日志

![](.\img\POPO20211204-143108.png)

## Controller

### 概述

管理和运行容器的对象

### Pod与Controller关系

pod是通过controller来实现应用的运维，比如伸缩，滚动升级

Pod是通过label标签来和controller来建立关系的

![](.\img\POPO20211204-151033.png)

### Deployment控制器应用场景

部署无状态的应用

管理pod和ReplicaSet

部署，滚动升级的功能

web服务，微服务中

### yaml文件字段说明



### Deployment部署应用

```shell
# 先导出
kubectl create deployment web --image=nginx --dry-run -o > web.yaml
# 使用yaml部署
kubectl apply -f web.yaml
# 对外发布(暴露端口)
kubectl expose deployment web --port=80 --type=NodePort --target-port=80 --name=web1 - o yaml > web1.yaml
# 使用yaml部署
kubectl apply -f web1.yaml

```



### 升级回滚/弹性伸缩

```shell
# 升级 升级镜像就好了 就是将镜像换成1.15的
kubectl set image deployment web nginx=nginx:1.15 

# 查看历史版本
kubectl rollout history deploment web
# 还原 
# 回滚到上一个版本
kebectl rollout undo deploment web
# 回滚到指定版本
kebectl rollout undo deploment web --to-revision=[版本号]
# 弹性伸缩
kebectl scale deployment web --replicas=10[副本数]
```

> 升级过程

```shell
# 先下载1.14的镜像 在将1.15依次替换1.14的镜像
# 升级过程中服务是不会停的
```

### 定时任务

#### 一次任务job

```shell
# yaml文件
apiVersion: batch/v1
kind: Job # 关键
metadata: 
```



#### 定时任务cornjob

```yaml
# yaml
apiVersion: batch/v1
kind: Job # 关键
metadata: 
spec:
 schedule: "*/1 * * * *"
```



## Service

定义一组Pod访问规则

> ### 意义

为了防止pod失联(服务发现)

![](.\img\POPO20211204-151721.png)

负载均衡，定义一组pod访问的访问策略

![](.\img\POPO20211204-151855.png)

> ### Pod与Service之间的关系

通过标签(label/selector)建立关系

![](.\img\POPO20211204-152123.png)

### 常见service类型

ClusterIP:一般用于集群内部使用(默认使用)

NodePort:一般用于对外访问应用的时候使用

LoadBalancer:对外访问应用使用，公有云的调用

> ### 无状态

认为Pod都是一样的

没有顺序要求

不用考虑在哪个node运行

随机进行伸缩和扩展

> ### 有状态

无状态的因素都有相关考虑

让每个Pod都是独立的,保持Pod的启动顺序和唯一性

唯一的网络标识符，持久进行区分

有序，比如mysql的主从

> ### 部署有状态应用

无头service

ClusrerIp: none

> ### 守护进程DaemonSet

先执行yaml文件

```shell
# 进入pod中
kubectl exec -it pod名 bash

```

## Secret

加密数据存在etcd里面，让Pod容器已数据卷方式进行访问，加密方式base64

以变量形式进行存储

```yaml
# yaml
apiVersion: batch/v1
kind: Secret # 关键
metadata: 
 name: mysercret
type: Opaque

```

## ConfigMap

存储不加密的数据到etcd中，让Pod以变量或者数据卷挂载到容器中

主要用于配置文件

```yaml
1. 创建配置文件
vim demo.json
2. 创建configmap
kubectl create configmap demo-config --from-file=demo
3. 挂载到pod中
apiVersion: batch/v1
kind: Pod # 关键
metadata: 
 name: mypod
spec: 
	xxxx
volumes:
 - name: xxxcoumn
 configMap:
 	name: demo-config 
```

## k8s集群的安全机制

### 概述

当我们访问k8s集群时，先经历三个操作,认证->鉴权->准入控制,当访问时，都会经过apiserver,apiserver会做统一协调,访问过程需要授权等

> ###  认证

传输安全，对外不暴露8080,只能内部访问，外部访问6443

客户端常用认证方式

1 https证书认证，CA证书

2 http token认证，通过token识别用户

3 http 基本认证，用户名+密码

> ### 鉴权(授权)

基于RBAC的方式进行鉴权，基于角色的访问控制

RBAC基于角色的访问控制 就是用户绑定一个角色，那这个人就拥有访问这个角色的权限

```shell
# 角色
role 				# 特定命名空间访问权限
ClusterRole			# 所有命名空间访问权限

# 角色绑定
roleBinding:		# 角色绑定主体
ClusterRoleBunding: # 集群角色绑定到主体

# 主体
user				# 用户
group				# 用户组
serviceAccount		# 服务账号

```

基本操作

```shell
# 创建命名空间
kubectl create ns roledemo
# 在新创建的命名空间创建pod
kubectl run nginx --image=nginx -n roledemo
# 创建一个角色
# role.yaml
kind：role
apiVersion: xxx
...
kunectl apply -f role.yaml
# 查看角色
kubectl get role -n roledemo
# 创建角色绑定
# bind.yaml
kubectl apply -f bind.yaml
# 使用证书识别身份(后续还有操作)
cp /root/TLS/ks8/ca*. ./

```



> ### 准入控制

就是准入控制的列表,如果列表有请求内容，就可以通过

## Ingress

```shell
1. 把端口号对外暴露，通过ip+端口号访问
使用service里面的NodePort实现
2. NodePort缺陷
每个节点都会启动一个端口号，早访问任何节点都是ip端口形式，意味着一个端口只能使用一次，启动一个服务
实际是通过域名跳转到端口
```

ingress和Pod关系

pod和ingress通过service关联的

ingress作为统一入口，由service关联一组pod

![](.\img\POPO20211205-145036.png)



使用ingress对外暴露应用

```shell
# 1. 创建nginx，对外暴露应用
kubectl create deployment web --image=nginx
# 1.2 创建service
kubectl expose deploment web --port=80 --target-port=80 --type=NodePort
# 2. 部署ingress controller
# xxx.yaml
kubectl apply -f xxx.yaml ingress-demo
# 3. 创建imngress规则
# xxx.yaml
kubectl apply -f xxx.yaml ingress-demo01 -o wide
```

## Helm

### 概述

使用之前的部署方式是部署单一服务应用，helm是做容器编排，一次部署多个服务

实现yaml文件的搞笑复用

使用helm应用级别的版本管理

Helm也是一个k8s的包管理工具和linux里面的apy/yum差不多,更方便部署和打包

Helm有三个很重要的概念

> ### Heml

一个客户端的命令行工具

> ### Chart

那yaml打包，是yaml的一个集合,是一个应用描述,资源文件相关的集合

> ### Release

基于chart部署的实体



![](.\img\POPO20211205-152915.png)

需要安装

安装

```shell
1. 下载压缩文件
2. 解压文件，把解压后的helm 目录移动到/usr/bin下
3. 这就安装完成了
```

配置helm仓库

```shell
# 添加仓库
helm repo add 仓库名称 地址
# 查看已天机参数
helm repo list
#删除
helm repo remove 仓库名
```

> #### 部署应用

1 使用命令搜索

```shell
# 搜索
helm search repo 名称
```

2 根据搜索内容进行安装

```shell
# 安装
helm install 安装之后的名称 搜索之后的应用名称
# 查看安装的状态
helm list
helm status 安装之后的名称
```

自己创建chart

```shell
# 创建chart 会生成4个文件
helm create mychart

# 当前chart属性配置信息
charts.yaml
# 编写yaml文件存放目录
templates
# yaml文件可以使用全局变量
values.yaml
```

应用升级

```shell
helm upgrade 名称 chart名称
```

> ### 实现yaml的高效复用

通过传递参数，实现复用

```shell
# 1. values.yaml定义变量和值


# 2. 在具体yaml文件，获取定义变量的值
# 一般yaml不同的参数
image
tag
label
port
replicas

# 在templates中使用values.yaml变量
# 通过表达式使用
# {{ .Values.变量名称 }}
# {{ .Release.Name }}-deploy 获取当前版本的名字
```

## 存储

### 数据卷

本地存储

### nfs

网络存储，pod重启，数据还是存在

1 找一台服务器nfs服务端，安装nfs，设置挂载路径

```shell
# 安装
yum install -y nfs-utils
# 设置挂载路径
vim /etc/exports
# 输入
/fata/nfs *(rw,no_root_squash)
```

2 在k8s集群节点安装nfs

```shell
# 安装即可
yum install -y nfs-utils
```

3 在nfs服务端，启动服务

```shell
# 启动服务
systemctl start nfs
```

4 k8s集群部署应用使得nfs持久网络

```shell
# 在yaml需要设备nfs的配置
# 启动yaml就可以了
```

### PV和PVC

#### pv

持久化存储，对存储资源进行抽象，对外提供可以调用的地方(生产者)

#### PVC

用于调用者，不需要关心内部实现细节(消费者)

#### 实现流程

![](.\img\POPO20211205-164308.png)



## 集群资源监控

### 监控指标

> 集群监控

节点资源利用率

节点数

运行pods

> Pod监控

容器指标

应用程序

### 监控平台

> #### 方案一 prometheus +Grafana

prometheus 

开源的

监控，报警，数据库

已Http协议周期抓取被监控组件的状态，数据

不需要复杂的集成过程，使用http接口接入就可以了

Grafana

开源的数据分析和可视化工具

支持多种数据源

> ### 搭建平台

1 部署prometheus (普罗米修斯)

直接使用yaml文件部署

2 部署Grafana

直接使用Grafana

3 打开Grafana，配置数据源，导入模板

默认是用户名密码是admin

配置数据源，使用prometheus

模板默认有个315
