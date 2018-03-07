title: Spring入门
date: 1/5/2016 1:46:44 PM 
tags: Spring
---
## Spring入门 ##

 ### Spring简介 ###

 - Spring 是一个开源框架.
 - Spring 为简化企业级应用开发而生. 使用 Spring 可以使简单的 JavaBean 实现以前只有 EJB 才能实现的功能.
 - Spring 是一个 IOC(DI) 和 AOP 容器框架

### Spring特性 ###

 - **轻量级**：Spring 是非侵入性的 - 基于 Spring 开发的应用中的对象可以不依赖于 Spring 的 API
 - **依赖注入**(DI --- dependency injection、IOC)
 - **面向切面编程**(AOP --- aspect oriented programming)
 - **容器**: Spring 是一个容器, 因为它包含并且管理应用对象的生命周期
 - **框架**: Spring 实现了使用简单的组件配置组合成一个复杂的应用. 在 Spring 中可以使用 XML 和 Java 注解组合这些对象
 - **一站式**：在 IOC 和 AOP 的基础上可以整合各种企业应用的开源框架和优秀的第三方类库 （实际上 Spring 自身也提供了展现层的 SpringMVC 和 持久层的 Spring JDBC）

### 搭建spring开发环境 ###

 需要导入以下jar 包

    commons-logging.jar
	spring-beans-4.2.1.RELEASE.jar
	spring-context-4.2.1.RELEASE.jar
	spring-core-4.2.1.RELEASE.jar
	spring-expression-4.2.1.RELEASE.jar

 加这么多就足够了，如果还需要其它功能，请加入其它的jar包

### 内容提要 ###

#### IOC & DI 概述 ####

 - IOC(Inversion of Control)：其思想是反转资源获取的方向. 传统的资源查找方式要求组件向容器发起请求查找资源. 作为回应, 容器适时的返回资源. 而应用了 IOC 之后, 则是容器主动地将资源推送给它所管理的组件, 组件所要做的仅是选择一种合适的方式来接受资源. 这种行为也被称为查找的被动形式
 - DI(Dependency Injection) — IOC 的另一种表述方式：即组件以一些预先定义好的方式(例如: setter 方法)接受来自如容器的资源注入. 相对于 IOC 而言，这种表述更直接

#### 配置bean ####
   - 配置形式：基于 XML 文件的方式；基于注解的方式
   - Bean 的配置方式：通过全类名（反射）、通过工厂方法（静态工厂方法 & 实例工厂方法）、FactoryBean
   - IOC 容器 BeanFactory & ApplicationContext 概述
   - 依赖注入的方式：属性注入；构造器注入
   - 注入属性值细节
   - 自动转配
   - bean 之间的关系：继承；依赖
   - bean 的作用域：singleton；prototype；WEB 环境作用域
   - 使用外部属性文件
   - spEL
   - IOC 容器中 Bean 的生命周期
   - Spring 4.x 新特性：泛型依赖注入

##### 在xml中配置bean #####

	<!-- 通过全类名的方式配置bean -->
	<bean id="dao" class="com.dream.hello.Dao"> </bean>

 id:bean的名称，在 IOC 容器中必须是唯一的，若 id 没有指定，Spring 自动将权限定性类名作为 Bean 的名字

##### 依赖注入 #####

 **属性注入**  

 - 属性注入即通过 setter 方法注入Bean 的属性值或依赖的对象
 - 属性注入使用 <property> 元素, 使用 name 属性指定 Bean 的属性名称，value 属性或 <value> 子节点指定属性值 
 - 属性注入是实际应用中最常用的注入方式

	<bean id="dao" class="com.dream.hello.Dao">
		<property name="name" value="zhangsan"></property>
	</bean>

 **构造方法注入**  

 - 通过构造方法注入Bean 的属性值或依赖的对象，它保证了 Bean 实例在实例化后就可以使用。
 - 构造器注入在 <constructor-arg> 元素里声明属性, <constructor-arg> 中没有 name 属性


>  按索引匹配入参：


	配置文件:
    <bean id="dao" class="com.dream.hello.Dao">
		<constructor-arg value="zhangsan" index="0"></constructor-arg>
		<constructor-arg value="12" index="1"></constructor-arg>
	</bean>  
	实体类:要有相对应的构造器和set get方法
	public Dao(String name, int age) {
		this.name = name;
		this.age = age;
	}
     

> 按类型匹配入参

	配置文件:
    <bean id="dao" class="com.dream.hello.Dao">
		<constructor-arg value="zhangsan" type="java.lang.String"></constructor-arg>
		<constructor-arg value="12" type="java.lang.Integer"></constructor-arg>
	</bean>
	实体类:把int改成Integer, 
	public Dao(String name, Integer age) {
		this.name = name;
		this.age = age;
	}


 ### Spring对JDBC的支持 ###

> - 为了使 JDBC 更加易于使用, Spring 在 JDBC API 上定义了一个抽象层, 以此建立一个 JDBC 存取框架.
> - 作为 Spring JDBC 框架的核心, JDBC 模板的设计目的是为不同类型的 JDBC 操作提供模板方法. 每个模板方法都能控制整个过程, 并允许覆盖过程中的特定任务. 通过这种方式, 可以在尽可能保留灵活性的情况下, 将数据库存取的工作量降到最低.

 **Spring提供了3个模板类**  

 - JdbcTemplate：Spring里最基本的JDBC模板，利用JDBC和简单的索引参数查询提供对数据库的简单访问。
 - NamedParameterJdbcTemplate：能够在执行查询时把值绑定到SQL里的命名参数，而不是使用索引参数。
 - SimpleJdbcTemplate：利用Java 5的特性，比如自动装箱、通用（generic）和可变参数列表来简化JDBC模板的使用。

 **JdbcTemplate主要提供以下4类方法**

 - execute方法：可以用于执行任何SQL语句，一般用于执行DDL语句；
 - update方法及batchUpdate方法：update方法用于执行新增、修改、删除等语句；batchUpdate方法用于执行批处理相关语句；
 - query方法及queryForXXX方法：用于执行查询相关语句；
 - call方法：用于执行存储过程、函数相关语句。


 #### JdbcTemplate 的使用 ####


    实现代码:
	
	db.properties
	url=jdbc\:oracle\:thin\:@localhost\:1521\:orcl
	driver=oracle.jdbc.driver.OracleDriver
	user=test
	password=ztt123456

	application.xml
	<!-- 导入资源文件 -->
	<context:property-placeholder location="classpath:db.properties"/>
	<!-- 配置 C3P0 数据源 -->
	<bean id="dataSource"
		class="com.mchange.v2.c3p0.ComboPooledDataSource">
		<property name="user" value="${user}"></property>
		<property name="password" value="${password}"></property>
		<property name="jdbcUrl" value="${url}"></property>
		<property name="driverClass" value="${driver}"></property>
	</bean>
	<!-- 配置 Spirng 的 JdbcTemplate -->
	<bean id="jdbcTemplate" 
		class="org.springframework.jdbc.core.JdbcTemplate">
		<property name="dataSource" ref="dataSource"></property>
	</bean>

	Test.java
	测试代码
	public class Main extends JdbcDaoSupport{

	@Test
	public  void Test1() {
		ApplicationContext context=new ClassPathXmlApplicationContext("application.xml");
		Dao bean =  (Dao) context.getBean("dao");
		bean.method();
	}
	
	@Test
	public void test1(){
		ApplicationContext context=new ClassPathXmlApplicationContext("application.xml");
		JdbcTemplate bean = (JdbcTemplate) context.getBean("jdbcTemplate");
		System.out.println(bean.toString());
	}
	
	@Test
	public void test2(){
		JdbcTemplate jdbcTemplate2 = getJdbcTemplate();
		System.out.println(jdbcTemplate2.toString());
	}
	}
	请自行添加所需jar包和相关配置文件

