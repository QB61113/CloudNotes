# Typora图片自动上传 —— 图床工具PicGo-Core篇 

[TOC]

---

> 解决Typora编写Markdown文档，转存时图片不显示问题。

## 一、Typora 

Typora 是一款**支持实时预览的Markdown文本编辑器**。它有 `Windows` 、`OS X` 、`Linux` 三个平台的版本，是**完全免费**的文本编辑器，它支持且仅支持Markdown语法的文本编辑。（从1.0版本开始收费，1.0以前的免费）

>传送门：https://lhxi.lanzoul.com/b017hxhef  密码:2qi3

<br>

## 二、Markdown

> Markdown语法帮助文档传送门：https://markdown.com.cn/basic-syntax/

Markdown是一种轻量级标记语言，排版语法简洁，让人们更多地关注内容本身而非排版。它使用易读易写的纯文本格式编写文档，可与HTML混编，可导出 HTML、PDF 以及本身的 .md  格式的文件。因简洁、高效、易读、易写，Markdown被大量使用，如Github、Wikipedia、简书等。

<br>

## 三、为什么Typora需要借助PicGo使用？

由于在Typora中编辑时，插入的图片保存方式为本地路径的方式，当转发md文档时，图片正常都是”丢在了初始的电脑上“，换句话说就是**新设备无法看到md文档中的图片**。此时可以借助PicGo（图床工具），所谓图床工具，就是自动把本地图片转换成链接的一款工具，PicGo 就是一款比较优秀的图床工具。

<br>

## 四、开始正题

1. **准备一个Gitee仓库**

2. [x] 准备一个**开源**Gitee仓库，专门用来存放上传的图片

   ![开源仓库](https://gitee.com/xleixz/cloud-notes/raw/master/Typora-Images/20220423130352.png)

​	

- [x] 右上角个人头像---->设置---->私人令牌，勾选projects，提交。

![私人令牌](https://gitee.com/xleixz/cloud-notes/raw/master/Typora-Images/20220423130449.png)

- [x] 保存好私人命令，复制，待会要用。

![image-20220423120930318](https://gitee.com/xleixz/cloud-notes/raw/master/Typora-Images/20220423130523.png)

<br>

2. **设置Typora**

>  打开Typora，点击文件---->偏好设置---->图像。

- [x] *插入图片时*，选择**上传图片**，对本地和网络位置的图片应用上述规则（根据自己偏好和需求设置）；
- [x] *上传服务设定*，选择**PicGo-Core(command line)**，通过插件上传服务。
- [x] 点击**下载或更新**，确保下载完成。

![偏好设置](https://gitee.com/xleixz/cloud-notes/raw/master/Typora-Images/20220423112453.png)

<br>

3. **安装Node.js**

> Node.js 不是一门新的编程语言，也不是一个 JavaScript 框架，它是一套 JavaScript 运行环境，用来支持 JavaScript 代码的执行。用编程术语来讲，Node.js 是一个 JavaScript 运行时（Runtime）。
>
> 所谓运行时，就是程序在运行期间需要依赖的一系列组件或者工具；把这些工具和组件打包在一起提供给程序员，程序员就能运行自己编写的代码了。

Node.js安装传送门：https://nodejs.org/en/

- [x] 安装完成后，通过cmd命令行`node -v`查看。

![image-20220423113836183](https://gitee.com/xleixz/cloud-notes/raw/master/Typora-Images/20220423113837.png)

<br>

4. **安装插件**

- [x] 待步骤一下载完成后，找到`C:\Users\这里是你的用户名\AppData\Roaming\Typora\picgo\win64`目录下的picgo.exe，按住**shift同时右击选择在终端打开**。

![image-20220423114914518](https://gitee.com/xleixz/cloud-notes/raw/master/Typora-Images/20220423114916.png)



<br>

**能找到上面路径的这里可以跳过不看！！！**，如果找不到这个路径，可以通过以下两种方式：
方式一：打开文件资源管理器（此电脑），点击查看，显示已隐藏的项目。

![方式一](https://gitee.com/xleixz/cloud-notes/raw/master/Typora-Images/20220423115517.png)

方式二：通过Everything工具，进行搜索。

> Everything工具传送门：https://www.voidtools.com/zh-cn/

​	

- [x] 分别输入以下安装插件的命令行：

```
picgo install super-prefix
```

```
picgo install gitee-uploader
```

​	

- [x] 等待安装成功。

![安装插件命令行](https://gitee.com/xleixz/cloud-notes/raw/master/Typora-Images/20220423115034.png)

<br>

5. **编辑Typora配置文件**

> 打开Typora，点击文件---->偏好设置---->图像---->打开配置文件

![image-20220423130848575](https://gitee.com/xleixz/cloud-notes/raw/master/Typora-Images/20220423130850.png)

​	

- [x] 编写配置文件。

**需要修改的配置：** 

`repo`为仓库名，格式为：`"用户名/仓库名"`；

`token`为私人令牌；

`path` 为仓库下的文件夹，若没有可以不写；

`customUrl`为域名，没有可以不写；

`branch`表示分支，此处默认`master`就OK。

**以下是我的配置文件和仓库结构，可以参考下图我的仓库结构进行配置：**

我的配置文件：

```json
{
  "picBed": {
    "uploader": "gitee",
    "gitee": {
      "repo": "xleixz/CloudNotes-Images",
      "token": "",
      "path": "/Typora-Images",
      "customUrl": "",
      "branch": "master"
    }
  },
  "picgoPlugins": {
    "picgo-plugin-gitee-uploader": true,
    "picgo-plugin-super-prefix": true
  },
  "picgo-plugin-super-prefix": {
    "fileFormat": "YYYYMMDDHHmmss"
  },
  "picgo-plugin-gitee-uploader": {
    "lastSync": "2022-04-23 01:08:51"
  }
}
```

<br>

我的仓库结构：

![image-20220423132557769](https://gitee.com/xleixz/cloud-notes/raw/master/Typora-Images/20220423132559.png)

<br>

6. **测试**

> 打开Typora，点击文件---->偏好设置---->图像---->验证图片上传选项

![image-20220423133014817](https://gitee.com/xleixz/cloud-notes/raw/master/Typora-Images/20220423133016.png)

​	

- [x] 等待验证，验证成功后，**一定要点击下图绿色框框的URL进去看看图片是否能打开**。

![image-20220423133112375](https://gitee.com/xleixz/cloud-notes/raw/master/Typora-Images/20220423133113.png)

​	

- [x] **若成功打开显示图片并且仓库中有图片**，恭喜你成功了，可以开始使用了（**下面就不用看了**）。

![image-20220423133119798](https://gitee.com/xleixz/cloud-notes/raw/master/Typora-Images/20220423133121.png)

![image-20220423134444456](https://gitee.com/xleixz/cloud-notes/raw/master/Typora-Images/20220423134445.png)

​	

- [x] **若打开是验证成功但是出现404页面**，并且编辑器图片显示`Image load failed` ，则说明**配置文件**有问题。

![image-20220423133528771](https://gitee.com/xleixz/cloud-notes/raw/master/Typora-Images/20220423133530.png)

​	

图片显示**Image load failed**

![image-20220423133920624](https://gitee.com/xleixz/cloud-notes/raw/master/Typora-Images/20220423133922.png)

