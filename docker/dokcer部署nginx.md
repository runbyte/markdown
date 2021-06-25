# docker 部署 ==nginx==

```shell
# 拉取镜像
docker pull nginx:1.20

# 运行容器，将容器内部配置文件挂在到宿主机指定位置(匿名挂载)
docker run -d -p 888:80 --name naginx01 -v nginx-demo:/etc/nginx nginx:1.20

# 查看容器
docker ps

# 进入容器
docker exec -it nginx-01 /bin/bash

# 找到 nginx 配置文件
whereis nginx
```

![image-20210621020055418](img\image-20210621020055418.png)

```shell
docker volume inspect juming-nginx
```

![image-20210621020159251](img\image-20210621020159251.png)

所有的docker容器内的卷，没有知道目录的情况下，都在`/var/lib/docker/volumes/xxx/_data`下

我们通过具名挂载可以方便的找到我们的一个卷，大多数情况下都是使用==具名挂载==

```shell
# 如何确定是具名挂载还是匿名挂载，还是指定路径挂载！
# 匿名挂载
-v 容器内路径
# 具名挂载
-v 卷名：容器内路径
# 指定路径挂载
-v /宿主机路径：容器内路径
```

拓展：

```shell
# 通过 -v 容器内路径：ro rw 改变读写权限
ro 只读
rw 可读可写
docker run -d -p 888:80 --name nginx01 -v juming-nginx:/etc/nginx:ro nginx:1.20
docker run -d -p 888:80 --name nginx01 -v juming-nginx:/etc/nginx:rw nginx:1.20
```

