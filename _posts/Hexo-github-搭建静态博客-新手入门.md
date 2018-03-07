title: Hexo+github搭建静态博客(新手入门)
tags: hexo
---
## 前言 ##

> 能来到这，说明你应该对博客有了些认知。相信你应该熟悉WordPress吧，或者说以前就是用的WordPress搭建的博客。本文针对于初学者，教程十分详细。我已经无法吐槽网上那些建博客教程，不是不能用，就是缺很多东西，到最后装也装不好，也不知道哪错了。在这我把我建博客的步骤写下来。希望能你给你们一些帮助。


### 准备工作 ### 


  - window8.1开发平台 楼主是在window下装的，虽然说是最烂的开发平台，但还是蛮好用的
  - 安装node.js [node官网][1]下载最新版本，一路安装就好
  - 安装git [get官网][2]下载与你电脑相对应的版本，安装
  - github账号，用户名注册之后便不可修改，用来做博客的远程仓库的，
  - 域名(可不需要也行) 可以通过域名来访问你的博客
 

### 安装hexo ###


  打开Git命令行，执行如下命令

    $  npm install -g hexo-cli


  初始化在电脑里建一个空文件夹(不要在C盘),我是在 E 盘建了一个 git 文件夹，然后右键打开 Git Bash Here,键入如下命令

    Dreamer@Dream MINGW64 /d/test
	$ hexo init #初始化文件夹，会在目录下生成一些文件，随后按照提示运行   
	INFO Copying data to D:\test  
    INFO  You are almost done! Don't forget to run 'npm install' before you   start blogging with Hexo!
	 ##按照提示输入
	$ npm install 耐心等待会，不要着急，然后运行下面命令
	$ hexo server`
	INFO  Hexo is running at http://0.0.0.0:4000/. Press Ctrl+C to stop.
表明Hexo Server已经启动了，在浏览器中打开 http://localhost:4000/，  这时可以看到Hexo已为你生成了一篇blog。你可以按Ctrl+C 停止Server。


**安装hexo插件(当时我就是因为没有装这些插件，导致各种失败)**

    npm install hexo-generator-index --save
    npm install hexo-generator-archive --save
    npm install hexo-generator-category --save
    npm install hexo-generator-tag --save
    npm install hexo-server --save
    npm install hexo-deployer-git --save
    npm install hexo-deployer-heroku --save
    npm install hexo-deployer-rsync --save
    npm install hexo-deployer-openshift --save
    npm install hexo-renderer-marked@0.2 --save
    npm install hexo-renderer-stylus@0.2 --save
    npm install hexo-generator-feed@1 --save
    npm install hexo-generator-sitemap@1 --save


**hexo常用命令**

    hexo new "文章名字" #生成一篇文章，在source目录下能看到,可简写  hexo n
    hexo new page"pageName"  #新建页面
    hexo generate  #生成静态页面至public目录   可简写 hexo g
    hexo server    #开启预览访问端口			 可简写 hexo s
    hexo deploy    #将.deploy目录部署到GitHub  可简写  hexo d
    hexo help      # 查看帮助
    hexo version   #查看Hexo的版本



### 配置并部署到github ###


首先登录github，创建你用户名对应的仓库，仓库名必须是如图格式，他们都说是必须，我也不知道为什么，你如果知道了原因告诉我声。

 ![如图](http://7xpw00.com1.z0.glb.clouddn.com/image11.png)


配置ssh密钥参考 [官方文档](https://help.github.com/articles/generating-ssh-keys/)(可能需要翻墙才能访问)如果你英文不错的话可以去看这个，这个写的很详细。
 
SSH密钥是一种方法来确定受信任的计算机，而不涉及密码。下面的步骤将引导您完成生成SSH密钥并添加公共密钥到你的GitHub上的帐户。
 
 1. 生成ssh 密钥  


	$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

 2. 输入要保存的文件的路径，保持默认即可 然后 回车


	Enter file in which to save the key (/Users/you/.ssh/id_rsa): [Press enter]

 3. 输入密码，密码是暗文的，输入即可


	Enter passphrase (empty for no passphrase): [Type a passphrase]
	Enter same passphrase again: [Type passphrase again]



>  完成以上工作之后 去文件目录应该会有id_rsa  和id_rsa.pub两个官钥
>  id_rsa 是私有的，不可公开
>  id_rsa.put 可公开

去github仓库中把ssh密钥加入进去 打开头像下的 Setting 目录

 ![](http://7xpw00.com1.z0.glb.clouddn.com/imagessk.png)
 
 点 Add SSH key
 title 随你写
 key 打开id_rsa.put 粘贴里面全部内容。保存即可


要配置的ssh-agent程序使用你的SSH密钥

 - 确保ssh 代理已启用

	$ ssh-agent -s

	$ eval $(ssh-agent -s)

 - 添加您的SSH密钥对的ssh-agent ，应该会让你输入密码

	$ ssh-add ~/.ssh/id_rsa (路径要改相应位置)

测试连接

	$ ssh -T git@github.com
	Hi zDream! You've successfully authenticated, but GitHub does not provide shell access.

 出现上图所示既表示添加成功

### 配置文件并上传到远程仓库中 ###


 如图是我的配置文件，_config.yml 
 
 **注意： 配置文件语法  xxx: content**  冒号后的空格必不可少


	# Hexo Configuration
	## Docs: http://hexo.io/docs/configuration.html
	## Source: https://github.com/hexojs/hexo/
	
	# Site
	title: 我的记忆
	subtitle: 点点滴滴
	description: 生命的足迹
	author: Dream
	language:
	timezone:
	
	# URL
	## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
	url: http://www.drayy.com
	root: /
	permalink: :year/:month/:day/:title/
	permalink_defaults:
	
	# Directory
	source_dir: source
	public_dir: public
	tag_dir: tags
	archive_dir: archives
	category_dir: categories
	code_dir: downloads/code
	i18n_dir: :lang
	skip_render:
	
	# Writing
	new_post_name: :title.md # File name of new posts
	default_layout: post
	titlecase: false # Transform title into titlecase
	external_link: true # Open external links in new tab
	filename_case: 0
	render_drafts: false
	post_asset_folder: false
	relative_link: false
	future: true
	highlight:
	  enable: true
	  line_number: true
	  auto_detect: true
	  tab_replace:
	
	# Category & Tag
	default_category: uncategorized
	category_map:
	tag_map:
	
	# Date / Time format
	## Hexo uses Moment.js to parse and display date
	## You can customize the date format as defined in
	## http://momentjs.com/docs/#/displaying/format/
	date_format: YYYY-MM-DD
	time_format: HH:mm:ss
	
	# Pagination
	## Set per_page to 0 to disable pagination
	per_page: 10
	pagination_dir: page
	
	# Extensions
	## Plugins: http://hexo.io/plugins/
	## Themes: http://hexo.io/themes/
	theme: jacman
	stylus:
	     compress: true
	
	# Deployment
	## Docs: http://hexo.io/docs/deployment.html
	deploy:
	  type: git
	  repository: git@github.com:zDream/zDream.github.io.git
	  branch: master
	  message: '站点更新: \{\{ now("YYYY-MM-DD HH:mm:ss") \}\}'



---

每次部署都需要执行如下命令，如果不出错可以不clean，
	
	hexo clean 
	hexo g
	hexo d

注意看命令后的那些代码，看有没有错误，如何上述全部都成功的话，

去访问你的博客应该就好了 http://zDream.github.com  ,改成你的名字即可

  [1]: https://nodejs.org/en/
  [2]: http://git-scm.com/download/