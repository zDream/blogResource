title: mybatis
date: 1/26/2016 11:28:48 AM 
tags: 框架
---

## 介绍 ##

> MyBatis是支持普通SQL查询，存储过程和高级映射的优秀持久层框架。MyBatis消除了几乎所有的JDBC代码和参数的手工设置以及对结果集的检索封装。MyBatis可以使用简单的XML或注解用于配置和原始映射，将接口和Java的POJO（Plain Old Java Objects，普通的Java对象）映射成数据库中的记录

## 添加jar 包 ##

	【mybatis】
		mybatis-3.1.1.jar
	【MYSQL驱动包】
		mysql-connector-java-5.1.7-bin.jar

## 添加Mybatis的配置文件conf.xml ##


	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
	<configuration>
		<environments default="development">
			<environment id="development">
				<transactionManager type="JDBC" />
				<dataSource type="POOLED">
					<property name="driver" value="oracle.jdbc.driver.OracleDriver" />
					<property name="url" value="jdbc:oracle:thin:@localhost:1521:orcl" />
					<property name="username" value="test" />
					<property name="password" value="ztt123456" />
				</dataSource>
			</environment>
		</environments>
		<mappers>
			<mapper resource="com/dream/bean/userMapper.xml"/>
		</mappers>
	</configuration>

## 	定义表所对应的实体类 ##

	public class User {
	private int id;
	private String name;
	private int age;
    //get,set方法
	}

## 定义操作users表的sql映射文件userMapper.xml ##

	<<?xml version="1.0" encoding="UTF-8" ?>
	<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
	<mapper namespace="com.dream.bean.userMapper">
	 <insert id="insertUser" parameterType="com.dream.bean.User">
		insert into user11(id,name,age) values(#{id},#{name},#{age})
	</insert>

	<select id="getUser" parameterType="int"
		resultType="com.dream.bean.User">
		select * from user11 where id=#{id}
	</select>

	</mapper>


## 编写测试代码：执行定义的select语句 ##

	package com.dream.test;
	
	import java.io.IOException;
	import java.io.Reader;
	
	import org.apache.ibatis.io.Resources;
	import org.apache.ibatis.session.SqlSession;
	import org.apache.ibatis.session.SqlSessionFactory;
	import org.apache.ibatis.session.SqlSessionFactoryBuilder;
	import org.junit.Test;
	
	import com.dream.bean.User;
	
	public class myTest {

	@Test
	public void mTest() throws IOException {
			String resource = "conf.xml";
			// 加载mybatis的配置文件（它也加载关联的映射文件）
			Reader reader = Resources.getResourceAsReader(resource);
			// 构建sqlSession的工厂
			SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(reader);
			// 创建能执行映射文件中sql的sqlSession
			SqlSession session = sessionFactory.openSession();
			System.out.println(session.getConnection());
			// 映射sql的标识字符串
			String statement = "com.dream.bean.userMapper.insertUser";
			// 执行查询返回一个唯一user对象的sql
			int user = session.insert(statement, new User(1,"zhansan",3));
			// 提交
			
		//		String statement = "com.dream.bean.userMapper.getUser";
		//		User selectOne = session.selectOne(statement, 4);
		//		System.out.println(selectOne.toString());
				
			session.commit();
	
			session.close();
	
			//System.out.println(user);
		}

	}


至此例子写好了

---