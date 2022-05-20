# Typora - Pandoc实现导入出功能

## 一、问题

问题一：在我们使用Typora时，导入Markdown文件是非常方便的，但是当我们需要导入`docx、laterx、wiki等格式的文件时，没有安装插件是无法导入的。

<img src="https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220428201156.png" alt="image-20220428201153458" style="zoom:50%;" />

问题二：在我们需要导出时，仅有三种文件可以任意导出，若需导出更多的格式文件，也需要我们安装插件。

![image-20220428202154562](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220428202156.png)

<br>

## 二、安装插件Pandoc

1. 首先[点击这里进入官网](https://github.com/jgm/pandoc/releases)，根据系统环境选择对应的版本进行下载。以我的环境为例`windows 64位`，选择`pandoc-2.18-windows-x86_64.zip`下载。这里下载的都是解压版的，都是免安装的，所以待会需要配置环境变量。

![image-20220428202804945](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220428202806.png)

<br>

2. 下载完后解压（要记住当前路径），打开系统环境变量，将路径配置到Path中。

![image-20220428203518186](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220428203519.png)

<br>

3. 配置完后打开cmd窗口，输入`pandoc --help` 出现如下信息就说明成功了！

![image-20220428203707184](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220428203708.png)

4. 重启Typora就可以导入、导出了。