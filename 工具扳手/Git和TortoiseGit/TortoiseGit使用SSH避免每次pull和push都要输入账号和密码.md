 # TortoiseGit使用SSH避免每次pull和push都要输入账号和密码

## 一、理解SSH

**SSH 工作原理**

`SSH` 全称(`Secure Shell`),是一项创建在**应用层和传输层基础上的安全协议**，为计算机上的Shell（壳层）提供安全的传输和使用环境。

相对`telnet`来讲，`ssh`主要被发明用来替代`telnet`，完善了其安全性问题。`Secure Shell`（缩写为SSH），由`IETF`的网络工作小组（`Network Working Group`）所制定。

传统的网络服务程序，如`rsh`、`FTP`、`POP`和`Telnet`其本质上都是不安全的；因为它们在网络上用**明文**传送数据、用户帐号和用户口令，很容易受到中间人（`man-in-the-middle`）攻击方式的攻击。就是存在另一个人或者一台机器冒充真正的服务器接收用户传给服务器的数据，然后再冒充用户把数据传给真正的服务器。

而`SSH`是目前较可靠，专为远程登录会话和其他网络服务提供安全性的协议。利用`SSH`协议可以有效防止远程管理过程中的信息泄露问题。通过`SSH`可以对所有传输的数据进行加密，也能够防止`DNS`欺骗和`IP`欺骗。



## 二、解决

1. 使用Putty工具解决每次pull和push都需要输入账号密码的问题

<img src="https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425145538" alt="img" style="zoom:50%;" />



2. 点击Generate，要来回在框内来回滑动鼠标

<img src="https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425145647" alt="img" style="zoom:50%;" />

 

3. 全选复制完之后，点击save Private key 按钮，保存.ppk文件，名字自取

![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425145728)



4. 将刚刚复制的公钥粘贴到Gitee---->头像---->设置---->SSH公钥处，点击确定

<img src="https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425145748" alt="img" style="zoom:50%;" />



5. 打开Pageant

<img src="https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425145828" alt="img" style="zoom:50%;" />



6. 点击Add Key ，找到刚刚保存的.ppl文件，添加进来

![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425145847)



7. 到仓库复制SSH密钥后，克隆即可，**记得指明加载putty密钥位置**

<img src="https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425150039" alt="img" style="zoom: 50%;" />

