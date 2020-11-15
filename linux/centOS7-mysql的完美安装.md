# CentOS7 64位安装mysql教程

从最新版本的linux系统开始，默认的是 Mariadb而不是mysql！这里依旧以mysql为例进行展示

1、先检查系统是否装有mysql

```shell
rpm -qa | grep mysql
```

这里返回空值，说明没有安装

这里执行安装命令是无效的，因为centos-7默认是Mariadb，所以执行以下命令只是更新Mariadb数据库

```
yum install mysql
```

删除可用

```
yum remove mysql
```

2、下载mysql的repo源

```
# wget http://repo.mysql.com/mysql-community-release-el7-5.noarch.rpm
```

安装mysql-community-release-el7-5.noarch.rpm包

```
# sudo rpm -ivh mysql-community-release-el7-5.noarch.rpm
```

安装这个包后，会获得两个mysql的yum repo源：/etc/yum.repos.d/mysql-community.repo，/etc/yum.repos.d/mysql-community-source.repo。

3、安装mysql

```
# sudo yum install mysql-server
```

根据步骤安装就可以了，不过安装完成后，没有密码，需要重置密码。

安装后再次查看mysql

```shell
rpm -qa | grep mysql
```

如果报错，内容含有

```
Error: Package: mysql-community-libs-5.6.35-2.el7.x86_64 (mysql56-community)
           Requires: libc.so.6(GLIBC_2.17)(64bit)
Error: Package: mysql-community-server-5.6.35-2.el7.x86_64 (mysql56-community)
           Requires: libc.so.6(GLIBC_2.17)(64bit)
Error: Package: mysql-community-server-5.6.35-2.el7.x86_64 (mysql56-community)
           Requires: systemd
Error: Package: mysql-community-server-5.6.35-2.el7.x86_64 (mysql56-community)
           Requires: libstdc++.so.6(GLIBCXX_3.4.15)(64bit)
Error: Package: mysql-community-client-5.6.35-2.el7.x86_64 (mysql56-community)
           Requires: libc.so.6(GLIBC_2.17)(64bit)
 You could try using --skip-broken to work around the problem
 You could try running: rpm -Va --nofiles --nodigest
   123456789101112123456789101112
```

解决：

```
#yum install glibc.i686
# yum list libstdc++*
```

4、重置密码

重置密码前，首先要登录

```
# mysql -u root
```

登录时有可能报这样的错：ERROR 2002 (HY000): Can’t connect to local MySQL server through socket ‘/var/lib/mysql/mysql.sock’ (2)，原因是/var/lib/mysql的访问权限问题。下面的命令把/var/lib/mysql的拥有者改为当前用户：

```
# sudo chown -R openscanner:openscanner /var/lib/mysql
```

如果报`chown: 无效的用户: "openscanner:openscanner"`错误，更换命令，并用 ll 查看目录权限列表

```
chown root /var/lib/mysql/
ll
```

附：
① 更改文件拥有者 (chown )
[root@linux ~]# chown 账号名称 文件或目录
② 改变文件的用户组用命令 chgrp
[root@linux ~]# chgrp 组名 文件或目录
③ 对于目录权限修改之后，默认只是修改当前级别的权限。如果子目录也要递归需要加R参数
Chown -R : 进行递归,连同子目录下的所有文件、目录

然后，重启服务：

```
service mysqld restart
   11
```

接下来登录重置密码：

```
mysql -u root -p

mysql > use mysql;
mysql > update user set password=password('123456') where user='root';
mysql > exit;
```

重启mysql服务后才生效 `# service mysqld restart`

必要时加入以下命令行，为root添加远程连接的能力。链接密码为 “root”（不包括双引号）

```
mysql> GRANT ALL PRIVILEGES ON *.* TO root@"%" IDENTIFIED BY "root";
```

6、查询数据库编码格式，确保是 UTF-8

```
show variables like "%char%";
```

需要修改编码格式为UTF-8，导入数据库sql的时候，请确保sql文件为utf8编码
进入mysql命令行后 输入

```
set names utf8;
   11
```

（测试数据库数据）
再进入数据库 use test;
在导入sql脚本 source test.sql;

7、开放3306端口号
firewalld 防火墙（centos-7）运行命令,并重启：

```
# firewall-cmd --zone=public --add-port=3306/tcp --permanent
# firewall-cmd --reload
```

iptables 防火墙（centos6.5及其以前）运行命令

```
vim /etc/sysconfig/iptables
```

在文件内添加下面命令行，然后重启

```
-A INPUT -p tcp -m state --state NEW -m tcp --dport 3306 -j ACCEPT
# service iptables restart
```

外部链接访问效果（一般建立sql数据库和数据表，建议通过远程链接控制，直观易操作）

附：

出现“Warning: Using a password on the command line interface can be insecure.”的错误

第一种方法、修改数据库配置文件
1、我们需要修改数据库配置文件，这个要看我们数据库的配置的，有些是在/etc/my.cnf，有些是/etc/my.conf

我们需要在[client]部分添加脚本：

```
socket=/var/lib/mysql/mysql.sock  ( mysql.sock 文件位置 )
host=localhost
user=数据库用户
password='数据库密码'
   12341234
```

这里参数要修改成我们自己的。

2、采用命令导出和导入数据库
其实在这个时候，我们如果采用”详解使用mysqldump命令备份还原MySQL数据用法整理http://www.laozuo.org/5047.html“介绍的方法也是可以使用的，虽然依旧有错误提示，但是数据库还是可以导出的。您肯定和老左一样是追求细节的人，一点点问题都不能有，但我们可以用下面的命令导出和导入，就没有错误提示。

导出数据库

```
mysqldump --defaults-extra-file=/etc/my.cnf database > database.sql
   11
```

导入数据库

```
mysql --defaults-extra-file=/etc/my.cnf database < database.sql
   11
```

这里我们可以看到上面的命令和以前常用的快速导入和导入命令有所不同了，需要加载我们配置的MYSQL配置文件，这个“/etc/my.cnf”要根据我们实际的路径修改。用这样的命令导出备份和导入是没有错误提示的。

登陆数据库

```
# mysql -u root -p
   11
```

第二种方法、利用mysql_config_editor

1、设置加密模式

```
mysql_config_editor set --login-path=local --host=localhost --user=db_user --password
   11
```

“db_user”是需要修改成我们自己数据库用户名的，回车之后会提示我们输入数据库密码，我们照样输入。

2、执行备份

```
mysqldump -u db_user -pInsecurePassword my_database | gzip > backup.tar.gz
   11
```

-u **db_user**
-p.**InsecurePassword** ( 中间的“.”记得去掉 )

根据我们数据信息修改用户和用户名和数据库密码，执行备份，这里老左测试还是有错误提示，但数据库是可以备份的。

修改MySQL的root用户的密码：
mysql -u root mysql
mysql>use mysql;
mysql>desc user;
mysql> GRANT ALL PRIVILEGES ON *.* TO root@”%” IDENTIFIED BY “root”;　　//为root添加远程连接的能力。
mysql>update user set Password = password(‘xxxxxx’) where User=’root’;
mysql>select Host,User,Password from user where User=’root’;
mysql>flush privileges;
mysql>exit;

重新登录：mysql -u root -p
delete from mysql.user where user=”;　 ← 删除匿名用户
select user,host from mysql.user;　 ← 查看用户信息