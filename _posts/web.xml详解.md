title: web.xml详解
date: 1/27/2016 2:11:54 PM 
tags: java
---

### web.xml的作用 ###

web.xml，一个Tomcat工程中最重要的配置文件。web.xml没有其实也可以----只要你确定你的项目里面不需要任何过滤器、监听器、Servlet等等。我试了一下，没有web.xml对那些已经编译成Servlet的jsp页面来说，是不影响正常显示的，但是那些没有编译成Servlet的jsp页面，访问的时候就会报500的错误了。下面逐一看一下web.xml里常用标签的作用。

### welcome-file-list ###

这个标签是用来配置首页用的：

	<welcome-file-list>
	    <welcome-file>index1.jsp</welcome-file>
	    <welcome-file>index2.jsp</welcome-file>
	    <welcome-file>index3.jsp</welcome-file>
	    <welcome-file>index4.jsp</welcome-file>
	    <welcome-file>/target/redirectAndFoward.jsp</welcome-file>
	</welcome-file-list>

这么配置的意思，就是当用户访问http://ip:port/工程名的时候，会根据welcome-file-list配置的页面列表，从项目的根目录开始找页面：

 - 第一个配置的index1.jsp能找到，就展示index1.jsp
 - 找不到index1.jsp，则去找第二个index2.jsp，index2.jsp能找到就展示index2.jsp，
 - 找不到index3.jsp，则去找第三个index3.jsp，以此类推，如果所有的页面都找不到则报HTTP Status 404即页面找不到

**注意一下**，像配置的最后一个welcome-file这种写法也是支持的，我试了一下最前面的那个"/"可加可不加


### error-page ###

error-page表示当HTTP返回指定状态码的时候，容器将此次请求转发到配置的指定页面： 

	<error-page>
	    <error-code>400</error-code>
	    <location>/filter/error.jsp</location>
	</error-page>
	  
	<error-page>
	    <error-code>404</error-code>
	    <location>/filter/error.jsp</location>
	</error-page>
	  
	<error-page>
	    <error-code>500</error-code>
	    <location>/filter/error.jsp</location>
	</error-page>

这表示HTTP状态码为400、404、500的时候，此次请求都会被转发到http://ip:port/工程名/filter/error.jsp这个页面上去。注意一下这里是error-code，所以如果是200的话，是没有效果的

### filter ###

走filter的顺序就是filter定义的顺序

### servlet ###

servlet开发者比较熟悉，先匹配规则，匹配到路径后走相应的Servlet类，就不说了。下面配一个相对不那么常用的，只是相对而已，这种servlet的写法很常见：

	<servlet>
	    <servlet-name>startUpServlet</servlet-name>
	    <servlet-class>com.xrq.servlet.StartUpServlet</servlet-class>
	    <init-param>
	        <param-name>Name</param-name>
	        <param-value>123</param-value>
	    </init-param>
	    <init-param>
	        <param-name>Age</param-name>
	        <param-value>456</param-value>
	    </init-param>
	    <load-on-startup>8</load-on-startup>
	</servlet>

这是一个启动servlet，表示容器启动的时候servlet启动，调用其init()方法，所以首先第一个标签load-on-start，分几点说：

- load-on-startup元素标记容器是否在启动的时候就加载这个servlet（实例化并调用其init方法）
- 它的值必须是一个整数，表示servlet应该被载入的顺序
- 当值为0或者大于0时，表示容器在应用启动时就加载并初始化这个servlet
- 当值小于0或者没有指定时，表示这个容器在该servlet被选择时才会去加载
- 正数值越小，该servlet的优先级就越高，应用启动时就越先加载
- 当值相同时，容器自己选择顺序来加载

所以，load-on-startup中配置了一个大于等于0的正整数时，该servlet可以当作一个普通的servlet来用，无非是这个servlet启动的时候会加载其init()方法罢了。

另外一个就是init-param了，表示一个键值对，只能在本servlet里面被使用，通过ServletConfig获取，StartUpServlet的写法是：

	public class StartUpServlet extends HttpServlet{
	    /**
	     * 序列化
	     */
	    private static final long serialVersionUID = 1L;
	    
	    public void init() throws ServletException
	    {
	        System.out.println("StartUpServlet.init()");
	        System.out.println("Name：" + getServletConfig().getInitParameter("Name"));
	        System.out.println("Age：" + getServletConfig().getInitParameter("Age"));
	    }
	    
	    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
	            throws ServletException, IOException
	    {
	        
	    }
	    
	    public void destroy()
	    {
	        System.out.println("StartUpServlet.destory()");
	    }
	}

servlet只能取到配置在自己的<servlet>标签内的<init-param>

### listener ###

listener即监听器，是Servlet的监听器，它可以监听客户端的请求、服务器端的操作等，在事情发生前后做一些必要的处理。通过监听器，可以自动激发一些操作，比如监听在线用户数量，下面就写一个监听用户数量的监听器，首先web.xml配置很简单：

	<listener>
	    <listener-class>com.xrq.listener.UserCounterListener</listener-class>
	</listener>

写一个监听器，监听用户数量一般都是以session创建和session失效为依据的，所以实现HttpSessionListener：
	
	public class UserCounterListener implements HttpSessionListener{
	    private AtomicInteger ai = new AtomicInteger(0);
	    
	    public void sessionCreated(HttpSessionEvent se)
	    {
	        ai.incrementAndGet();
	    }
	    
	    public void sessionDestroyed(HttpSessionEvent se)
	    {
	        ai.decrementAndGet();
	    }
	    
	    public int getUserCount()
	    {
	        return ai.get();
	    }
	}

除了监听session的监听器外，再介绍一些别的监听器接口：

#### ServletContextListener ####

用于监听WEB引用启动和销毁的事件，SevletContextListener是ServletContext的监听者，如果ServletContext发生变化，如服务器启动、服务器关闭，都会被ServletContextListener监听到。监听事件为ServletContextEvent

#### ServletContextAttributeListener ####

用于监听WEB应用属性改变的事件，包括添加属性、删除属性、修改属性。监听时间为ServletContextAttributeEvent

#### HttpSessionBindingListener ####

HttpSessionBindingListener是唯一一个不需要在web.xml中配置的Listener，当我们的类实现了HttpSessionBindListener接口后，只要对象加入session或者从session中移除，容器会分别自动调用以下两个方法：

 - void valueBound(HttpSesssionBindEvent event)
 - void valueUnbound(HttpSessionBindingEvent event)

**注意**，这个监听器的触发是针对于实现了该监听器的类的，只有把实现了该监听器的类set进session或从session中remove才会触发这个监听器

#### HttpSessionAttributeListener ####

用于监听HttpSession中的属性的操作，当session里面增加一个属性时，触发attributeAdded(HttpSessionBindEvent se)方法；当在session中删除一个属性时，触发attributeRemoved(HttpSessionBindEvent se)方法；当session属性被重新设置时，触发attributeReplaced(HttpSessionBindingEvent se)方法。

**注意**，这个监听器的触发是针对所有的session的，只要session的属性发生变化，都会触发这个监听器

#### HttpSessionListener ####

这个上面已经写过了，监听HttpSession。当创建一个session时，触发sessionCreated(HttpSessionEvent se)方法；当销毁一个session时，会触发sessionDestoryed(HttpSessionEvent se)方法

#### HttpSessionActivationListener ####

这个用得不太多，主要监听同一个session转移至不同的JVM的情形

#### ServletRequestListener和ServletRequestAttributeListener ####

和ServletContextListener和ServletContextAttributeListener类似，前者监听Request的创建和销毁、后者监听Request中属性的增删改


### context-param ###

context-param里面配置的键值对是全局共享的，整个web项目都能取到这个上下文，比方说我在web.xml里面配置了一个HTTP端口和一个HTTPS端口：

	<context-param>
	    <param-name>NotSSLPort</param-name>
	    <param-value>8080</param-value>
	</context-param>
	<context-param>
	    <param-name>SSLPort</param-name>
	    <param-value>8443</param-value>
	</context-param>

servlet可以这么取：

	protected void doPost(HttpServletRequest request, HttpServletResponse response)
	        throws ServletException, IOException
	{
	    System.out.println("NotSSLPort：" + getServletContext().getInitParameter("NotSSLPort"));
	    System.out.println("SSLPort：" + getServletContext().getInitParameter("SSLPort"));
	}

filter可以这么取：

	public void doFilter(ServletRequest request, ServletResponse response,
	        FilterChain chain) throws IOException, ServletException
	{
	    HttpServletRequest req = (HttpServletRequest)request;
	    ServletContext sc = req.getSession().getServletContext();
	    System.out.println("NotSSLPort：" + sc.getInitParameter("NotSSLPort"));
	    System.out.println("SSLPort：" + sc.getInitParameter("SSLPort"));
	    chain.doFilter(request, response);
	}

listener可以这么取，以ServletContextListener为例：

	public void contextInitialized(ServletContextEvent sce)
	{
	    System.out.println("Enter SCListener.contextInitialized");
	    System.out.println("NotSSLPort：" + sce.getServletContext().getInitParameter("NotSSLPort"));
	    System.out.println("SSLPort：" + sce.getServletContext().getInitParameter("SSLPort"));
	}

反正最终的目的就是取到一个ServletContext就对了。是不是感觉ServletContext很熟悉呢？没错，看一下jsp默认的内置对象，随便打开一个转换成Servlet的jsp页面，里面都有内置对象的定义：

	PageContext pageContext = null;
	HttpSession session = null;
	ServletContext application = null;
	ServletConfig config = null;
	JspWriter out = null;
	Object page = this;
	JspWriter _jspx_out = null;
	PageContext _jspx_page_context = null;

ServletContext也就是我们常说的Application

### <mime-mapping> ###

<mime-mapping>可能不太常见，这个标签是用来指定对应的格式的浏览器处理方式的，添加mime-type的映射，就可以避免某些类型的文件直接在浏览器中打开了。

举个例子：

	<mime-mapping>
	    <extension>doc</extension>
	    <mime-type>application/msword</mime-type>
	</mime-mapping>
	<mime-mapping>
	    <extension>pdf</extension>
	    <mime-type>application/pdf</mime-type>
	</mime-mapping>
	<mime-mapping>
	    <extension>rar</extension>
	    <mime-type>application/x-rar-compressed</mime-type>
	</mime-mapping>
	<mime-mapping>
	    <extension>txt</extension>
	    <mime-type>text/plain</mime-type>
	</mime-mapping>
	<mime-mapping>
	    <extension>xls</extension>
	    <mime-type>application/vnd.ms-excel</mime-type>
	</mime-mapping>

### session-config ###

session-config是用来配置session失效时间的，因为session-config里面只有一个子标签：

	<session-config>
    	<session-timeout>30</session-timeout>
	</session-config>

以分钟为单位。当然，代码里面也可以设置：

"request.getSession.setMaxInactiveInterval(30 * 60);"就可以了，单位为秒

### 元素加载顺序 ###

首先可以肯定，加载顺序与它们在web.xml文件中的先后顺序无关，即不会因为filter写在listener前面就先加载filter。最终得出的结论是listener->filter->servlet。

然后是context-param，用于向ServletContext提供键值对，即应用程序上下文信息，listener、filter、servlet都可能会用到这些上下文中的信息，那么context-param是否应该写在listener、filter、servlet前面呢？未必，context-param可以写在任何位置，因此真正的加载顺序应该是：context-param->listener->filter->servlet。