# linux基础知识

# 手动分区

至少要分三个分区：

- boot分区：
  - 挂载点：`/boot` linux在启动的时候，需要引导文件，会默认放在boot分区下，200M足以
- swap分区：
  - 无挂载点，文件系统类型选择`swap`  设置 2048M，一般是物理内存的1.5倍到2倍，当系统内存不够用的时候，会用swap暂时充当内存
- 根分区：
  - 挂载点：`/` 剩余空间全部给根分区



# 终端使用

终端的使用，点击鼠标右键，点击终端打开



# 配置上网

点击右上角网络网卡



# VMTools

虚拟机可以实现windows和linux之间复制粘贴和文件夹共享



# 文件系统目录结构

## 基本介绍

linux的文件系统是采用级层式的树状目录结构，在此结构中的最上层是根目录`/`，然后在此目录下再创建其他的目录

`/`  根目录，只有一个，所有的其他目录都在根目录下，例如：`/boot /bin /home /root`

**在linux世界里，一切皆文件**，包括硬件。例如声卡，网卡，硬盘等都会被映射成文件来管理。

例如：

```properties
/dev	设备管理目录
/media	多媒体目录
硬件例如：
CPU DISK会被映射到/dev目录下进行管理
DVD U盘会在/media目录被识别
```

## 目录详解

```properties
具体的目录结构:

/bin[重点]
(/usr/bin、 /usr/local/bin)
是Binary的缩写,这个目录存放着最经常使用的命令

/sbin
(/usr/sbin、 /usr/local/sbin)
s就是super User的意思，这里存放的是系统管理员使用的系统管理程序。

/home[重点]
存放普通用户的主目录，在Linux中每个用户都有一个自己的目录，一般该目录名是以用户的账号命名的。

/root[重点]
该目录为系统管理员，也称作超级权限者的用户主目录。

/lib
系统开机所需要最基本的动态连接共享库，其作用类似于windows里的DLL文件。几乎所有的应用程序都需要用到这些共享库。

/lost+found
这个目录一般情况下是空的，当系统非法关机后，这里就存放了一些文件。

/etc[重点]
所有的系统管理所需要的配置文件和子目录

/usr[重点]
这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似与windows下的program files目录。

/boot[重点]
存放的是启动Linux时使用的一些核心文件，包括一些连接文件以及镜像文件

/proc  -- 与linux内核相关，轻易不可动
这个目录是一个虚拟的目录，它是系统内存的映射，访问这个目录来获取系统信息。

/srv  -- 与linux内核相关，轻易不可动
service缩写，该目录存放一些服务启动之后需要提取的数据。

/sys  -- 与linux内核相关，轻易不可动
这是linux2.6内核的一个很大的变化。该目录下安装了2.6内核中新出现的一个文件系统ysfs

/tmp
这个目录是用来存放一些临时文件的。

/dev
类似于windows的设备管理器，把所有的硬件用文件的形式存储。

/media[重点]
linux系统会自动识别一些设备，例如u盘、光驱等等，当识别后，linux会把识别的设备挂载到这个目录下。

/mnt[重点]
系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将外部的存储挂载在/mnt/上，然后进入该目录就可以查看里的内容了。

/opt  -- 默认安装包放这儿，约定俗成的不强制
这是给主机额外软件安装包所摆放的目录。如安装ORACLE数据库就可放到该目录下。默认为空。

/usr/local[重点]
这是另一个给主机额外安装软件所安装的目录。一般是通过编译源码方式安装的程序

/var[重点]
这个目录中存放着在不断扩充着的东西，习惯将经常被修改的目录放在这个目录下。包括各种日志文件。

/selinux
SELinux是一种安全子系统,它能控制程序只能访问特定文件。

```

**/opt和/usr/local目录的区别**

```properties
一、opt目录

/opt目录用来安装附加软件包，是用户级的程序目录，可以理解为D:/Software。
安装到/opt目录下的程序，它所有的数据、库文件等等都是放在同个目录下面。
opt有可选的意思，这里可以用于放置第三方大型软件（或游戏），当你不需要时，直接rm -rf掉即可。
在硬盘容量不够时，也可将/opt单独挂载到其他磁盘上使用。

二、/usr/local目录

/usr：系统级的目录，可以理解为C:/Windows/。
/usr/lib：理解为C:/Windows/System32。
/usr/local：用户级的程序目录，可以理解为C:/Progrem Files/。用户自己编译的软件默认会安装到这个目录下。
这里主要存放那些手动安装的软件，即不是通过“新立得”或apt-get安装的软件。它和/usr目录具有相类似的目录结构。
让软件包管理器来管理/usr目录，而把自定义的脚本(scripts)放到/usr/local目录下面。

三、总结

其实安装软件程序并不是非要在指定的目录下完成，安装java、tomcat等也可以安装在opt目录下。
但是安装程序的扩展性和管理性来说，方便使用才是最好的。
总的来说就是/usr/local下一般是你安装软件的目录，这个目录就相当于在windows下的programefiles这个目录。
/opt这个目录是一些大型软件的安装目录，或者是一些服务程序的安装目录 。
```



## 注意事项

如果希望安装好XShell 5就可以远程访问Linux系统的话，需要有一个前提，就是Linux启用了SSHD服务，该服务会监听22号端口。



# vi 和 vim 编辑器

所有的Linux系统都会内建vi文本编辑器。

Vim具有程序编辑的能力，可以看做是vi的增强版本，可以主动的以字体颜色辨别语法的正确性，方便程序设计。代码补完、编译及错误跳转等方便编程的功能特别丰富，在程序员中被广泛使用。



## vi 和 vim 三种模式

**正常模式：**

以vim打开一个档案就直接进入一般模式了(这是默认的模式)。在这个模式中，你可以使用『上下左右』按键来移动光标，你可以使用『删除字符』或『删除整行』来处理档案内容，也可以使用『复制、贴上』来处理你的文件数据。

**插入模式：**

按下`i,l, o,O, a,A, r,R`等任何一个字母之后才会进入编辑模式,一般来说按`i`即可.

**命令行模式：**

在这个模式当中，可以提供你相关指令，完成读取、存盘、替换、离开vim、显示行号等的动作则是在此模式中达成的！

**三种模式转换**

vim filename -> 普通模式

普通模式  输入 i a o  -->  插入模式  按ESC  -->  普通模式

普通模式  输入 ：-->  命令行模式  按ESC  -->  普通模式

![](G:\GitHub_Repository\runbyte\markdown\linux\vim-vi-workmodel.png)

## 插入和退出命令

```shell
quitting
:x - exit, saving changes
:wq - exit, saving changes
:q - exit, if no changes
:q! - exit，ignore changes

inserting text
i - insert before cursor
I - insert before line
a - append after cursor
A - append after line
o - open new line after cur line
O - open new line before cur line
r - replace one character
R - replace many characters

```



## 常用快捷键

```properties
yy 5yy p
拷贝当前行yy,拷贝当前行向下的5行5yy，并粘贴p

dd 5dd
删除当前行dd ,删除当前行向下的5行5dd

/hello 回车查找，n下一个
:noh清除查找
在文件中查找某个单词。命令行下/关键字，回车查找，输入n 就是查找下一个

:set nu
:set nonu
设置文件的行号，取消文件的行号.[命令行下: set nu和:set nonu]

gg
G
H：屏幕头
M：屏幕中
L：屏幕尾
编辑letc/profile文件，使用快捷键到底文档的最末行[G]和最首行[gg]

-- 退出到正常模式，按u
在一个文件中输入""hello”,然后又撤销这个动作 u

-- 正常模式输入20，然后按shift+g
编辑letcprofile文件，并将光标移动到20行shift+g


```



# 关机重启指令

## 基本命令介绍

```shell
shutdown -h now		立刻进行关机

shutdown -h 1		"hello,1分钟后会关机了"

shutdown -r now		现在重新启动计算机

halt				关机，作用和上面一样

reboot				现在重新启动计算

sync				把内存的数据同步到磁盘
```

## 注意事项

无论是重启系统还是关闭系统，首先要运行`sync`命令，把内存中的数据写到磁盘中





















