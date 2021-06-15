Centos7默认安装了firewalld，如果没有安装的话，可以使用 ***yum install firewalld firewalld-config***进行安装。

1.启动防火墙

```
systemctl start firewalld 
```

2.禁用防火墙

```
systemctl stop firewalld
```

3.设置开机启动

```
systemctl enable firewalld
```

4.停止并禁用开机启动

```
sytemctl disable firewalld
```

5.重启防火墙

 

```
firewall-cmd --reload
```

6.查看状态

```
systemctl status firewalld或者 firewall-cmd --state
```

7.查看版本

```
firewall-cmd --version
```

8.查看帮助

```
firewall-cmd --help
```

9.查看区域信息

```
firewall-cmd --get-active-zones
```

10.查看指定接口所属区域信息

```
firewall-cmd --get-zone-of-interface=eth0
```

11.拒绝所有包

```
firewall-cmd --panic-on
```

12.取消拒绝状态

```
firewall-cmd --panic-off
```

13.查看是否拒绝

```
firewall-cmd --query-panic
```