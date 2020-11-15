# LINUX遇到的问题及解决

## 安装JDK遇到的问题

今天在安装jdk的时候出现了一个问题：

`-bash: /usr/local/src/jdk1.8.0_191/bin/java: /lib/ld-linux.so.2: bad ELF interprete`

之前一直以为是自己的安装步骤不对，但是想想好像就解压， 配置路径，不应该不对呀

后面搜了一下，原来是jdk的版本不对，只要执行：`sudo yum install glibc.i686`就可以解决了



