title: 深入理解java I/O的工作机制
date: 3/20/2018 7:41:59 PM 
tags: java
---

## 1. 基本架构 ##
java 的io操作类在java.io包下,大概有将近80个类，可大概分为如下四组：

 - 基于字节操作的io接口：InputStream 和OutputStream
 - 基于字符操作的io接口：Writer和Reader
 - 基于磁盘的io接口：File
 - 基于网络操作的io接口：Socket

前两组主要是传输数据的**数据格式**，后两组主要是传输数据的**方式**，

### 1.1 基于字节的操作接口 ###

基于字节的输入的输出分别是InputStream和OutputStream

InputStream子类

	AudioInputStream ， ByteArrayInputStream ， FileInputStream ，  
	FilterInputStream ， InputStream ， ObjectInputStream ，   
	PipedInputStream ， SequenceInputStream ， StringBufferInputStream

OutputStream子类
 
	ByteArrayOutputStream ， FileOutputStream ， FilterOutputStream ，
	ObjectOutputStream ， OutputStream ， PipedOutputStream 

### 1.2 基于字符的操作接口 ###

不管是磁盘还是网络传输，最小的存储单元都是字节，而不是字符。

写类Writer,读Reader

Writer

	BufferedWriter ， CharArrayWriter ， FilterWriter ， OutputStreamWriter ，
	PipedWriter ， PrintWriter ， StringWriter 

Reader

	BufferedReader ， CharArrayReader ， FilterReader ， InputStreamReader ，
	PipedReader ， StringReader 

### 1.3 字节和字符的转化接口 ###

OutputStreamWriter：是Writer的子类。将输出的字符流变成字节流：即将字符流的输入对象变成字节流输入对象。

InputStreamReader:是Reader的子类。将输入的字节流变成字符流，即将一个字节流的输入对象变成字符流输入对象。

## 2. 磁盘io工作机制 ##

### 2.1 标准访问文件的方式 ###

读文件方式：当调用read()接口时，先检查缓存，如果存在，则从缓存中返回。如果没有，则从磁盘中读取，然后缓存在操作系统的缓存中。

写入的方式：用户调用writer()将数据复制到内核地址的缓存中，这时对用户来说已经完成，至于什么 时候再写到磁盘  中由操作系统决定，

### 2.2 直接io的方式 ###
所谓直接io的方式就是应用程序直接访问磁盘  数据，而不经过操作系统内核数据缓冲。这样做的目的就是减少一次从内核缓冲区到用户程序缓存的数据复制。

但这样也有负面影响，如果访问的数据不在应用程序缓存中，那么每次数据都会直接从磁盘  进行加载，这种直接加载会非常缓慢。通常直接io与异步io结合使用，会得到比较好的性能。

### 2.3 同步访问文件的方式 ###

数据的读取和写入都是同步操作的，它与标准访问文件的方式不同的是，只有当数据被成功写到磁盘  时才返回给应用程序成功的标志。

这种访问文件的方式性能都比较差，只有在一些对数据安全性要求比较高的场景中才会使用，而且通常这些操作方式的硬件都是定制的。

### 2.4 异步访问文件的方式 ###

异步访问文件的方式就是当访问数据的线程发出请求之后，线程会接着去处理其他事情，而不是阻塞等待。这种可以提高应用程序的效率，但不会改变访问文件的效率。

### 2.5 内存映射的方式 ###

指操作系统将内存中的某一块区域与磁盘中的文件关联起来，当要访问内存中的一段数据时，转换为访问文件的某一段数据。目的同样是减少数据从内核空间缓存到用户空间缓存的数据复制操作，因为这两个空间的数据是 共享的。

### 2.6 java序列化技术 ###
java序列化就是将一个对象转化成一串二进制表示的字节数组，通过保存或者转移这些字节数据来达到持久化的目的。持久化，对象必须继承java.io.Serializable接口，反序列化则是相反的过程，将这个字节数组再重新构造成对象。

序列化情况总结：

 - 当父类继承Serializable接口，所有子类都可以被序列化。
 - 子类实现了Serializable接口，父类没有，父类中的属性不能序列化(不报错，数据会丢失)，但是在子类中属性仍能正确序列化。
 - 如果序列化的属性是对象，则这个对象也必须实现Serializable接口，否则会报错
 - 在反序列化时，如果对象的属性有修改或删减，则修改的部分属性会丢失，但不会报错。
 - 在反序列化时，如果SerialVersionUID被修改，则反序列化时会失败。

## 3. 网络io工作机制 ##

### 3.1 TCP状态转化 ###
暂无详细了解

### 3.2 影响网络传的因素 ###

 - 网络带宽： 所谓带宽就是一条物理链路在1s内能够传输的最大比特数。注意这里比特（bit）而不是字节数，也就是 b/s .
 - 传输距离： 也就是数据在光纤中要走的距离，虽然光的转播速度很快，但也有时间的，由于数据在光纤中的移动并不是走直线的，会有一个折射率，大概是光的2/3，这个就是我们说的传输延时。
 - TPC拥塞控制： TCP传输是一个 停-等-停-等 的协议。传输方的接受方的步调要一致，要达到步调一致就要通过拥塞控制来调节。

### 3.3 java Socket 的工作机制 ###

Socket没有对应到一个具体的实体，它描述计算机之间完成相互通信的一种抽象功能。Socket连接必须由底层 Tcp/IP 来建立连接，建立Tcp连接需要底层IP来寻址网络中的主机。

### 3.4 建立通信链路 ###

客户端socket实例创建过程：客户端创建一个socket实例，给实例分配没有使用过的端口号，并创建一个包含本地地址，远程地址和端口号的套接字数据结构，这个数据结构一直保存在系统中直到这个连接关闭，在创建socket构造函数正确返回之前，要进行tcp的3次握手协议，协议完成后socket实例对象将创建完成，否则交付抛出IOException错误。

服务器：服务器将创建一个ServerSocket实例，只要端口号没被占用，一般都会创建成功，同时操作系统将创建一个底层数据结构，这个数据结构包含指定监听的端口号和包含监听地址的通配符。之后调用acccept方法时，将进入阻塞状态，等待客户端的请求。

### 3.5 数据传输 ###

当连接建立成功时，服务端和客户端都会拥有一个socket实例，每个Socket都有一个InputStream和OutputStream，并通过这两个对象来交换数据。
