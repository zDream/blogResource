title: HTTP解析
date: 3/5/2018 9:13:33 PM  
tags: java
---
一直对请求响应不太明白，做下总结，分析。原理机制先不说了，先对用法，注意事项做下梳理

## 一. HTTP 无状态性 ##

HTTP 协议是无状态的(stateless)。也就是说，同一个客户端第二次访问同一个服务器上的页面时，服务器无法知道这个客户端曾经访问过，服务器也无法分辨不同的客户端。HTTP 的无状态特性简化了服务器的设计，使服务器更容易支持大量并发的HTTP 请求。

## 二. HTTP 持久连接 ##

HTTP1.0 使用的是非持久连接，主要缺点是客户端必须为每一个待请求的对象建立并维护一个新的连接，即每请求一个文档就要有两倍RTT 的开销。因为同一个页面可能存在多个对象，所以非持久连接可能使一个页面的下载变得十分缓慢，而且这种短连接增加了网络传输的负担。HTTP1.1 使用持久连接keepalive，所谓持久连接，就是服务器在发送响应后仍然在一段时间内保持这条连接，允许在同一个连接中存在多次数据请求和响应，即在持久连接情况下，服务器在发送完响应后并不关闭TCP 连接，而客户端可以通过这个连接继续请求其他对象。

### 1. HTTP/1.1 协议的持久连接有两种方式： ###

1. 非流水线方式：客户在收到前一个响应后才能发出下一个请求;
2. 流水线方式：客户在收到 HTTP 的响应报文之前就能接着发送新的请求报文;   

## 三. 常见的HTTP请求头 ##

 - Accept-Charset		用于指定客户端接受的字符集
 - Accept-Encoding   用于指定可接受的内容编码
 - Accept-Language	用于指定一种自然语言，
 - Host 	用户指定被请求资源的Internet主机和端口号
 - User-Agent 	客户端将它的操作系统，浏览器和其他属性告诉服务器
 - Connection	当前连接是否保持

## 四. 常见的HTTP响应头 ##

 - Server 	使用的服务器名称 
 - Content-Type 	用来指明发送给接收者的实体正文的媒体类型。如Content-Type:text/html:charset=GBK
 - Content-Encoding	与请求报头Accept-Encoding对应，告诉浏览器服务端采用的是什么压缩编码
 - Content-Language 描述了资源所有的自然语言，与Accent-Language 相对应 
 - Content-Length 指明实体正文的长度，用以字节方式存储的十进制数字来表示
 - Keep-Alive	保持连接的时间

## 五. 常见的HTTP状态码 ##

 - 200 请求已成功，请求所希望的响应头或数据体将随此响应返回。
 - 302 请求的资源现在临时从不同的 URI 响应请求。由于这样的重定向是临时的，客户端应当继续向原有地址发送以后的请求。
 - 400 1、语义有误，当前请求无法被服务器理解。除非进行修改，否则客户端不应该重复提交这个请求。 　　2、请求参数有误。
 - 403 服务器已经理解请求，但是拒绝执行它
 - 404 请求失败，请求所希望得到的资源未被在服务器上发现。
 - 500 服务器遇到了一个未曾预料的状况，导致了它无法完成对请求的处理。一般来说，这个问题都会在服务器的程序码出错时出现

## 六. HTTP请求信息由3部分组成 ##

1. 请求方法（GET/POST）、URI、协议/版本
2. 请求头(Request Header)
3. 请求正文

以上图做例进行分析：

	POST http://xg.mediportal.com.cn/health/sms/verify/telephone HTTP/1.1
	
	User-Agent: DGroupPatient/1.052701.230/Dalvik/2.1.0 (Linux; U; Android 5.1.1; KIW-AL10 Build/HONORKIW-AL10)
	Content-Type: application/x-www-form-urlencoded; charset=UTF-8
	Host: xg.mediportal.com.cn
	Connection: Keep-Alive
	Accept-Encoding: gzip
	Content-Length: 33
	
	username=zhangsan&password=admin


### 1.请求方法、URI、协议/版本 ###

请求的第一行是“方法、URL、协议/版本”
根据HTTP标准，HTTP请求可以使用多种请求方法。
例如：HTTP1.1目前支持7种请求方法：GET、POST、HEAD、OPTIONS、PUT、DELETE和TARCE。

- GET 请求获取由Request-URI所标识的资源
- POST 在Request-URI所标识的资源后附加新的数据
- HEAD 请求获取由Request-URI所标识的资源的响应消息报头
- OPTIONS 请求查询服务器的性能，或查询与资源相关的选项和需求
- PUT 请求服务器存储一个资源，并用Request-URI作为其标识
- DELETE 请求服务器删除由Request-URI	所标识的资源
- TRACE 请求服务器回送收到的请求信息，主要用语测试或诊断

### 2.请求头 ###

- Content-Type 内容类型，专业术语叫“媒体类型”，即MediaType，也叫MIME类型，用来指明报文主体部分内容属于何种类型。  

> 但是content-type一般只存在于Post方法中，因为Get方法是不含“body”的，它的请求参数都会被编码到url后面，所以在Get方法中加Content-type是无用的。

常见的**MIME类型**如下：

- text/html ： HTML格式
- text/plain ：纯文本格式      
- text/xml ：  XML格式
- image/gif ：gif图片格式    
- image/jpeg ：jpg图片格式 
- image/png：png图片格式

以application开头的媒体格式类型：

-   application/xhtml+xml ：XHTML格式
-   application/xml     ： XML数据格式
-   application/atom+xml  ：Atom XML聚合格式    
-   application/json    ： JSON数据格式
-   application/pdf       ：pdf格式  
-   application/msword  ： Word文档格式
-   application/octet-stream ： 二进制流数据（如常见的文件下载）
-   application/x-www-form-urlencoded ： <form encType=””>中默认的encType，form表单数据被编码为key/value格式发送到服务器（表单默认的提交数据的格式）
  

另外一种常见的媒体格式是上传文件之时使用的：

-   multipart/form-data ： 需要在表单中进行文件上传时，就需要使用该格式

### 3. 请求正文 ###

请求头和请求正文之间是一个空行，这个行非常重要，它表示请求头已经结束，接下来的是请求正文。请求正文中可以包含客户提交的查询字符串信息：

	telephone=15527177736&userType=1&

## 七. HTTP响应格式 ##

HTTP应答与HTTP请求相似，HTTP响应也由3个部分构成，分别是：

 1. 状态行
 2. 响应头(Response Header)
 3. 响应正文

	HTTP/1.1 200 OK   //状态行  
	Server: nginx  
	Date: Tue, 31 May 2016 02:09:24 GMT  
	Content-Type: application/json;charset=UTF-8  
	Connection: keep-alive  
	Vary: Accept-Encoding  
	Access-Control-Allow-Origin: *  
	Access-Control-Allow-Headers: X-Requested-With,access_token,access-token,content-type,multipart/form-data,application/x-www-form-urlencoded
	Access-Control-Allow-Methods: GET,POST,OPTIONS  
	Content-Length: 49  
	
	{"resultCode":1,"resultMsg":"手机号未注册"}   //正文

### 1.状态行 ###

由**协议版本**、数字形式的**状态代码**、及相应的**状态描述**，各元素之间以空格分隔。

- 状态描述： 状态描述给出了关于状态代码的简短的文字描述。
- 状态代码：  状态代码由3位数字组成，表示请求是否被理解或被满足。

状态代码的第一个数字定义了响应的类别，后面两位没有具体的分类。
第一个数字有五种可能的取值：

- 1xx:   指示信息—表示请求已接收，继续处理。
- 2xx:   成功—表示请求已经被成功接收、理解、接受。
- 3xx:   重定向—要完成请求必须进行更进一步的操作。
- 4xx:   客户端错误—请求有语法错误或请求无法实现。
- 5xx: 服务器端错误—服务器未能实现合法的请求。

状态代码 状态描述    说明
- 200  OK    客户端请求成功  
- 400  Bad Request   由于客户端请求有语法错误，不能被服务器所理解。  
- 401  Unauthonzed   请求未经授权。这个状态代码必须和WWW-Authenticate报头域一起使用  
- 403   Forbidden   服务器收到请求，但是拒绝提供服务。服务器通常会在响应正文中给出不提供服务的原因  
- 404   Not Found   请求的资源不存在，例如，输入了错误的URL。  
- 500  Internal Server Error 服务器发生不可预期的错误，导致无法完成客户端的请求。  
- 503  Service Unavailable   服务器当前不能够处理客户端的请求，在一段时间之后，服务器可能会恢复正常

### 2.响应头 ###

响应头可能包括：


- Content-Type :指明发送给接收者的实体正文的媒体类型。如：Content-Type: text/html;charset=ISO-8859-1Content-Type: text/html;charset=GB2312  
- Content-Language：源所用的自然语言
- Content-Length：指明正文的长度，

