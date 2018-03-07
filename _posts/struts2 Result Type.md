title: struts2返回结果类型
date: 1/19/2016 9:15:33 AM 
tags: 框架
---
  ## Result Types ##

 ### Chain Result ###

用来处理Action链，被跳转的action中仍能获取上个页面的值

	配置文件
	<package name="public" extends="struts-default">
		<!-- Chain creatAccount to login, using the default parameter -->
		<action name="createAccount" class="com.dream.struts.ResultAction" method="createAccount">
			<result type="chain">login</result>
		</action>

		<action name="login" class="com.dream.struts.ResultAction" method="login">
			<!-- Chain to another namespace -->
			<result type="chain">
				<param name="actionName">dashboard</param>
				<param name="namespace">/secure</param>
			</result>
		</action>
	</package>

	<package name="secure" extends="struts-default" namespace="/secure">
		<action name="dashboard" class="com.dream.struts.ResultAction" method="dashboard">
			<result>/dashboard.jsp</result>
		</action>
	</package>

	ResultAction.java
	public class ResultAction {
	public String createAccount(){
		System.out.println("ceate account");
		return "success";
	}
	
	public String login(){
		System.out.println("login");
		return "success";
	}
	public String dashboard(){
		System.out.println("dashboard");
		return "success";
	}
	}

### Dispatcher Result ###

 缺省值，如果没有配置类型默认就是dispatcher，包括或转发到一个视图(通常是一个jsp)。在后台Struts2将使用一个RequestDispatcher,目标servlet/JSP接收相同的request/response对象作为原始的servlet或JSP。 因此,可以使用request.setAttribute()传递数据- - - Struts的action是可用的。如果请求action会找不到资源。 

	<result name="success" type="dispatcher">
	  <param name="location">/foo.jsp</param>
	</result>

### Redirect Result ###

 重定向到一个资源，唯一的传参方法是通过会话或用OGNL表达式，url参数(url ?名称=值)。 

- **parse：** true by default. If set to false, the location param will not be parsed for Ognl expressions.  

	

	<package name="passingRequestParameters" extends="struts-default" namespace="/passingRequestParameters">
	   <-- Pass parameters (reportType, width and height) -->
	   <!--
	   The redirect url generated will be - the namespace of current acction will be appended as location doesn&#39;t start with "/":
	   /passingRequestParameters/generateReport.jsp?reportType=pie&width=100&height=100#summary
	   -->
	   <action name="gatherReportInfo" class="...">
	      <result name="showReportResult" type="redirect">
	         <param name="location">generateReport.jsp</param>
	         <param name="reportType">pie</param>
	         <param name="width">100</param>
	         <param name="height">100</param>
	         <param name="parse">false</param>
	         <param name="anchor">summary</param>
	      </result>
	   </action>
	</package


### Redirect Action Result ###

### HttpHeader Result ###

### Stream Result ###

### Velocity Result ###

### XSL Result ###

### PlainText Result ###

### Tiles 2 Plugin ###

### Tiles 3 Result ###

### Postback Result ###

### FreeMarker Result ###

 返回一个视图使用FreeMarker模板引擎。

	<result name="success" type="freemarker">foo.ftl</result>