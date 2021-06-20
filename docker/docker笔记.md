# Docker概述

类比开发安卓：

Java -- apk -- 发布（应用商店） -- 张三使用apk -- 下载安卓应用！

Java -- jar（环境） --- 打包项目带上环境（镜像） --- （Docker仓库：商店） --- 下载我们发布的镜像

（根本无需考虑环境问题，因为镜像中集成了环境）



Docker的思想来自于集装箱！

Docker通过隔离机制，可以将服务器利用到极致。

传统：JRE -- 多个应用（端口冲突）-- 原来都是交叉的

隔离：Docker核心思想！打包装箱！每个箱子都是互相隔离的



# Docker历史

在容器技术出来之前，我们都是使用虚拟机技术！

虚拟机：在window中装一个Vmware，通过这个软件我们可以虚拟出来一台或者多台电脑！笨重！

容器：

虚拟机属于虚拟化技术，Docker容器技术，也是一种虚拟化技术！

```shell
vm: linux centos 原生镜像（相当于一台电脑）如果想要隔离，需要开启多个虚拟机！
docker: 如果要隔离，镜像（最核心的环境只有4mb，放一些命令和开机启动等）
按需求制作自己的镜像：核心环境 4MB + jdk + mysql 按需求打包，十分轻巧
虚拟机动辄几个G，非常笨重；启动需要几分钟
容器最小有KB级别，一般几M或者几十MB，几百MB已经非常大了；容器秒级启动
```



Docker是基于GO语言开发的

官网：www.docker.com

文档地址：https://docs.docker.com

仓库地址：https://hub.docker.com 类似github，可以将自己开发的镜像发布上去，也可以下载

# Docker能干嘛

==容器化技术不是模拟的一个完整的操作系统==

容器之间是互相隔离的，每个容器都有运行环境和APP，容器直接运行在操作系统之上，可以充分的利用操作系统的资源

**比较Docker和虚拟机技术的不同：**

传统虚拟机，虚拟出一条硬件，运行一个完整的操作系统，然后在这个系统上安装和运行软件。

容器内的应用直接运行在宿主机的内核上，容器是没有自己的内核的，也没有虚拟硬件，所以就轻便了

每个容器间是互相隔离的，每个容器内都有一个属于自己的文件系统，互不影响

>DevOps（开发、运维）

**应用更快速的交付和部署**

传统：一堆帮助文档，安装程序

Docker：打包镜像发布测试，一键运行

**更便捷的升级和扩容**

使用了Docker之后，我们部署应用就和搭积木一样

项目打包为一个镜像，扩展服务器

**更简单的系统运维**

在容器化之后，我们的开发，测试环境都是高度一致的

**更高效的计算资源利用**

Docker是内核级别的虚拟化，可以在一个物理机上运行很多的容器实例！服务器的性能可以体现到极致

# Docker安装

## 基本组成

![img](img\idig8.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg)

**镜像（image）**

docker镜像就好比是一个模板，可以通过这个模板来创建容器服务

tomcat镜像 ==> run ==> tomcat01容器（提供服务）通过这个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中的）

**容器（container）**

Docker利用容器技术，独立运行一个或者一个组应用，通过镜像来创建的

启动，停止，删除，基本命令！

目前就可以把这个容器理解为就是一个简易的linux系统

**仓库（repository）**

仓库就是存放镜像的地方！

仓库分为共有仓库和私有仓库！

官方：Docker Hub（默认是国外的）

阿里云：都有容器服务（配置镜像加速）

## 环境准备

CentOS 7

Xshell

```shell
# 系统内核是 3.10 以上的
[root@localhost ~]# uname -r
3.10.0-1127.el7.x86_64
```

```shell
# 系统版本
[root@localhost ~]# cat /etc/os-release 
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"
```

## 安装

```shell
#1、卸载旧版本
yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
#2、需要的安装包
yum install -y yum-utils

#3、设置镜像的仓库
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo # 默认是国外的
    
yum-config-manager \
    --add-repo \
    http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo # 建议安装阿里云的

# 安装之前更新yum软件包索引
yum makecache fast

#4、安装docker相关的内容
# 安装：核心-客户端-容器
# docker-ce 社区
# docker-ee 企业版
yum install docker-ce docker-ce-cli containerd.io

#5、启动docker
systemctl start docker

#6、测试是否安装成功
docker version

#7、运行HelloWorld
docker run hello-world

#8、查看一下下载的 hello-world 镜像
docker images

#9、卸载docker
# 卸载依赖
yum remove docker-ce docker-ce-cli containerd.io
# 删除资源
rm -rf /var/lib/docker
rm -rf /var/lib/containerd
```

## run流程

![image-20210617222911815](img\image-20210617222911815.png)

## 底层原理

**docker是怎么工作的**

Docker是一个Client - Server结构的系统，Docker的守护进程运行在主机上。通过Socket从客户端访问！

DockerServer接收到 Docker-Client的指令，就会执行这个命令！

![image-20210617223712341](img\image-20210617223712341.png)

**docker为什么比VM快**

docker有这比虚拟机更少的抽象层

docker利用的是宿主机的内核，VM需要是GuestOS

![image-20210617224003303](img\image-20210617224003303.png)

所以说，新建一个容器的时候，docker不需要想虚拟机一样重新加载一个操作系统内核，避免引导。虚拟机是加载Guest OS，分钟级别的

而docker是利用宿主机的操作系统，省略了这个复杂的过程，秒级！

# Docker常用命令

## 帮助命令

```shell
# 显示docker的版本信息
docker version
# 显示docker的系统信息，包括镜像和容器数量
docker info
# 帮助命令
docker 命令 --helo
```

## 镜像命令

### 查看镜像

`docker images`：查看所有本地主机上的镜像

```shell
[root@localhost /]# docker images
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    d1165f221234   3 months ago   13.3kB

# 解释
REPOSITORY	镜像的仓库源
TAG			镜像的标签
IMAGE ID	镜像的id
CREATED		镜像的创建时间
SIZE		镜像的大小

```

### 搜索镜像

`docker search`：搜索镜像

```shell
[root@localhost /]# docker search mysql
NAME            DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql           MySQL is a widely used, open-source relation…   11003     [OK]       
mariadb         MariaDB Server is a high performing open sou…   4169      [OK]

# 解释
# 可选项
# 通过星数量来过滤，不小于3000
--filter=STARS=3000
```

### 下载镜像

`docker pull`：镜像拉取

```shell
# 下载镜像 docker pull 镜像名[:tag]，默认用最新版
[root@localhost /]# docker pull mysql
Using default tag: latest # 如果不写 tag 默认就是最新版
latest: Pulling from library/mysql
69692152171a: Pull complete # 分层下载， docker image的核心，联合文件系统
1651b0be3df3: Pull complete 
951da7386bc8: Pull complete 
0f86c95aa242: Pull complete 
37ba2d8bd4fe: Pull complete 
6d278bb05e94: Pull complete 
497efbd93a3e: Pull complete 
f7fddf10c2c2: Pull complete 
16415d159dfb: Pull complete 
0e530ffc6b73: Pull complete 
b0a4a1a77178: Pull complete 
cd90f92aa9ef: Pull complete 
Digest: sha256:d50098d7fcb25b1fcb24e2d3247cae3fc55815d64fec640dc395840f8fa80969 # 签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest # 真实地址

# 相互等价
docker pull mysql
docker.io/library/mysql:latest

# 指定版本下载
[root@localhost /]# docker pull mysql:5.7
5.7: Pulling from library/mysql
69692152171a: Already exists  # 已存在的不再下载
1651b0be3df3: Already exists 
951da7386bc8: Already exists 
0f86c95aa242: Already exists 
37ba2d8bd4fe: Already exists 
6d278bb05e94: Already exists 
497efbd93a3e: Already exists 
a023ae82eef5: Pull complete 
e76c35f20ee7: Pull complete 
e887524d2ef9: Pull complete 
ccb65627e1c3: Pull complete 
Digest: sha256:a682e3c78fc5bd941e9db080b4796c75f69a28a8cad65677c23f7a9f18ba21fa
Status: Downloaded newer image for mysql:5.7
docker.io/library/mysql:5.7
```

### 删除镜像

`docker rmi`：删除镜像

```shell
# 通过 id 来删除镜像
[root@localhost /]# docker rmi -f c0cdc95609f1
Untagged: mysql:latest

# 删除指定镜像
docker rmi -f 镜像ID
# 删除多个镜像
docker rmi -f 镜像ID 镜像ID 镜像ID
# 删除全部镜像
docker rmi -f $(docker images -aq)
```

## 容器命令

**说明：有了镜像才可以创建容器，linux，下载一个centos镜像来学习**

```shell
docker pull centos
```

### 新建容器并启动

```shell
docker run [可选参数] image

# 参数说明
--name="Name"	容器名字 tomcat01  tomcat02 用来区分容器
-d				后台方式运行
-it				使用交互方式运行，进入容器查看内容
-P（大写）		指定容器的端口 -p 8080:8080 -p 8080
	-P ip:主机端口:容器端口
	-P 主机端口:容器端口（常用）
	-P 容器端口
-p（小写）		随机指定端口

# 测试
# 启动并进入容器
[root@localhost /]# docker run -it centos /bin/bash
# 查看容器内部的centos，基础版本，很多命令都是不完善的
[root@fb1bdb0d2cac /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
# 退出容器，回到主机
[root@fb1bdb0d2cac /]# exit
exit
```

### 守护容器

创建一个守护式容器：如果对于一个需要长期运行的容器来说，我们可以创建一个守护式容器

命令如下（容器名称不能重复）：

```shell
docker run -di --name=mycentos2 centos
```

登录守护式容器：

docker exec -it container_name (或者container_id)/bin/bash（exit退出时，容器不会停止）

命令如下：

```shell
docker exec -it mycentos2 /bin/bash
```



### 列出运行的容器

```shell
docker ps
		#列出当前正在运行的容器
-a		#列出当前正在运行的容器+带出历史运行过的容器
-n=1	#显示最近创建的容器
-q		#只显示容器的编号

# 查看运行的容器
docker ps
# 查看运行过的容器
docker ps -a
```

### 退出容器

```shell
exit	#直接容器停止并退出
CTRL+P+Q	# 容器不停止退出
```

### 删除容器

```shell
# 删除指定的容器，不能删除正在运行的容器，如果要强制删除，那就 rm -f
docker rm 容器ID
# 删除所有容器
docker rm -f $(docker ps -aq)
# 删除所有容器
docker ps -a -q|xargs docker rm
```

### 启动和停止容器

```shell
# 启动容器
docker start 容器ID
# 重启容器
docker restart 容器ID
# 停止容器
docker stop 容器ID
# 强制停止当前容器
docker kill 容器ID
```

## 常用其他命令

**后台启动容器**

```shell
# 通过 docker run -d 镜像名
docker run -d centos

# 问题 docker ps，发现 centos 停止了

# 常见的坑：docker 容器使用后台运行，就必须要有要一个前台进程，docker发现没有应用，就会自动停止
# nginx，容器启动后，发现自己没有提供服务，就会立刻停止
```

**查看日志**

```shell
# 显示日志
-tf
--tail
docker logs -tf -t --tail 10 容器ID
```

**查看容器中的进程信息**

```shell
docker top 容器ID
```

**查看镜像元数据**

```shell
docker inspect 容器ID
```

**进入当前正在运行的容器**

```shell
# 我们通常容器都是使用后台方式运行的，需要进入容器，修改一些配置

# 方式一命令
docker exec -it 容器id bash

# 方式二命令
docker attach 容器id bash

# 区别
docker exec		进入容器后开启一个新的终端，可以在里面操作（常用）
docker attach	进入容器正在执行的终端
```

**从容器内拷贝文件到主机上**

```shell
# 将容器内的文件拷贝到指定目录
docker cp 容器ID:/home/test.java /home
```



![image-20210618002534690](img\image-20210618002534690.png)





# Docker镜像讲解

## 镜像是什么

镜像是一种轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件所需的所有内容，包括代码、运行时、库、环境变量和配置文件。

所有的应用，直接打包docker镜像，就可以直接跑起来！

如何得到镜像：

- 从远程仓库下载

- 朋友拷贝给你

- 自己制作一个镜像DockerFile

## Docker镜像加载原理

![image-20210618010740419](img\image-20210618010740419.png)

![image-20210618012115273](img\image-20210618012115273.png)

![image-20210618012213343](img\image-20210618012213343.png)



## 分层理解

![image-20210618012511423](img\image-20210618012511423.png)

![image-20210618012629600](img\image-20210618012629600.png)

![image-20210618012748659](img\image-20210618012748659.png)

![image-20210618012930919](img\image-20210618012930919.png)

![image-20210618013140586](img\image-20210618013140586.png)

# Commit镜像

![image-20210618014351409](img\image-20210618014351409.png)

# 容器数据卷



# DockerFile

## DockeFile介绍

dockerfile是用来构建dokcer镜像的文件!命令参数脚本

构建步骤：

1、编写一个dockerfile文件

2、docker build构建成为一个镜像

3、docker run运行镜像

4、docker push发布镜像( DockerHub、阿里云镜像仓库)

```shell
FROM scratch
ADD centos-7-x86_64-docker.tar.xz /

LABEL \
    org.label-schema.schema-version="1.0" \
    org.label-schema.name="CentOS Base Image" \
    org.label-schema.vendor="CentOS" \
    org.label-schema.license="GPLv2" \
    org.label-schema.build-date="20201113" \
    org.opencontainers.image.title="CentOS Base Image" \
    org.opencontainers.image.vendor="CentOS" \
    org.opencontainers.image.licenses="GPL-2.0-only" \
    org.opencontainers.image.created="2020-11-13 00:00:00+00:00"

CMD ["/bin/bash"]
```

很多官方的镜像都是基础包，很多功能都没有，我们通常会自己搭建自己的镜像！

官方既然可以制作镜像，我们也可以！

## DockerFile构建过程

基础知识：

1、每个保留关键字（指令）都必须是大写字母

2、执行从上到下顺序执行

3、# 表示注释

4、每一个指令都会创建提交一个新的镜像层，并提交

![image-20210618015805680](img\image-20210618015805680.png)

dockerfile是面向开发的，我们以后要发布项目，做镜像，就需要编写dockerfile文件，这个文件是十分简单的！

Docker镜像逐渐成为了企业交付的标准，必须要掌握！

步骤：开发、部署、运维。。。缺一不可

DockerFile：构建文件，定义了一切的步骤，源代码

DockerImages：通过DockerFile构建生成的镜像，最终发布和运行的产品！

（原来是jar  war交付给客户，现在直接生成镜像给用户）

Docker容器：容器就是镜像运行起来提供服务的



## DockerFile的指令

```shell
FROM		# 基础镜像，一切从这里开始构建
MAINTAINER	# 镜像是谁写的：姓名+邮箱
RUN			# 镜像构建的时候需要运行的命令
ADD			# 步骤：tomcat镜像，这个tomcat压缩包就是添加内容
WORKDIR		# 镜像的工作目录
VOLUME		# 挂在的目录
EXPOSE		# 指定暴露端口
CMD			# 指定容器启动的时候要运行的命令，只有最后一个会生效，可被替代
ENTRYPOINT	# 指定容器启动的时候要运行的命令，可以追加命令
ONBUILD		# 当构建一个被继承 DockerFile 这个时候就会运行 ONBUILD 的指令。触发指令
COPY		# 类似ADD，将我们的文件拷贝到镜像中
ENV			# 构建的时候设置环境变量
```

![image-20210618020311369](img\image-20210618020311369.png)

## 实战测试

Docker Hub中99%镜像都是从这个基础镜像过来的`FROM scratch`，然后配置需要的软件和配置来进行的构建

> 创建一个自己的 centos

1、编写DockerFile

![image-20210618021745644](img\image-20210618021745644.png)

2、通过这个文件构建镜像

```shell
docker build -f mydockerfile-centos -t mycentos:0.1 .
```

查看镜像构建历史

```shell
docker history 镜像ID
```

## 实战tomcat镜像

1、准备镜像文件：tomcat压缩包，jdk压缩包！

2、编写 dockerfile 文件

官方命名：`Dockerfile`，build会自动寻找这个文件

![image-20210618024017043](img\image-20210618024017043.png)

3、构建镜像

```shell
docker build -t diytomcat .
```















