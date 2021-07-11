# `docker`常用命令

[toc]

## 安装卸载

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

# 可以去阿里云配置一下加速服务
```

## 启动停止

```shell
# 启动
systemctl start docker
# 停止
systemctl stop docker
# 重启
systemctl restart docker
# 查看状态
systemctl status docker
# 开机启动
systemctl enable docker
# 查看概要信息
docker info
# 查看帮助文档
dicker --help
```

## 镜像相关

### 基础命令

```shell
# 查看镜像
docker images
# 搜索镜像
docker search mysql
# 录取镜像
docker pull centos:7
# 删除镜像
docker rmi $IMAGE_ID
# 删除所有镜像
docker rmi `docker images -q`
```

### 保存与加载

```shell
# 镜像保存为文件
docker save -o mynginx.tar mynginx
# 文件加载为镜像
docker load -i mynginx.tar
```

## 容器相关

```shell
docker ps
		#列出当前正在运行的容器
-a		#列出当前正在运行的容器+带出历史运行过的容器
-n=5	#显示最近创建的5个容器
-q		#只显示容器的编号

# 查看运行的容器
docker ps
# 查看所有容器
docker ps –a
# 最后一次运行的容器
docker ps -l
# 查看停止的容器
docker ps -f status=exited

```

### 运行停止

```shell
# 运行容器
docker run
	-i: 运行容器
	-it: 表示容器启动后会进入其命令行
	--name: 为容器命名
	-v: 挂载卷
	-d: 创建一个守护式容器在后台运行
	-p: 端口映射，可以使用多个－p做多个端口映射
# 例如
docker run -d -p 80:80 -v nginx-file:/etc/nginx --name nginx-80 nginx:1.21

# 停止容器
docker stop 容器NAME/ID
# 启动容器
docker start 容器NAME/ID
# 重启
docker restart 容器NAME/ID
# 强制停止当前容器
docker kill 容器NAME/ID
```

### 进入退出容器

```shell
# 我们通常容器都是使用后台方式运行的，需要进入容器，修改一些配置

# 方式一命令
docker exec -it 容器id /bin/bash

# 方式二命令
docker attach 容器id /bin/bash

# 区别
docker exec		进入容器后开启一个新的终端，可以在里面操作（常用）
docker attach	进入容器正在执行的终端
```

### 删除容器

```shell
docker rm $CONTAINER_ID/NAME
# 删除所有
docker rm `docker ps -a -q`
docker rm $(docker ps -aq)
# 删除所有容器
docker ps -a -q|xargs docker rm
```



### 交互式容器

==exit 容器会停止==

```shell
# 创建交互式容器，exit容器，容器会停止运行
docker run -it --name=mycentos centos:7 /bin/bash
```

### 守护式容器

==exit 容器不会停止==

```shelll
# 守护式容器，exit容器不会退出
docker run -di --name=mycentos2 centos:7
```

## 文件拷贝

```shell
# 宿主机 考入 容器
docker cp 需要拷贝的文件或目录 容器名称:容器目录
# 容器 考入 宿主机
docker cp 容器名称:容器目录 需要拷贝的文件或目录
```

## 目录挂载

```shell
# 创建容器 添加-v参数 后边为   宿主机目录:容器目录
docker run -di -v /usr/local/myhtml:/usr/local/myhtml --name=mycentos3 centos:7
# 如果你共享的是多级的目录，可能会出现权限不足的提示。
# 这是因为CentOS7中的安全模块selinux把权限禁掉了，我们需要添加参数--privileged=true来解决挂载的目录没有权限的问题。
docker run -di --privileged=true -v /root/test:/usr/local/test --name=mycentos4 centos:7

# 如何确定是具名挂载还是匿名挂载，还是指定路径挂载！
# 匿名挂载
-v 容器内路径
# 具名挂载
-v 卷名：容器内路径
# 指定路径挂载
-v /宿主机路径：容器内路径
# 通过 -v 容器内路径：ro rw 改变读写权限

# ro 只读
# rw 可读可写
docker run -d -p 888:80 --name nginx01 -v juming-nginx:/etc/nginx:ro nginx:1.20
docker run -d -p 888:80 --name nginx01 -v juming-nginx:/etc/nginx:rw nginx:1.20
```

## `volume`

```shell
docker volume ls
docker volume inspect 名字
```

## 查看日志

```shell
# 显示日志
-tf
--tail
docker logs -tf -t --tail 10 容器ID
```

## 查看容器进程信息

`docker top 容器ID`

## 查看元数据

```shell
# 我们可以通过以下命令查看容器运行的各种数据
docker inspect 容器NAME/ID
# 也可以直接执行下面的命令直接输出IP地址
docker inspect --format='{{.NetworkSettings.IPAddress}}' mycentos2
```

# MYSQL

```shell
# 拉取镜像
docker pull mysql
# 运行容器
docker run -di --name=my_mysql -p 33306:3306 -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7
# 进入容器
docker exec -it my_mysql /bin/bash
# 登陆 mysql
mysql -u root -p
# 授权允许远程登陆
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '123456' WITH GRANT OPTION;
# Navicat 连接
```

# NGINX

```shell
docker pull nginx:1.21

docker run -di -p 80:80 -v nginx-file:/etc/nginx --name=my-nginx nginx:1.21
# 进入挂载卷修改配置

# 也可以先把配置文件拷贝出来，改完再考进去
# 拷出来
docker cp pinyougou_nginx:/etc/nginx/nginx.conf nginx.conf
# 拷进去
docker cp nginx.conf  pinyougou_nginx:/etc/nginx/nginx.conf

```

### 修改配置

```shell
upstream my-service {
	server 172.17.0.7:8080;
}
server {
	listen 80;
	server_name passport.pinyougou.com;
	location / {
		proxy_pass http://my-service;
		index index.html index.htm;
	}
}
```

# `Dockerfile`

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

## 案例制作`JAR`包镜像

```shell
FROM java:8
COPY *.jar /app.jar
CMD ["--server.port=8080"]
EXPOSE 8000
ENTRYPOINT ["java","-jar","/app.jar"]
```

## 制作镜像

```shell
docker build -t admin-service:1.0.0 .
```



# `docker-compose`

```shell
# 下载
https://github.com/docker/compose/releases
# 放到 /usr/local/bin/docker-compose
# 修改此文件的权限，增加可执行
sudo chmod +x /usr/loca1/bin/docker-compose
# 查看版本信息
docker-compose version
```

## `docker-compose.yml`

```yml
# 3层
# 第一层 版本 重要
version: '3.8' 
# 第二层 服务 核心
services:
  my-spbt:
  	# 服务配置
  	images
  	build
  	...
  serv2: redis
  	...
  serv3: xxx
  	...
  	
# 第三层 其他配置 全局规则等，项目没有要求也可以不配
volumes:
networks:
...
```

## 执行

```shell
# 在yml文件目录下
docker-compose up -d
```

## 案例

```yml
version: '3.8' 
services:
  account-service:
    image: account:1.0.0
    restart: always
    ports:
      - "39999:8003"
```



# `wordpress`案例

```yml
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

