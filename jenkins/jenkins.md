# Jenkins

## 持续集成及Jenkins介绍

### 什么是持续集成(CI)

频繁的，多次的将代码集成到主干去

> ### 目的

让产品快速迭代，同时还得保持高质量

> ### 组成要素

自动构建

代码仓库

持续集成服务器

> ### 好处

降低风险

减少重复性工作

提供多个版本

持续部署

### Jenkins

搭建持续集成的软件，可用于自动构建,自动测试,自动部署

http://jenkins.io

主要是需要插件的支撑,主要也有1000多个插件

## Jenkins安装和持续集成环境配置

### Gitlab安装

1 安装相关依赖

```shell
yum -y install policycoreutils openssh-clients postfix
```

2 启动ssh和开启自启

```shell
systemctl enable sshd && systemctl start sshd
```

3 设置postfix开启自启，postfix支持gitlab发信功能

```shell
systemctl enable postfix && systemctl start postfix
```

4 防火墙开发http及ssh服务

```shell
firewall-cmd -add-service=ssh --permanent
firewall-cmd -add-service=http --permanent
firewall-cmd --reload
```

5 下载gitlab包

```shell
wget https://xxxxx.rpm
# 安装
rpm -i gitlab-ce-xxx.rpm
```

6 修改gitlab配置

```shell
vim /etc/gitlab/gitlab.rb
```

修改gitlab配置

```shell
external_url 'http://192.168.66.100:82'
nginx['listen_port'] = 82
```

重载配置及启动

```shell
gitlab-ctl reconfigure
gitlab-ctl restart
```

把端口添加到防火墙

```shell
firewall-cmd --zone=public --add-port=82/tcp --permanent
firewall-cmd --reload
```

gitlab的默认账号

```shell
root
root
```

### Jenkins安装

1 安装jdk1.8

```shell
yum install java-1.8.0-openjdk* -y
```

安装目录在/usr/lib/jvm

2 安装jenkins

地址：https://pkg.jenkins.io/redhat/

```shell
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat/jenkins.io.key
```

修改jenkins配置

```shell
vi /etc/sysconfig/jenkins
```

修改内容如下

```shell
JENKINS_USER='root'
JENKINS_PORT="8888"
```

启动jenkins

```shell
systemctl start jenkins
```

注意防火墙，如果没有关闭关闭或者开发8888端口

### 插件管理

修改下载地址

Jenkins->Manage Jenkins->Manage Plugins 点击Available

cd /var/liv/jenkins/updates

set xxxxx

#### 安装Role-Based Authorization Strategy插件

#### 安装Credentials Binding插件

保护与第三方凭证的管理

### pipline语法入门

```python
./components/TestProgress
./components/compatibility
```

## 前端打包失败react

```python
地址:https://github.com/umijs/umi/issues/4569
看路由里面的大小写，与组件里面的大小写是否一致
```

