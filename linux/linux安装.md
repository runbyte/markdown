（1）完成上面操作后，用此U盘启动，肯定遇到问题，**最终出现提示符： dracut: /#**

这时，在提示符后输入：

dracut:/# cd  /dev             /* 进到设备列表 */

dracut:/dev # ls               /* 查看设备列表 */

注意：

观察以 sdxx 标识的字符 ，sd 开头，表示硬盘，U盘也算是硬盘 ； 

硬盘是从字符 a 开始排序，sda 表示第一个硬盘 ；

硬盘标识符 sda 后面跟数字，是表示这个硬盘的分区，

比如我电脑上有一个硬盘，分了4个区，使用 ls 将会看到 sda , sda1 sda2 sda5 sda7 ；

（不同的分区方式，在 sda 后面的数字，会有差异，但总的数量与分区数量相符）

 

而安装文件所在的U盘，因为是采用 UltraISO 直接写入，只会有一个区；

( 在这里，将会显示为 sdb , sdb4 ）；

( 比对自己的分区数量，很容易就知道哪个是硬盘，哪个是U盘，记住这个分区标识 sdb4）；

 

（2）重启电脑，在安装提示界面，将光标移到 install CentOS 7 上，按 Tab 键，

随后出现命令：

vmlinuz initrd=initrd.imginst.stage2=hd:LABEL=CentOS\x207\x20x86_64 rd.live.check quiet

移动光标，将命令修改为：

vmlinuz initrd=initrd.imginst.stage2=hd:/dev/sdb4 quiet inst.gpt

（删除了 LABEL=CentOS\x207\x20x86_64 rd.live.check ）

（其中 inst.gpt 是表示，以GPT分区进行安装）

 

至此，成功启动，进入 CentOS 安装界面，按提示进行安装即可；