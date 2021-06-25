# Docker - Compose

## 安装

```shell
# 第一步
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

# 这个可能快点!
cur1 -L https://get.daocloud.io/docker/compose/releases/download/1.25.5/docker-compose-`uname -s`-`uname -m' > /usr/loca1/bin/docker-compose

# 第二步:修改此文件的权限，增加可执行
sudo chmod +x /usr/loca1/bin/docker-compose

# 查看版本信息
docker-compose version
```



## 体验

1、应用：app.jar

2、Dockerfile 应用打包为镜像

3、Docker-compose yame文件（定义整个服务，需要的环境。web、redis）完整的上线服务！

4、启动compose 项目（`docker compose up`）

流程：

1、创建网络

2、执行 Docker-compose yaml

3、启动服务



## yaml 规则

docker-compose.yaml 核心文件

```yaml
# 3层
# 第一层 版本 重要
version: '' 
# 第二层 服务 核心
services:
  serv1: web
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



## 实战

1、编写项目微服务

2、dockerfile构建镜像

3、docker-compose.yaml 编排项目

4、丢到服务器 docker-compose up



## 小结

未来项目只要有 docker-compose 文件，按照这个规则，启动编排容器

公司：docker-compose 直接启动

网上开源项目：docker-compose 一键搞定