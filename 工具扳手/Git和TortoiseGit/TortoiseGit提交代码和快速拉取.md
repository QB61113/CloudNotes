# TortoiseGit提交代码和快速拉取

## 一、TortoiseGit提交代码

1. 下载Git两个软件，按下图顺序下载

> Git传送门：[Git - Downloads](https://git-scm.com/downloads)
>
> TortoiseGit传送门：[TortoiseGit - Download](https://tortoisegit.org/download/)

![image-20220425142144206](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425142145.png)



2. 准备一个仓库（私有和开源皆可）

<img src="https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425142100.png" alt="img" style="zoom:33%;" />



3. 点击“克隆/下载”，复制HTTPS

![点击并拖拽以移动](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425142329.png)



4. 到本地任意位置，右击“Git Cone”，将仓库克隆到本地

<img src="https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425142513.png" alt="image-20220425142511598" style="zoom:33%;" />



5. 输入URL（会自动粘贴，没有就手输），点击OK

<img src="https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425143200.png" alt="img" style="zoom: 50%;" />



6. 输入gitee用户名和账户密码 

<img src="https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425143027.png" alt="img" style="zoom: 50%;" />



> 用户名在主页位置

<img src="https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425142811.png" alt="image-20220425142809482" style="zoom: 50%;" />



7. 输完账号密码，验证成功后，点击close，完成克隆

<img src="https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425143022.png" alt="img" style="zoom: 50%;" />



## 二、提交代码

1. 右击---->Git提交（Git Create ……）

![提交](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425143340.png)



2. 填写日志，变更列表选择**全部**，选择**提交并推送**

<img src="https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425144103.png" alt="image-20220425144102100" style="zoom:50%;" />



3. 填写完账号和密码之后，出现success及提交成功

<img src="https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425144438.png" alt="img" style="zoom: 50%;" />



4. 进入个人仓库刷新即可

![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425144322.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)



## 三、快速拉取（更新）到本地仓库

1. 本地，右击TortoiseGit----->拉取（Pull）

<img src="https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425144634.png" alt="image-20220425144633513" style="zoom:50%;" />



2. 默认OK，显示success后，本地会自动更新，与云端同步



## 四、报错分析

### 4.1 ⚠️提交时邮件地址报错解决方法

> 将“不公开我的地址”取消勾选

![img](https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425144811.png)



### 4.2 ⚠️git绿色、红色图标不显示的问题

>    ✅绿色图标是指提交成功的
>
> ​    ❌红色图标是指修改后还未提交的

虽然没有图标，但是不影响文件的上传和下载，却非常影响我们直观的地判断哪些文件是否经过修改。



**解决**

1. 打开注册表 ---->` win+r,regedit.exe `

2. 找到`HKEY_LOCAL_MACHINE\Software\Microsoft\windows\CurrentVersion\Explorer`

3. 修改键名 `Max Cached Icons` (最大缓存图标) 的值为 `2000` （没有这个键，可以新建）

4. 重启电脑

5. 打开后找到`HKEY_LOCAL_MACHINE–>SOFTWARE–>Microsoft–>Windows–>CurrentVersion–>Explorer–>ShellIconOverlayIdentifiers`这一项

6. 将Tortoise相关的项都提到靠前的位置（重命名，在名称之前加几个空格）（Windows会使用掉4项默认排序，另外还有11项是供应用程序配置的，如果排在11项之后，可能导致应用程序的配置无效），排到靠前位置后

7. 重启资源管理器即可（任务管理器-->资源管理器（重新启动））

<img src="https://gitee.com/xleixz/CloudNotes-Images/raw/master/Typora-Images/20220425145057.png" alt="image-20220425145056153" style="zoom:50%;" />



## 五、TortoiseGit汉化包

> TortoiseGit汉化包传送门：https://lhxi.lanzoul.com/iKPtn024ei4d  密码:3mvq

