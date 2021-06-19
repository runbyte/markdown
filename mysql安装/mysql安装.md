# mysql glibc版本安装

## 1、配置规划

```shell
默认安装目录：/usr/local/mysql
数据文件目录：/usr/local/mysql/data
端口：3306
默认 socket 文件存放路径：/tmp/mysql.sock 用于客户端与服务器通信的套接字文件
```

## 2、glibc版本的安装步骤

```shell
第一步:上传软件包到Linux操作系统中

第二步:创建特殊的账号，叫做mysq1(所属组mysq1)
[root@localhost ~]# useradd -r -s /sbin/nologin mysq1

第三步:解压mysq1压缩包，解压到/usr/local/mysq1目录
[root@localhost ~] # tar -zxf mysq1-5.6.44-1inux-glibc2.12-x86_64.tar.gz
[root@loca1host ~] # mv mysq1-5.6.44-7inux-glibc2.12-x86_64 /usr/loca1/mysq1

第四步:更改/usr/loca1/mysq1目录权限，更改文件拥有者与所属组都必须为mysq1
[root@loca7host ~] # chown -Rmysq7.mysql /usr/loca1/mysq1

第五步:卸载mariadb-7ibs软件包
[root@loca7host ~]# yum remove mariadb-libs

第六步:初始化数据库
[root@localhost /usr/loca1/mysq1] # scripts/mysq1_insta7ll_db --user=mysq1

第七步:启动mysq1数据库
[root@loca1host /usr/loca1/mysq1] # cp support-files/mysql.server/ etc/init.d/mysql
[root@7ocalhost ~]# service mysql start

第八步:设置MysQL密码
[root@localhost /usr/loca1/mysq1] # bin/mysqladmin -u root password '新密码'

第九步:测试是否安装成功
[root@localhost /usr/loca1/mysq1]# bin/mysq1 -u root -pEnter Password:123
```

