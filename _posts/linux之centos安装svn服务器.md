title: cent os 安装svn服务器
date: 1/12/2016 9:36:00 PM 
tags: linux
---
  ## yum 安装 svn ##
	
	# yum install -y subversion

 等待一会,出现 complete 安装完成

 ## 验证是否安装完成 ##

	# svnserve --version  
	
 ## 创建版本库 ##

	# svnadmin  create  svn

 ## 修改目录下的三个文件 ##

 ### svnserve.conf   ###

	anon-access = none #没有登录的用户不能访问  
	auth-access = write #登录的用户可以写入  
	password-db = passwd #密码文件为当前目录下的passwd    
	authz-db = authz #验证文件为当前目录下的authz 

 **注意**前边不能有空格有 # 号

 ### 修改 passwd 文件 ###

	[users]
	admin = 123456 # 用户 和 密码
	user = 123456  # 用户 和 密码

 ### 修改authz文件 ###

	[groups]
	team1 = admin
	team2 = user   #将上面创建的分成两个组 

 	[/]
	@team1 = rw 
	@team2 = r
	# 第一个小组只有读写的权限，第二个小组有读取的权限  

 ## 启动svn ##

	# svnserve -d -r svn/ 

 ## 关闭svn  ##

	# ps -aux |grep svn
	Warning: bad syntax, perhaps a bogus '-'? See /usr/share/doc/procps-3.2.7/FAQ
	root       854  0.0  0.2  80252  1128 ?        Ss   08:54   0:00 svnserve -d -r /home/svn
	root      1353  0.0  0.1   7208   820 pts/0    S+   16:56   0:00 grep svn

	# kill -9 854(进程 id)


