title: Spring MVC笔记
date: 2/17/2016 10:13:05 AM 
tags: 框架
---

## struts和spring mvc ##

> Struts创建的action类是多例模式，当刷新页面的时候会重新new Action类
> spring mvc  是单例模式，只创建一个

Spring mvc Bean配置文件 name的表示请求路径

## Controller类中方法参数，返回值 的处理 ##

1.    返回string(建议)  
a)         根据返回值找对应的显示页面。路径规则为：prefix前缀+返回值+suffix后缀组成  
b)         代码如下：

	@RequestMapping(params="method=reg4")
    public String reg4(ModelMap map) {
       System.out.println("HelloController.handleRequest()");
       return"index";
    }

 前缀为：/WEB-INF/jsp/    后缀是：.jsp
 在转发到：/WEB-INF/jsp/index.jsp
 
2.  也可以返回ModelMap、ModelAndView、map、List、Set、Object、无返回值。一般建议返回字符串！