# AJAX

AJAX是一种无需重新加载整个页面的情况下，能够更新部分网页的技术。
AJAX**不是一种新的编程语言，而是一种用于创建更好更快以及交互性更强的Web应用程序的技术。**
增强B/S的体验性。（浏览器/服务器）
**B/S：未来的主流，并且会爆发式的持续增长**

产品链：H5 + 网页 + 客户端 + 手机端（Android、iOS） + 小程序

**利用AJAX可以做：**
注册时，用户输入用户名，自动检测用户名是否存在。
登录时，提示用户名密码错误。
删除数据行时，将行ID发送到后台，后台在数据库中删除，数据库删除成功后，在页面DOM中将数据行也删除。
我们可以使用前端的一个标签来伪造一个Ajax的样子，iframe标签。

---

**使用jQuery需要先导入JQuery的js文件。**
传送门：[jquery-3.6.0.js](https://www.yuque.com/attachments/yuque/0/2022/js/2560440/1649302283328-61f5e1d5-f9b4-48a2-ba41-06761928328a.js?_lake_card=%7B%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2022%2Fjs%2F2560440%2F1649302283328-61f5e1d5-f9b4-48a2-ba41-06761928328a.js%22%2C%22name%22%3A%22jquery-3.6.0.js%22%2C%22size%22%3A288580%2C%22type%22%3A%22text%2Fjavascript%22%2C%22ext%22%3A%22js%22%2C%22status%22%3A%22done%22%2C%22taskId%22%3A%22u63a26e16-15bf-4f96-a464-c63349fb12d%22%2C%22taskType%22%3A%22upload%22%2C%22id%22%3A%22cAs50%22%2C%22card%22%3A%22file%22%7D)

JQuery不是生产者，只是一个搬运工。

**AJAX总结**
使用JQuery需要导入JQuery，使用Vue导入Vue，两个都用，自己使用原生态实现

**三部曲**

1. 编写对应处理的COntroller，返回消息或者字符串或者json格式的数据；
1. 编写AJAX请求
   1. url：Controller请求
   1. data：键值对
   1. success：回调函数
3. 给AJAX绑定事件，点击click，失去焦点onblur，键盘弹起keyup
