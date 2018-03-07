title: linux（centos）上git的安装
date: 1/20/2016 6:08:24 PM 
tags: linux
---

> 我是在centos的环境下以源码的形式装的

## 先去下源码包 ##

### 下载源码 ###
[git](http://git-scm.com/downloads)在这去下源码包 git-2.7.0.tar.gz

![](http://7xpw00.com1.z0.glb.clouddn.com/imageQQ%E6%88%AA%E5%9B%BE20160120175303.png)

![](http://7xpw00.com1.z0.glb.clouddn.com/imagegit.png)

下好之后上传到linux服务器上,

### 解压，配置安装 ###



	[root@drayy git-2.7.0]# cd git-2.7.0
	[root@drayy git-2.7.0]# ./configure
	[root@drayy git-2.7.0]# make
	[root@drayy git-2.7.0]# make install

但在执行make操作时报错：缺少gcc，yum安装gcc

	[root@drayy git-2.7.0]# yum -y install gcc

重新执行make操作，还报错，缺少zlib.h。我们可以查看是否存在zlib.h

	[root@drayy git-2.7.0]# whereis zlib

如果存在则会输出zlib路径。不存在则输出空白，以下是安装详情：

 - 从[下载zlib](http://www.zlib.net/)下载zlib最新版：zlib-1.2.8.tar.gz


	[root@drayy git-2.7.0]# tar -zxvf zlib-1.2.8.tar.gz
	[root@drayy git-2.7.0]# cd zlib-1.2.8
	[root@drayy git-2.7.0]# ./configure
	[root@drayy git-2.7.0]# make
	[root@drayy git-2.7.0]# make install
	
	再执行
	[root@drayy git-2.7.0]# git --version
	git version 2.7.0

安装成功