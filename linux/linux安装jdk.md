# 安装JDK

## 新建安装目录

```shell
-- 新建目录
# mkdir /usr/local/software
```

## 卸载自带 openjdk

```shell
-- 查询自带的openjdk软件名称
# rpm -qa|grep "jdk" --color
-- 卸载openjdk
# rpm -e --nodeps '软件名称'
-- 确认openjdk是否成功删除
# rpm -qa|grep "jdk" --color
-- 没有查到结果，说明openjdk已经被成功卸载，可以安装我们上传的标准JDK了
```

## 安装标准JDK

上传文件到 `/usr/local/software`，解压即可

```shell
-- 解压文件到指定目录
# tar -xvf jdk-8u181-linux-i586.tar.gz -C /usr/local/software
```

## 配置JDK环境变量

```shell
-- 用vim打开配置文件
# vim /etc/profile
-- 在文件末尾增加如下内容以后，保存并退出vim
#set java environment
export JAVA_HOME=/usr/local/software/jdk1.8.0_181
export CLASSPATH=.:$JAVA_HOME/lib:$JAVA_HOME/jre/lib
export PATH=$JAVA_HOME/bin:$PATH
-- 使环境变量生效
# source /etc/profile
```

## 检查是否成功

```shell
# java -version
# java
# javac
```

