title: mybatis 实体类，配置文件生成工具
date: 3/24/2016 12:00:18 AM 
tags: 工具
---

注：你如果英文足够好，把工具下下来，在  docs目录下可以去看  index.html官方文档，这才是权威性的文档。

> 你是否还在为写mybatis实体类，和mybatis配置文件烦恼，那么多的实体类，看得我是头晕眼花，正是因为如此，我一直在找有什么办法可以解决。    自从用了  mybatis-generator生成工具，再也不用担心那些繁多的字段和配置文件了


## 下面来开始mybatis-generator的学习 ##

介绍不用我多说了吧，你能来到这，说明你应该有了了解。官方那么破文档我也看不懂，用这个工具，可以生成 实体类和配置文件，当然还有其它的方式。

## 准备工具 ##

先去下载工具 [下载](https://github.com/mybatis/generator/releases) 地址如下https://github.com/mybatis/generator/releases

![](http://7xpw00.com1.z0.glb.clouddn.com/imagemybatis2.png)

## 配置文件 ##


![](http://7xpw00.com1.z0.glb.clouddn.com/imagemybatis.png)

在目录文件下加入你要连数据库驱动的jar包和 generatorConfig.xml配置文件

generatorConfig.xml配置文件如下，可参考配置

		<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE generatorConfiguration PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN" "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
	<generatorConfiguration>
	<!-- classPathEntry:数据库的JDBC驱动,换成你自己的驱动位置 -->
	<classPathEntry location="E:\app\Administrator\product\11.2.0\dbhome_1\jdbc\lib\ojdbc6.jar" />
	<context id="ORATables" targetRuntime="MyBatis3" defaultModelType="flat" >
		<!-- <plugin type="com.nova.his.core.mybatis.plugin.DeleteLogicByIdsPlugin">
		</plugin> -->
		<!-- 去除自动生成的注释 -->
		<commentGenerator>
			<property name="suppressDate" value="true" />
			<property name="suppressAllComments" value="true" />
		</commentGenerator>
		
		 <!-- 配置连接数据信息 -->
		<jdbcConnection driverClass="oracle.jdbc.driver.OracleDriver" connectionURL="jdbc:oracle:thin:@192.168.88.98:1521:orcl" userId="nova_his" password="admin"> 
		</jdbcConnection>
		<javaTypeResolver >
			<property name="forceBigDecimals" value="false" />
		</javaTypeResolver>
		
		<!-- 配置自动生成的Model的保存路径与其它参数 -->
		<javaModelGenerator targetPackage="com.nova.his.medicalworkstation.model" targetProject="src\main\java"  >
			<property name="enableSubPackages" value="false"/> 
			<property name="trimStrings" value="true"/> 
		</javaModelGenerator> 
		
		<!-- 配置自动生成的Mappper.xml映射的保存路径与其它参数 --> 
		<sqlMapGenerator targetPackage="com.nova.his.medicalworkstation.dao.mappings" targetProject="src\main\java"> 
			<property name="enableSubPackages" value="false"/> 
		</sqlMapGenerator>
		
		<!-- 配置自动生成的Mappper.java接口的保存路径与其它参数 --> 
		<javaClientGenerator type="XMLMAPPER" targetPackage="com.nova.his.medicalworkstation.dao" targetProject="src\main\java">
			<property name="enableSubPackages" value="false"/> 
		</javaClientGenerator>
		
		<!-- 生成表对应的操作与实体对象 -->
		<!--tableName和domainObjectName为必选项，分别代表数据库表名和生成的实力类名，其余的可以自定义去选择（一般情况下均为false）。-->
		<table  tableName="jy_bgd_dwo" domainObjectName="Jybgd" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false"> 
			<property name="useActualColumnNames" value="false"/> 
		</table>
		
	</context>
	</generatorConfiguration>

## 运行，生成代码，配置文件 ##
	
打开 cmd 命令行，运行如下代码即可生成相应的代码	

	java -jar mybatis-generator-core-1.3.2.jar -configfile generatorConfig.xml -overwrite

![](http://7xpw00.com1.z0.glb.clouddn.com/imagemybatiss01.png)

**注意，不要把mybatis-generator-core-1.3.2这个生成工具放到中文路径下。**