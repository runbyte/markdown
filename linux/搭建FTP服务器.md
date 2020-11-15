# linux安装ftp步骤

## 1，查看是否安装了FTP：

```bash
rpm -qa |grep vsftpd
```

如果没有任何输出，表示没有安装。

如果出现如下版本信息，则表示已经安装。



## 2，如果没有安装，可以使用如下命令直接安装

```shell
// 默认安装目录：/etc/vsftpd
yum -y install vsftpd
```



## 3，添加FTP账号

```shell
// 该账户路径默认指向/home/admin目录
useradd ftpadmin -s /sbin/nologin

// 设置密码
passwd admin
```

 

## 4，一些常用设置

 ```properties
a，设置匿名用户可以下载上传

将文件/etc/vsftpd/vsftpd.conf 中

anon_upload_enable=YES

anon_mkdir_write_enable=YES

两项启用(去掉前面的#号)

b，设置防火墙：iptables -F

c，设置开机启动sftp：chkconfig vsftpd on

d，根据个人需要设置默认目录

这里设置/www为默认目录。修改/etc/passwd文件，找到你的用户名的那一行修改路径，然后保存即可，无需重启

将admin:x:500:500::/home/admin:/sbin/nologin

改成admin:x:500:500::/www:/sbin/nologin
 ```



## 5，启动

启动：`service vsftpd start `

重启：`service vsftpd restart`



## 6，测试

用浏览器地址栏或者我的电脑地址栏测试下

`ftp://103.96.XX.XX`



输入账号和密码即可打开

用filezilla建立连接



## 其他操作

1. 修改vsftpd的默认根目录
2.  
3. 做实验时有时需要将FTP服务器vsftpd的默认根目录(/var/ftp/pub)修改成指定的其他目录，比如/media/ftp/pub/
4.  
5. 修改vsftpd的配置文件/etc/vsftpd/vsftpd.conf,添加下面三行
6. local_root=/media/ftp/pub
7. chroot_local_user=YES
8. anon_root=/var/www/html/
9.  
10. local_root 表示本地用户登录后的根目录，也就是非匿名，而是输入用户名和密码登录进入的，这里顺便说一下ftp登录的格式
11. ftp://username:passwd@localhost
12.  
13. anon_root anonymous用户，即匿名用户访问的主目录
14.  
15. 但是这时候可能会出现以下报错：
16.  
17. [root@localhost pub]# lftp localhost
18. lftp localhost:~> ls
19. ls: Login failed: 500 OOPS: vsftpd: refusing to run with writable anonymous root
20. 原因还是权限设置问题：
21. 是ftp默认主目录权限设置不对，我这里报这个错误是因为/media/ftp设置权限为777,/media/ftp/pub设置权限也为777。正确的权限设置是将/media/ftp权限设置为755，chmod 755 /media/ftp后重启ftp服务就ok了。
22.  
23.  
24.  
25. 另：
26. 如果你是默认的ftp目录出现此问题，那一定是这个/home/ftp的权限不对所致，这个目录的权限是不能打开所有权限的；是您运行了chmod 777 /home/ftp所致；如果没有ftp用户这个家目录，当然您要自己建一个；
27.  
28. 如下FTP用户的家目录是不能针对所有用户、用户组、其它用户组完全开放；
29. [root@localhost ~]# ls -ld /home/ftp
30. drwxrwxrwx 3 root root 4096 2005-03-23 /home/ftp
31.  
32. 修正这个错误，应该用下面的办法；
33. [root@localhost ~]# chown root:root /home/ftp
34. [root@localhost ~]# chmod 755 /home/ftp
35.  
36. 有的弟兄可能会说，那匿名用户的可读、可下载、可上传怎么办呢？这也简单，在/home/ftp下再建一个目录，权限是777的就行了，再改一改vsftpd.conf就OK了；没有什么难的；
37.  
38. vsFTPd出于安全考虑，是不准让ftp用户的家目录的权限是完全没有限制的，您可以去读一下vsFTPd的文档就明白的了；否则也不能称为最安全的FTP服务器了，对不对？
39. 

