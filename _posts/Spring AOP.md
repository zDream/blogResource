title: Spring AOP
date: 1/5/2016 1:46:44 PM 
tags: Spring
---

> **本文来源于官方文档**

### Aspect Oriented Programming with Spring 面向切面编程 ###

 #### 简介 ####

>  - 面向切面编程（AOP）弥补面向对象编程（OOP）通过提供思考程序结构的另一种方式。在OOP中模块化的关键单元是类，而在AOP模块为单位的切面。面对关注点，如事务管理跨多个类型和对象切模块化。（这些关注经常被称为横切在AOP文学的关注。）
>  - Spring的一个关键组件就是AOP框架。虽然Spring IoC容器并不依赖于AOP，这意味着你不需要使用AOP，如果你不想，AOP补充Spring IoC容器提供了非常强大的中间件解决方案。

##### AOP概念 #####

 - **切面(Aspect)**: 横切关注点(跨越应用程序多个模块的功能)被模块化的特殊对象
 - **连接点(Join point)**： 一个程序，在执行过程中的一个点，如方法的执行或异常的处理。在Spring AOP中，一个连接点总是 代表一个方法的执行。
 - **通知(Advice)**： 在某个特定的一个方面动作的连接点。不同类型的建议包括“around”，“before”和“after”的建议。（advice 的类型将在下面讨论）。许多AOP框架，包括Spring，Model 中的Advice 作为拦截器，保持了拦截器链周围的**连接点(Joinpoint)**。
 - **切点(pointcut)**： 每个类都拥有多个连接点：例如 ArithmethicCalculator 的所有方法实际上都是连接点，即连接点是程序类中客观存在的事务。AOP 通过切点定位到特定的连接点。类比：连接点相当于数据库中的记录，切点相当于查询条件。切点和连接点不是一对一的关系，一个切点匹配多个连接点，切点通过 org.springframework.aop.Pointcut 接口进行描述，它使用类和方法作为连接点的查询条件
 - **代理(AOP proxy)**： AOP框架，以实现Aspect契约（例如通知方法执行等等）创建的对象。在Spring中，AOP代理可以是JDK动态代理或者CGLIB代理。

**advice的类型** :

 - Before advice: 建议一个执行一个 Join point 之前，但它不具有防止执行流程的连接点（除非它抛出一个异常）的能力。
 - After returning advice: 连接点(Join point)正常完成后,方法没有抛出异常后执行
 - After throwing advice: advice抛出一个异常时执行。
 - After (finally) advice: 结果都会执行，
 - Around advice: 环绕通知, 围绕着方法执行



##### Spring 使用 AspectJ 注解 实现AOP 编程#####、

	需导入 aop,aspects 相关jar包

	AtithmeticCalculatorImpl.java实现类
	package com.dream.apo.impl;
	import org.springframework.stereotype.Component;
	@Component("atithmeticCalculator1")
	public class AtithmeticCalculatorImpl implements AtithmeticCalculator {
	@Override
	public int add(int i, int j) {
		int result=i+j;
		return result;
	}
	@Override
	public int sub(int i, int j) {
		int result=i-j;
		return result;
	}
	@Override
	public int mul(int i, int j) {
		int result=i*j;
		return result;
	}
	@Override
	public int div(int i, int j) {
		int result=i/j;
		return result;
	}
	}
	
	AOP实现代码 LoginAspect.java 
	package com.dream.apo.impl;

	import java.util.Arrays;
	
	import org.aspectj.lang.JoinPoint;
	import org.aspectj.lang.annotation.After;
	import org.aspectj.lang.annotation.AfterReturning;
	import org.aspectj.lang.annotation.AfterThrowing;
	import org.aspectj.lang.annotation.Aspect;
	import org.aspectj.lang.annotation.Before;
	import org.springframework.stereotype.Component;	
	@Aspect
	@Component
	public class LoginAspect {
	@Before("execution(public int com.dream.apo.impl.AtithmeticCalculator.*(..) )")
	public void beforeMethod(JoinPoint joinPoint){
		String methodName=joinPoint.getSignature().getName();
		Object []args=joinPoint.getArgs();
		System.out.println("the method is "+methodName+"begin with "+Arrays.asList(args));		
	}	
	@After("execution(public int com.dream.apo.impl.AtithmeticCalculator.*(..) )")
	public void after(JoinPoint joinPoint){
		String methodName=joinPoint.getSignature().getName();
		System.out.println("the method is "+methodName+" ends ");		
	}	
	@AfterReturning(value="execution(public int com.dream.apo.impl.AtithmeticCalculator.*(..) )",
			returning="result")
	public void afterReturning(JoinPoint joinPoint,Object result){
		String methodName=joinPoint.getSignature().getName();
		System.out.println("the method is "+methodName+" return ends "+result);
	}	
	@AfterThrowing(value="execution(public int com.dream.apo.impl.AtithmeticCalculator.*(..) )",
			throwing="ex")
	public void afterThrowing(JoinPoint joinPoint,Exception ex){		
		String methodName=joinPoint.getSignature().getName();
		System.out.println("the method is "+methodName+" occurs exception  "+ex);
	}
	}

	主方法
	package com.dream.apo.impl;

	import org.springframework.context.ApplicationContext;
	import org.springframework.context.support.ClassPathXmlApplicationContext;
	public class Main {
	public static void main(String[] args) {
		ApplicationContext ac=new ClassPathXmlApplicationContext("applicationContext.xml");
		AtithmeticCalculator act= (AtithmeticCalculator) ac.getBean("atithmeticCalculator1");
		
		int result=act.add(3, 4);
		System.out.println("result="+result);
		
		result=act.div(6, 1);
		System.out.println("result="+result);
	}
	}

	applicationContext.xml 配置文件 
	<context:component-scan base-package="com.dream.apo.impl"></context:component-scan>
	
	<aop:aspectj-autoproxy></aop:aspectj-autoproxy>