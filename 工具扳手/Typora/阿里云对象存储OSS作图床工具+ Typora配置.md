# 阿里云对象存储OSS作图床工具+ Typora配置

---

# 一、介绍图床

**什么是Typora？** 

Typora 是一款**支持实时预览的Markdown文本编辑器**。它有 `Windows` 、`OS X` 、`Linux` 三个平台的版本，是**完全免费**的文本编辑器，它支持且仅支持Markdown语法的文本编辑。（从1.0版本开始收费，1.0以前的免费）

>传送门：https://lhxi.lanzoul.com/b017hxhef  密码:2qi3

​	

**什么是Markdown？**

> Markdown语法帮助文档传送门：https://markdown.com.cn/basic-syntax/

Markdown是一种轻量级标记语言，排版语法简洁，让人们更多地关注内容本身而非排版。它使用易读易写的纯文本格式编写文档，可与HTML混编，可导出 HTML、PDF 以及本身的 .md  格式的文件。因简洁、高效、易读、易写，Markdown被大量使用，如Github、Wikipedia、简书等。

​	

**为什么要使用图床？**

- 从编辑工作来说：由于在Typora中编辑时，插入的图片保存方式为本地路径的方式，当转移Markdown文档时，如果不将图片文件夹同步转移，图片正常都是丢在了初始的电脑上，换句话说就是**新设备无法看到Markdown文档中的图片**。此时可以借助PicGo（图床工具），所谓图床工具，就是把本地图片转存到网络云盘中，使用时通过输出URL链接获取图片。
- 从网络博客来说：假如我们搭建个人博客时，通过Markdown导入到博客中，此时的图片可能无法加载，这时我们就可以通过图床（可以将图片转成URL的云盘）中获取URL来显示图片。

---

​	

# 二、 搭建

 ## 2.1 阿里云OSS准备

首先注册/登录阿里云，选择产品下的**对象存储OSS**。

![对象存储OSS](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220526101746942.png)

可以选择折扣套餐，也可以选择立即开通。（折扣套餐是通过购买资源包使用，经常使用的推荐！40G一年9元），（立即开通是默认按流量收费，按每月使用的存储容量计费）。详情见产品文档[存储费用 (aliyun.com)](https://help.aliyun.com/document_detail/173534.html "点击查看产品文档查看费用")

![image-20220526102255772](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220526102255772.png)

以我的折扣套餐（资源包）为例，选择**标准(LRS)存储包**，地域按需求选择，国内较便宜于国外，点击购买。

![](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220526102922557.png)

到Bucket列表，点击创建Bucket。

![image-20220526103216531](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220526103216531.png)

设置一个Bucket名称，地域选择距离你最近的即可，存储类型为**标准存储**，读写权限要设置为**公共读！！**，其余默认，点击完成。

![image-20220526103447980](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220526103447980.png)

​	

**需要复制以下几个信息信息，待会配置picgo时要用。**

![image-20220526104111772](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220526104111772.png)

鼠标悬浮于头像处，点击AccessKey管理。

![image-20220526104325034](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220526104325034.png)

点击继续使用AccessKey。

![image-20220526104453005](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220526104453005.png)

点击创建AccessKey，点击Secret，复制**AccessKey ID和AccessKey Secret**。

![image-20220526104648095](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220526104648095.png)

![image-20220526104826060](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220526104826060.png)

​	

## 2.2配置PicGo-core

打开【PicGo Core (command line)】选项下的【打开配置文件】，粘贴配置文件（**要粘贴的的信息都在上面复制过**）。

![image-20220526105108515](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220526105108515.png)

```json
{
  "picBed": {
    "uploader": "aliyun",
    "aliyun": {
      "accessKeyId": "你的AccessKeyID",
      "accessKeySecret": "你的AccessKeySecret",
      "bucket": "xleixz",//换成你自己的Bucket名称
      "area": "oss-cn-nanjing",//OSS概览里的EndPoint(地域节点)，“.”前面的内容
      "path": "typora-img/",//Bucket下的文件夹，没有可以不写，默认不要文件夹
      "customUrl": "https://xleixz.oss-cn-nanjing.aliyuncs.com",//OSS概览里的Bucket域名(开头加上https://)
      "options": ""//可以不写
    }
  },
  "picgoPlugins": {
  }
}
```

​	

最后验证一下是否成功，点击链接是否能正常看到图片！！

![image-20220526110302922](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220526110302922.png)

​	

## 2.3 设置OSS防盗链

> OSS支持对存储空间（Bucket）设置防盗链，即通过对访问来源设置白名单的机制，避免OSS资源被其他人盗用。

如果不设置防盗链，自己的图片可能会被不良用户使用，扣费流量费用是图片作者的。详情见[防盗链 (aliyun.com)](https://help.aliyun.com/document_detail/31869.html?spm=5176.8466032.help.dexternal.34e914500TPUXz "点击查看产品文档防盗链")

​	

点击权限管理 - 防盗链 - 设置。（如果不添加`*.console.aliyun.com`，则自己在工作台也无法访问图片）

![image-20220526110833433](https://xleixz.oss-cn-nanjing.aliyuncs.com/typora-img/image-20220526110833433.png)

​	

本章教程就到这里，更多产品费用信息可见[API文档 (aliyun.com)](https://help.aliyun.com/document_detail/31947.html?spm=5176.8465980.help.dexternal.4e7014509mCLBO "点击查看阿里云API文档")。