---
title: http协议
date: 2019-06-06 22:35:45
tags:
---

### http 是超文本协议
> HTTP(HyperText Transfer Protocol)是一套计算机通过网络进行通信的规则 - 从www浏览器传输到web服务器到一种传输协议 - 从客户机到服务器到请求和从服务器到客户机的响应进行约束和规范
- 无状态 指web浏览器和web服务器不需要建立持久连接，不保留信息（如果记录用户状态，需要用到session[由cookie->加密cookie->session->服务器存session,过程中还可能会被篡改]），客户端向服务器发出请求，服务器返回响应，连接就被关闭，服务器不保留连接相关信息
- 遵循请求/应答模型 web浏览器向web服务器发送请求，web服务器处理请求并返回应答。所有http连接都被构造成一套请求和应答
- 内容类型 服务器返回的文件有类型 （json, html，二进制文件），所有类型在MIME Internet邮件协议有模型化
### 浏览器行为与http协议 -- 图解
![Image test](/static/浏览器行为与http协议.png)
## 基础
### 五层架构
> OSI: Open System Interconnect 开发式系统互联
  - osi七层模型
    - 应用层： 文件传输、电子邮件、文件服务，虚拟终端 TFTP，HTTP，SNMP，FTP，SMTP，DNS，Telnet 【操作系统或网络应用程序提供访问网络服务的接口】
    - 表示层： 数据格式化，代码转换，数据加密 没有协议【即提供格式化的表示和转换数据服务。数据的压缩和解压缩， 加密和解密等工作都由表示层负责】
    - 会话层： 解除或建立与别的接点的联系 没有协议【报文，提供包括访问验证和会话管理在内的建立和维护应用之间通信的机制 ，如服务器验证用户登录】
    - 传输层： 提供端对端的接口 TCP，UDP 【TCP的数据单元称为段 （segments）而UDP协议的数据单元称为“数据报（datagrams）”，负责获取全部信息】
    - 网络层： 为数据包选择路由 IP，ICMP，RIP，OSPF，BGP，IGMP 
    - 数据链路层： 传输有地址的帧以及错误检测功能 SLIP，CSLIP，PPP，ARP，RARP，MTU 【数据的单位称为帧（frame）】
    - 物理层： 以二进制数据形式在物理媒体上传输数据
  - TCP/IP 五层网络架构
    - 应用层 
    - 传输层 
    - 网络层 
    - 数据链路层 
    - 物理层 

### TCP/IP协议簇
> 协议簇包括很多协议，因为TCP、ip重要，以此命名
> TCP/IP协议集包括应用层,传输层，网络层，网络访问层
> 简单包括：
 - 应用层
  1、超文本传输协议（HTTP）:万维网的基本协议；
  2、文件传输（TFTP简单文件传输协议）；
  3、远程登录（Telnet），提供远程访问其它主机功能, 它允许用户登录internet主机，并在这台主机上执行命令；
  4、网络管理（SNMP简单网络管理协议），该协议提供了监控网络设备的方法， 以及配置管理,统计信息收集,性能管理及安全管理等；
  5、域名系统（DNS），该系统用于在internet中将域名及其公共广播的网络节点转换成IP地址。
  - 网络层
  1、Internet协议（IP）；
  2、Internet控制信息协议（ICMP）；
  3、地址解析协议（ARP）；
  4、反向地址解析协议（RARP）。
  - 网络访问层
  IP地址与物理地址硬件的映射， 以及将IP封装成帧.基于不同硬件类型的网络接口，网络访问层定义了和物理介质的连接

### TCP
> TCP（Transmission Control Protocol，传输控制协议）是面向连接的协议.在收发数据前，必须和对方建立可靠的连接
> 三次握手
> 四次挥手
> 名词解释
1、ACK 是TCP报头的控制位之一，对数据进行确认。确认由目的端发出， 用它来告诉发送端这个序列号之前的数据段都收到了。 比如确认号为X，则表示前X-1个数据段都收到了，只有当ACK=1时,确认号才有效，当ACK=0时，确认号无效，这时会要求重传数据，保证数据的完整性。

2、SYN 同步序列号，TCP建立连接时。

3、FIN 发送端完成发送任务位，当TCP完成数据传输需要断开时,提出断开连接的一方。
> 优点
> 缺点
> 使用场景：
### UDP
> UDP是一个非连接的协议，传输数据之前源端和终端不建立连接
> 优点
> 缺点
> 使用场景
> ping（测试两台主机之间TCP/IP通信是否正常）的原理： 向对方主机发送UDP数据包，然后对方主机确认收到数据包， 如果数据包是否到达的消息及时反馈回来，那么网络就是通的
### UDP和TCP有什么区别
1、基于连接与无连接；

2、对系统资源的要求（TCP较多，UDP少）；

3、UDP程序结构较简单；

4、流模式与数据报模式 ；

5、TCP保证数据正确性，UDP可能丢包；

6、TCP保证数据顺序，UDP不保证。
### http通信机制： 
1. 建立TCP连接
- HTTP工作之前，web浏览器先要通过网络和web服务器建立连接，这次连接是通过TCP完成，TCP协议和IP协议共同创建Internet===TCP/IP协议簇。HTTP是比TCP更高层次的应用层协议，根据规则，只有底层协议建立后，才能进行更高层协议的连接。
2. web浏览器向web服务器发送请求命令
- 建立了TCP连接，web浏览器就会向Web服务器发送请求命令 eg: GET /xx HTTP1.1
3. web浏览器发送请求头信息
- 发送请求命令后，头信息会发送报文，主体等信息，之后浏览器发送一段空白行通知服务器，结束头信息的发送
4. web服务器应答
- 客户机向服务器发出请求后，服务器会向客户机回送应答 HTTP/1.1 200OK
5. web服务器发送应答头信息
- 服务器发送响应头信息
6. web服务器向浏览器发送数据
- 空白借宿，以content-type应答头信息所描述格式发送用户请求的实际数据
7. web服务器关闭tcp连接
- 一般服务器向浏览器发送数据，就会关闭TCP连接
- connection:keep-alive ==> TCP在发送后仍然打开状态，浏览器可以继续通过相同的连接发送请求，保持连接可以节省每个请求建立新连接的时间，节约网络带宽 - 浏览器对同时发送http连接有限制，一般是6个（以前）

### http请求/应答格式
> http头文件
> general部分
- Request URL: 请求路径；分析用户来源很有用
- Request Method
- Status Code
- Referrer Policy ： 规定如何发送Referrer
   - 防止： url包括用户敏感信息，第三方拿到不安全
   - 可以：针对第三方网站隐藏referrer,只发送url和host部分；策略是有权不告诉对象请求来源，但是不能欺骗
   - 5中策略：
    - no referrer: 任何情况都不发送referrer信息
    - no-referrer-when-downgrade: 协议降级不发送referrer信息
    - origin-only: 只包含协议 + host
    - origin-when-cross-origin: 仅在跨域访问时发送只包含host的referrer，同域是完整的
    - unsafe url: 无论协议降级、本站、站外链接，都发送，最宽松不安全策略
- 怎么用
  - CSP（Content Security Policy），是一个跟页面内容安全有关的规范。 头部设置 eg:Origin When Cross-origin 策略
  - <meta> 标签也可以指定 Referrer 策略 
  ```js
   <meta name="referrer" content="no-referrer|no-referrer-when-downgrade|origin|origin-when-crossorigin|unsafe-url">
   // 如果 content 属性不是合法的取值，浏览器会自动选择 no-referrer 这种最严格的策略。
  ```
  - <a> 标签的 referrer 属性
  ```js
  <a href="http://example.com" referrer="no-referrer|origin|unsafe-url">xxx</a>
  ```
> Response Headers部分
- connection: close / keep-alive; 是否需要持久连接
- content-length: 内容长度，只有浏览器使用keep-alive才需要，如果你想要利用持久连接的优势，可以把输出文档写入ByteArrayOutputStram，完成后查看其大小，然后把该值放入Content-Length头，最后通过 byteArrayStream.writeTo(response.getOutputStream()发送内容。
- content-type: 指定请求时发送内容（实体介质）的类型
- date: 消息发送时间，描述的是世界标准时
- keep-alive: timeout(过期时间-http.conf是keepAliveTimeout)
- server: 处理请求的原始服务器的软件信息
> Request Headers部分
- accept: 用于指定客户端接受哪些类型的信息
- accept-encoding: 指定浏览器可以支持web服务器返回内容压缩编码类型
- accept-language: 指定客户端可以接受的语言
- cache-control： 指定请求和响应遵循的缓存机制
  - 请求缓存指令
   - no-cache
   - no-store
   - max-age
   - max-stale
   - min-fresh
   - only-if-cached
  - 响应缓存指令
    - public
    - private
    - no-cache
    - no-store
    - no-transform
    - must-revalidate
    - proxy-revalidate
    - max-age
- connection 是否持久连接
- host: 指定请求资源的internet主机和端口号，必须表示请求url的原始服务器或网关的位置。HTTP/1.1请求必须包含主机头域，否则系统会以400状态码返回。origin
- pragma：头域包含实现特定的指令
- referrer：允许客户端请求uri的源资源地址。可以允许服务器生成回退链表，可用来登陆、优化cache等。他也允许废除的或错误的连接由于维护的目的被 追踪。如果请求的uri没有自己的uri地址，Referer不能被发送。如果指定的是部分uri地址，则此地址应该是一个相对地址。
- user-agent： 头域包含发出请求的用户信息
### url 和 uri
> 遵循：scheme:[//authority][/path][?query][#fragment]
scheme − 对于 URL, 是访问资源的协议名称；对其他URI,是分配标识符的规范的名称
authority − 可选的组成用户授权信息部分，主机及端口（可选）
path − 用于在scheme和authority内标识资源
query − 与路径一起的附加数据用于标识资源。对于url是查询字符串
fragment − 资源特定部分的可选标识符
- world wide web结合了三种技术： 数据格式、协议、两者联系在一起的标识符
- URL和URN是URI的子集，URI属于URL更高层次的抽象
- URI: Uniform Resource Identifier，统一资源标识符 类似文件路径名 定位器、名称
  - 用来唯一的标识一个资源
  - Web上可用的每种资源如HTML文档、图像、视频片段、程序等都是一个来URI来定位的
组成：
①访问资源的命名机制
②存放资源的主机名
③资源自身的名称，由路径表示，着重强调于资源。 
- 作用：
在HTML中，URi被用来：
1.链接到另一个文档或资源
2.链接到一个外部样式表或脚本
3.在页内包含图形、对象或applet
4.简历图像映射
5.提交一个表单
6.建立一个框架文档
7.引用一个外部参考
8.指向一个描述文档的metadata
- URL: uniform resource locators统一资源定位符，经常指定为非正式的网址，就是那些提供了访问机制的.
URL是Internet上用来描述信息资源的字符串，主要用在各种WWW客户程序和服务器程序上，特别是著名的Mosaic。
采用URL可以用一种统一的格式来描述各种信息资源，包括文件、服务器的地址和目录等。
- 每个URL都必须从以下scheme开始:ftp、http、https、gopher、mailto、news、nntp、telnet、wais、file或prospero。如果不是以此开头，则不是URL
组成：
①协议(或称为服务方式)
②存有该资源的主机IP地址(有时也包括端口号)
③主机资源的具体地址。如目录和文件名等
- 类比：URI的作用像身份证号一样，URL的作用像家庭住址
- URN：uniform resource name统一资源命名，是通过名字来标识资源
  - 不依赖于位置，并且有可能减少失效连接的个数
---> 为了在xml名称空间、xml模式、Extensible Stylesheet Language Transformations (XSLT) 用上url,产生了URI和URN<---
### 请求头/响应头具体
- https://blog.csdn.net/u010256388/article/details/68491509
![请求头](/static/http-request-head.jpg)
![请求头](/static/request-head.jpg)
![响应头](/static/http-response-head.jpg)
> 报文
### 状态码
HTTP应答码也称为状态码，它反映了Web服务器处理HTTP请求状态。HTTP应答码由3位数字构成，其中首位数字定义了应答码的类型：

1XX
信息类(Information),表示收到Web浏览器请求，正在进一步的处理中
2XX
成功类（Successful）,表示用户请求被正确接收，理解和处理例如：200 OK
3XX
重定向类(Redirection),表示请求没有成功，客户必须采取进一步的动作。
4XX
客户端错误(Client Error)，表示客户端提交的请求有错误，例如：404 NOT Found，意味着请求中所引用的文档不存在。
5XX-服务器错误(Server Error)表示服务器不能完成对请求的处理：如500对于我们Web开发人员来说掌握HTTP应答码有助于提高Web应用程序调试的效率和准确性。
## 相关应用
### 用户鉴权方式-解决前端安全
### cookies vs session
### 缓存机制

### http与反向代理
### 负载均衡

### nginx
## 版本比较
### http1 vs http1.1 vs http2.0 vs http3

### 问题
> 为什么TCP协议是三次握手

链接：
  - http相关:http://jartto.wang/2016/08/04/Rudimentary-http-protocol/
  - 五层协议：https://blog.csdn.net/huangjin0507/article/details/51613561
  - TCP/UDP: https://zhuanlan.zhihu.com/p/24860273
  - uri/url：https://www.ibm.com/developerworks/cn/xml/x-urlni.html
  - 请求和响应头部信息连接： https://blog.csdn.net/u010256388/article/details/68491509