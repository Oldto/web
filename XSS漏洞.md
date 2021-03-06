# 漏洞描述
跨站脚本攻击是指恶意攻击者往Web页面里插入恶意Script代码，当用户浏览该页之时，嵌入其中Web里面的Script代码会被执行，从而达到恶意攻击用户的目的。
 xss漏洞通常是通过php的输出函数将javascript代码输出到html页面中，通过用户本地浏览器执行的，所以xss漏洞关键就是**寻找参数未过滤的输出函数**。
 常见的输出函数有： `echo printf print print_r sprintf die var-dump var_export`.

# XSS漏洞分类

- 反射性XSS
  - **<非持久化>** 攻击者事先制作好攻击链接, 需要欺骗用户自己去点击链接才能触发XSS代码（服务器中没有这样的页面和内容），一般容易出现在搜索页面。
- 存储型XSS
  - **<持久化>** 代码是存储在服务器中的，如在个人信息或发表文章等地方，加入代码，如果没有过滤或过滤不严，那么这些代码将储存到服务器中，每当有用户访问该页面的时候都会触发代码执行，这种XSS非常危险，容易造成蠕虫，大量盗窃cookie（虽然还有种DOM型XSS，但是也还是包括在存储型XSS内）。
- Dom型XSS
  - 基于文档对象模型（Document Objeet Model，DOM)的一种漏洞。DOM是一个与平台、编程语言无关的接口，它允许程序或脚本动态地访问和更新文档内容、结构和样式，处理后的结果能够成为显示页面的一部分。DOM中有很多对象，其中一些是用户可以操纵的，如url ，location，refelter等。客户端的脚本程序可以通过DOM动态地检查和修改页面内容，它不依赖于提交数据到服务器端，而从客户端获得DOM中的数据在本地执行，如果DOM中的数据没有经过严格确认，就会产生DOM XSS漏洞。
- Self-XSS

# XSS的最根本型的原理

​	攻击者传入恶意的JavaScript代码，在用户端的浏览器执行，最终对用户造成危害。

# XSS存在于哪些地方？

任何只要能让你输入数据保存的地方都有可能存在XSS漏洞。

# JavaScript代码中src的用法，代码如下

`<script src=//www.xxx.com/abcd></script>`

src是引用的意思，类似于include。

如果是公网的链接可以使用//,如果不是公网是本地，//前需要加上http或HTTPs，这样才能正常加载，否为会将其视为文档。

`<script src=http://www.xx.com/q23></script>`
# 如何测试是否存在

- <script>alert(1)</script>适合反射型XSS可以用，存储型容易导致网站出现问题

- <script>console.log(1)</script>>

- <img src=1>通过插入图片链接，将代码插入，做了HTML实体化转义可以通过此方法绕过。
# XSS防御手段

- HTML实体化转义--&lt;img src=1&gt;
- URL编码--%3Cimg%20src=1%3E
- HttpOnly防护，禁止JavaScript操作cookie

如果是对<>做了过滤，基本上是没戏了，内容做过滤还有绕过的可能性。

# 重点：

当存在XSS漏洞的网站如果采用的是https协议，那么XSS接收站点也得必须为https协议。如果存在XSS漏洞的网站采用的http协议，接收站点是https协议也可以接收到cookie信息。

# XSS如何绕过HttpOnly？
**phpinfo绕过**

phpinfo中会泄露cookie信息，查找PHP Variables，首先我们要先打开登录首页，这样phpinfo才能获取到cookie信息，否则获取不到,此种方法只能用一次

**401钓鱼**

在XSS存在的地方插入代码，伪造一个弹框登录

**EXE钓鱼**

就是对方访问链接是会跳转到其他链接，需要你下载程序后才能使用，这种方法可以直接控制对方主机。
