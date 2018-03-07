title: maven学习
date: 3/10/2016 10:15:49 AM 
tags: maven
---

### Maven介绍 ###

Maven是一个软件项目管理和综合工具。根据项目对象模型（POM）的概念，Maven的可以从中央一条信息管理项目的构建，报告和文件。

### maven项目构建过程 ###

> 清理，编译，测试，打包，集成测试，验证，部署

### maven生命周期 ###

#### clean 清理项目  ####

pre-clean 执行清理前的工作  
clean 清理上一次构建生成的所有文件  
post-clean 执行清理后的文件



#### default 构建项目(核心) ####

compile test package install

#### site 生成项目站点 ####

pre-site 在生成项目站点前要完成的工作  
site 生成项目的站点文档  
post-site 在生成项目站点后要完成的工作  
site-deploy 发布生成的站点到服务器上  

### pox文件解析 ###

0.

	<!-- 指定了当前 pom 的版本 -->
	<modelVersion>4.0.0</modelVersion>

	<!-- 反写的公司网址+项目名 -->
	<groupId>org.apache.maven.plugins</groupId>

	<!-- 项目名+模块名 -->
	<artifactId>maven-source-plugin</artifactId>
	
	<!-- 
		第一个0表示大版本号
		第二个0表示分支版本号
		第三个0表示小版本号
		0.0.1
		snapshot 快照
		alpha 内部测试
		beta 公测
		release 稳定
		ga 正式发布
	 -->
	<version>2.4</version>
	<executions>
			<execution>
				<phase>package</phase>
					<goals>
						<goal>jar-no-fork</goal>
					</goals>
			</execution>
	</executions>

### maven 依赖 ###

#### 依赖  短路优先 ####

	A -> B -> C -> X(jar)
	
	A -> D ->  X(jar)

#### 先声明 先优先 ####

如果路径长度相同，则谁先声明，先解析谁