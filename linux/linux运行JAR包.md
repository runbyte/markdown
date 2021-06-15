# 一、运行 JAR

## 1、方式一（ -jar ）

  命令格式：`java -jar xxxx.jar`

```shell
# java -jar test_jar-1.0-SNAPSHOT.jar
```

我们可以看到，此方式运行jar包一个极其严重的缺点：锁定窗口。当然，我可以通过CTRL + C打断程序运行，或直接关闭窗口，程序退出，不过在实际的工作环境中，是绝对不会允许这种粗暴的方式终止的运行，毕竟我们的项目都是给用户提供服务的，程序要是退出了，公司还营不营业了。

## 2、方式二（ & ）

 命令格式：`java -jar xxxx.jar &`

```shell
# java -jar test_jar-1.0-SNAPSHOT.jar &
```

`&`代表在后台运行。优点是当前ssh窗口不被锁定，但是当窗口关闭时，程序终止运行。

那么我们就会想，如何继续改进，让窗口关闭时，程序仍然运行呢？

## 3、方式三（ nohup ）

方式三主要是引入nohup命令，具体使用主要包括下面三种方式。

**（1）命令格式：`nohup java -jar xxx.jar &`**

```shell
# nohup java -jar test_jar-1.0-SNAPSHOT.jar &
```

`nohup`意思是不挂断运行命令，当账户退出或终端关闭时，程序仍然运行。

当用 nohup 命令运行jar包时，缺省情况下该应用的所有输出被重定向到nohup.out的文件中，除非另外指定了输出文件。

**（2）命令格式：`nohup java -jar xxx.jar > log.txt &`**

```shell
# nohup java -jar test_jar-1.0-SNAPSHOT.jar > log.txt &
```

发现输出都重定向到了log.txt文件里面。

**（3）命令格式：`nohup java -jar xxx.jar >output 2>&1 &`**

```shell
# nohup java -jar test_jar-1.0-SNAPSHOT.jar >output 2>&1 &
```

```properties
linux shell中"2>&1"含 义：
	对于& 1 更准确的说应该是文件描述符 1，而1表示标准输出stdout。
	对于2 ，表示标准错误stderr。
	2>&1 的意思就是将标准错误重定向到标准输出。
```

而标准输出又导入文件output里面，所以结果是标准错误和标准输出都导入文件output里面了。

至于为什么需要将标准错误重定向到标准输出的原因，那就归结为标准错误没有缓冲区，而stdout有。这就会导致 >output 2>output， 文件output被两次打开，而stdout和stderr将会竞争覆盖，这肯定不是我门想要的。

**（4）命令格式：`nohup java -jar xxx.jar >/dev/null 2>&1 &`**

```shell
# nohup java -jar test_jar-1.0-SNAPSHOT.jar >/dev/null 2>&1 &
```

```properties
这里谈一下/dev/null文件的作用。/dev/null是一个虚拟的空设备，可以看作是一个"黑洞"，黑洞的特性是只进不出，哪怕是光都逃不掉，/dev/null亦是如此。
```

它等价于一个只写文件，所有写入它的内容都会永远丢失。如果尝试从它那儿读取内容，那么什么也读不到。

明白了/dev/null之后，我们再来看这个命令。>/dev/null 表示将标准输出信息重定向到"黑洞"，2>&1 表示将标准错误重定向到标准输出，由于标准输出已经重定向到“黑洞”了（即：标准输出此时也是"黑洞"），再将标准错误输出定向到标准输出，相当于标准错误输出也被定向至“黑洞”。

## 4、脚本方式

脚本方式的本质依然是上面介绍的那些命令，只不过shell脚本更加的方便，并且可以干更多的事情。

这里依然以test_jar-1.0-SNAPSHOT.jar这个小demo为例，编写如下脚本start.sh

```sh
#!/bin/bash
 
#这里可替换为你自己的执行程序，其他代码无需更改
APP_NAME=test_jar-1.0-SNAPSHOT.jar
 
 
#使用说明，用来提示输入参数
usage() {
    echo "Usage: sh 脚本名.sh [start|stop|restart|status]"
    exit 1
}
 
#检查程序是否在运行
is_exist(){
  pid=`ps -ef|grep $APP_NAME|grep -v grep|awk '{print $2}' `
  #如果不存在返回1，存在返回0     
  if [ -z "${pid}" ]; then
   return 1
  else
    return 0
  fi
}
 
#启动方法
start(){
  is_exist
  if [ $? -eq "0" ]; then
    echo "${APP_NAME} is already running. pid=${pid} ."
  else
    nohup java -Xmx512m -Xms512m -jar $APP_NAME  > test2.txt 2>&1 &
    echo "${APP_NAME} start success"
  fi
}
 
#停止方法
stop(){
  is_exist
  if [ $? -eq "0" ]; then
    kill -9 $pid
  else
    echo "${APP_NAME} is not running"
  fi  
}
 
#输出运行状态
status(){
  is_exist
  if [ $? -eq "0" ]; then
    echo "${APP_NAME} is running. Pid is ${pid}"
  else
    echo "${APP_NAME} is NOT running."
  fi
}
 
#重启
restart(){
  stop
  start
}
 
#根据输入参数，选择执行对应方法，不输入则执行使用说明
case "$1" in
  "start")
    start
    ;;
  "stop")
    stop
    ;;
  "status")
    status
    ;;
  "restart")
    restart
    ;;
  *)
    usage
    ;;
esac
```

执行脚本：

`cd /home` (我是把jar包和脚本放在home下面)

`chmod +x start.sh` (第一次运行的时候获取一下超级管理员权限)

然后以后每次运行直接

`./start.sh`即可

```sh
# sh start.sh
Usage:sh xxxxxxxx  [start|stop|restart|status] -- 命令提示
# sh start.sh start
xxx.jar start success -- 启动成功
# sh start.sh status
xxx.jar is running. Pis is 2184
# sh start.sh stop
# sh start.sh status
xxx.jar is NOT running.
```

# 二、停止 JAR

## 1、方式一

根据端口号查询进程编号，在根据进程编号关闭。

```shell
# netstat -lnp|grep 端口号
```

通过`kill -9 进程编号`关闭

```shell
# kill -9 进程编号
```

## 2、方式二

直接使用 `jps` 列出所有启动的`Java`线程，然后使用`kill -9 进程号`关闭。

## 3、方式三

```shell
# ps -ef|grep XXX.jar
```

# 三、常见问题

`java -jar XXX.jar`中没有主清单属性

代码在开发工具里面启动时没有问题，但是达成`jar`启动的时候回遇到这个问题。其实程序告诉我们，他迷路啦，找不到`main`方法啦。所以我们需要在`pom.xml`配置`main`方法的位置。

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
        </plugin>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <version>2.2.2.RELEASE</version>
            <configuration>
                <!-- main方法的地址 只需要修改这个地址-->
                <mainClass>com.security.user.AuthServerApplication</mainClass>
            </configuration>
            <executions>
                <execution>
                    <goals>
                        <goal>repackage</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

