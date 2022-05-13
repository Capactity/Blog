# 从输入url到页面显示的全过程



#### 1 用户输入内容，进行URL 解析（编码），根据 dns 系统进行 ip 查找。



**问题1**: url解析？

 

网络规定url只能是数字和字母及特殊字符组成，里面可能携带参数，如不解析可能会出现歧义。

url编码的规则：取决于html头部meta中charset的设定。

   一般为utf-8。但不同的操作系统、不同的浏览器、不同的网页字符集，将导致完全不同的编码结果。

   使用javascript对URL进行编码来统一格式。主要是两个方法：**encodeURI** **/** **encodeURIComponent****。**

**encodeURI**  **： 对整个url编码（除网页特殊字符），输出utf-8格式，在每个字节前加%，但不编码单引号。**

**encodeURIComponent**  **： 编码范围更广，对url的组成部分进行个别编码（包括网页特殊字符），编码方式一致。**



**问题2**: dns解析流程，并且html如何做dns优化？   

---例如查询[www.baidu.com](https://link.juejin.cn/?target=http%3A%2F%2Fwww.baidu.com "http://www.baidu.com")。

  1 浏览器输入域名，操作系统先查询hosts文件是否有记录，有则返回对应映射的ip。

  2 如没有则查询本地dns解析器（电话本）有没缓存。没有则寻找计算机配置的dns服务器上有无缓存，有则返回。

  3 如果还没就去dns根服务器查询，判断.com所属服务器，如果仍未查询到，则判断baidu.com所属服务器，直到查询到ip或返回错误信息（非转发模式）。

  总结：通过 DNS 来查询 IP 地址 • DNS 先查本地、后查运营商、逐级网上查。（域名解析是从后往前查的）

---html头部写入dns缓存地址优化。

````javascript
<meta http-equiv="x-dns-prefetch-control" content="on" />
<link rel="dns-prefetch" href="http://bdimg.share.baidu.com" />
````



#### **2 查询到ip后，发起http请求，开始http协议的三次握手。**



**问题1**: 什么是TCP/IP（http）协议。

tcp/ip传输控制协议将应用层、表示层、会话层合并为应用层，传输层、网络层、物理层和数据链路层合并为网络接口层。



**问题2**: http1.x和http2、http3的区别。

**新的二进制格式**、**多路复用（共享连接）、header压缩、服务端推送。**

**三次握手**

   第一次握手：主机A向服务器发送位码为SYN＝1的TCP包。随机产生一个确认号seq number，服务器收到SYN=1后知道了 A要求建立连接。（第一次握手，由浏览器发起，告诉服务器我要发送请求了）。

  第二次握手：主机B收到请求后，向A发送确认号 ack number =（主机A的seq+1），syn=1，seq = 随机数 的TCP包。（第二次握手，由服务器发起，告诉浏览器我准备接受了，你赶紧发送吧）。

  第三次握手：主机A收到后检查ack7是否码与SYN码正确，再发送ack number =（服务器的seq+1）,服务器收到后确认ack与seq码，建立连接。（第三次握手，由浏览器发送，告诉服务器，我马上就发了，准备接受吧）。

<img src="../img/三次握手.png" alt="三次握手"  />

**拓展： 负载均衡还有网关，cdn，服务器常见的结构，发布流程**





#### 3 建立连接后，请求html文件，如果html文件在缓存里浏览器直接返回，如果没有，就去后台拿。



 **问题1**: 解释下缓存？

浏览器首次加载，缓存资源及 请求头header里的date等属性一并缓存。

二次加载，缓存资源及 请求头header里的date等属性一并缓存。