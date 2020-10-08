

# 图床工具的使用---PicGo

所谓图床工具，就是自动把本地图片转换成链接的一款工具，网络上有很多图床工具，就目前使用种类而言，PicGo 算得上一款比较优秀的图床工具。它是一款用 `Electron-vue` 开发的软件，可以支持微博，七牛云，腾讯云COS，又拍云，GitHub，阿里云OSS，SM.MS，imgur 等8种常用图床，功能强大，简单易用



我们到下面的链接下载最新版本，

https://github.com/Molunerfinn/PicGo/releases

注意：*mac* 系统选择 *dmg* 下载，*windwos* 选择 *.exe*系统



# GitHub

在此，我们就拿 *GitHub* 作为图床来说一下具体用法。由于在国内访问 *GitHub* 速度不是很快，不过如果你有特殊工具或者适应这样的速度的话，那就不妨使用一下，这是个不错的选择

首先登陆 *GitHub*，新建一个仓库或者也可以使用一个已有仓库

创建好后，需要在 GitHub 上生成一个 *token* 以便 PicGo 来操作我们的仓库，来到个人中心，选择 *Developer settings* 就能看到 *Personal access tokens*，我们在这里创建需要的 *token*

点击 Generate new token 创建一个新 token，选择 repo，同时它会把包含其中的都会勾选上，我们勾选这些就可以了。然后拉到最下方点击绿色按钮，Generate token 即可。之后就会生成一个 *token* ，记得复制保存到其他地方，这个 *token* 只显示一次！！



# 配置 PicGo

打开 PicGo 面板，

- 仓库名格式为 `用户名/仓库名`
- 分支名：master
- token：上一个咱们创建的token

然后点击确定即可完成绑定，然后设置成默认图床



# 配置Typora

**文件--偏好设置--图像**

参考图片中的进行配置。

![image-20201008202725479](https://raw.githubusercontent.com/runbyte/picBed/master/image-20201008202725479.png)

选择本机 PicGo 的路径。

上面的设置完成后，在 Typora 里写字时，就可以自动上传图片到图床啦。



使用快捷键 **Ctrl + Shift + I**，可以调出插入图片的功能。



也可以直接复制图片，然后再编辑器中直接粘贴。



然后图片就可以上传到图床了。

另外，还可以看到**所有的上传在 PicGo 的相册里都能找到：**