title: git分布式学习
date: 3/1/2016 9:32:58 AM 
tags: git
---

### git分布式 ###

	跳转到gitwork空目录 
	Dreamer@Dream MINGW64 /e/gitwork
	$ git init  ##初如化git目录
	Initialized empty Git repository in E:/gitwork/.git/
	
	Dreamer@Dream MINGW64 /e/gitwork (master)

### 把文件添加到git 仓库 ###


	先把文件添加到仓库
	$ git add readme.txt

	把文件提交到仓库
	$ git commit -m "write a readme file"
	[master (root-commit) 5dc3355] write a readme file
	 1 file changed, 2 insertions(+)
	 create mode 100644 readme.txt

---

查看仓库当前的状态

	git status


查看当前文件具体详细情况

	git diff readme.txt 

显示提交日志

	git log

	commit d1b6a7db84b59c263e9e2435f5506f4b16e79ce9
	Author: dream <zttongdeyx@126.com>
	Date:   Tue Mar 1 10:26:32 2016 +0800
	
	    second
	
	commit 89090c7d0b47bcd9f672f2f5f38f289036eb5791
	Author: dream <zttongdeyx@126.com>
	Date:   Tue Mar 1 10:25:57 2016 +0800
	
	    second
	
	commit 95bbe5de7cee7658879cef2c32c208006060f531
	Author: dream <zttongdeyx@126.com>
	Date:   Tue Mar 1 10:21:59 2016 +0800
	
	    first
	
	commit 70a86aa447ad9a626dcd63895d0505199e1e1980
	Author: dream <zttongdeyx@126.com>
	Date:   Tue Mar 1 09:55:06 2016 +0800
	
	    init

	最上边的为最后提交的

版本回退

	git reset --hard head^ #回退到上一个版本

	head表示当前版本 上一个版本上 head^ ，上上一个就是head^^,可以写成 head~10

	git  reset --hard 3628164(版本号，不用写全，写一部分就好，会根据前几位自动去找)

用来记录你的每一次命令：

	git reflog

### 撤销修改命令 ###

	git checkout -- test.txt

> 把工作区的修改全部撤销  
> 这里撤销分两种情况

 1. 修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
 2. 已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

就是让这个文件回到最近一次git commit或git add时的状态。


> 如果已经添加到工作区又想回退到之前的状态，需要两个步骤

 1. 命令 `git reset HEAD file`，就回退到没有添加到工作区之前
 2. 然后再 `git check -- text.txt`

> 如果已经添加到版本库的话则回退版本

	git reset --hard head^ 

### 删除文件 ###

当你删除一个文件之后会有两种情况

 1. 的确要从版本库中删除文件

	git rm test.txt  
	git commit -m "remove test.txt"

 2. 另一种是删错了，版本库中还有，可以回退

	git checkout -- test.txt  
	其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

### ssh密钥 ###

	 ssh-keygen -t rsa -C "youremail@example.com"

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

### 往远程库中推送代码 ###

创建好库之后根据提示推送代码

	git remote add origin git@github.com:zDream/demo.git
	git push -u origin master

使用如上命令即可推送代码，git会默认从本地  默认扫描 ~/.ssh/id_pub  ssh，从而和github上的ssh关联起来，所以才能推送成功。

	git push origin master

推送最新修改的代码

### 克隆远程库代码  ###

	git clone git@github.com:zDream/second.git

克隆支持多种协议 https 和ssh 。默认使用ssh

使用https除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用ssh协议而只能用https。

通过ssh支持的git协议速度最快

### git log显示问题 ###

当 显示日志过多的时候会显示不完整，会出现如图情况，上下箭头可上下翻，按 q 退出显示日志
![](http://7xpw00.com1.z0.glb.clouddn.com/imagegit1.png)

### git分支管理 ###

首先，我们创建dev分支，然后切换到dev分支：

	$ git checkout -b dev
	Switched to a new branch 'dev'
	
	以上命令相当于以下两条命令  git checkout命令加上-b参数表示创建并切换

	$ git branch dev
	$ git checkout dev
	Switched to branch 'dev'


查看当前分支，当前分支前面会标一个 * 号，然后可以去分支进行修改，提交

	$ git branch
	* dev
	  master


切换分支

	$ git checkout master
	Switched to branch 'master'

合并分支

	$ git merge dev
	Updating d17efd8..fec145a
	Fast-forward
	 readme.txt |    1 +
	 1 file changed, 1 insertion(+)

删除分支

	$ git branch -d dev
	Deleted branch dev (was fec145a).

### 远程库 ###

远程仓库的默认名称是origin  

查看远程库的信息

	$ git remote
	origin

显示更详细信息

	$ git remote -v
	origin  git@github.com:zDream/first.git (fetch)
	origin  git@github.com:zDream/first.git (push)


推送分支 需要指定本分支，即可推送到远程库中

	$ git push origin master	

如果要推送其它分支

	$ git push origin dev

### 标签管理 ###

Git中打标签非常简单，首先，切换到需要打标签的分支上：	

打标签

	git tag v1.0

查看所有标签

	$ git tag
	v1.0

找到历史提交的 id 

	git log --pretty=oneline --abbrev-commit
	6a5819e merged bug fix 101
	cc17032 fix bug 101
	7825a50 merge with no-ff
	6224937 add merge
	59bc1cb conflict fixed
	400b400 & simple
	75a857c AND simple
	fec145a branch test
	d17efd8 remove test.txt
	...

显示出历史信息时可以用如下命令打标签

	$ git tag v0.9 6224937

查看标签信息

	$ git show v0.9


还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：

	$ git tag -a v0.1 -m "version 0.1 released" 3628164

删除标签

	$ git tag -d v0.1
	Deleted tag 'v0.1' (was e078af9)

推送标签到远程

	$ git push origin v1.0
	Total 0 (delta 0), reused 0 (delta 0)
	To git@github.com:michaelliao/learngit.git
	 * [new tag]         v1.0 -> v1.0

推送全部尚未推送到远程的本地标签：

	$ git push origin --tags
	Counting objects: 1, done.
	Writing objects: 100% (1/1), 554 bytes, done.
	Total 1 (delta 0), reused 0 (delta 0)
	To git@github.com:michaelliao/learngit.git
	 * [new tag]         v0.2 -> v0.2
	 * [new tag]         v0.9 -> v0.9

如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：

	$ git tag -d v0.9
	Deleted tag 'v0.9' (was 6224937)

然后，从远程删除。删除命令也是push，但是格式如下：

	$ git push origin :refs/tags/v0.9
	To git@github.com:michaelliao/learngit.git
	 - [deleted]         v0.9