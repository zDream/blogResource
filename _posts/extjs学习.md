title: extjs学习
date: 1/30/2016 1:19:25 PM 
tags: Extjs
---

> 本文引自[李林峰的园子](http://www.cnblogs.com/iamlilinfeng/archive/2012/06/18/2553481.html)

> 注：本教程的讲解是基于ExtJs3.0版本[Ext3.0下载](http://7xpw00.com1.z0.glb.clouddn.com/imageExt3.0.rar),[Ext3.0API下载](http://7xpw00.com1.z0.glb.clouddn.com/imageExt3.0.API.rar)。

## 简介 ##


<a href="http://7xpw00.com1.z0.glb.clouddn.com/image%E5%89%8D%E7%AB%AF%E7%9F%A5%E8%AF%86.txt"></a>

> ExtJS是一种主要用于创建前端用户界面，是一个与后台技术无关的前端ajax框架。
功能丰富，无人能出其右
无论是界面之美，还是功能之强，ext的表格控件都高居榜首。
单选行，多选行，高亮显示选中的行，推拽改变列宽度，按列排序，这些基本功能咱们就不提了。
自动生成行号，支持checkbox全选，动态选择显示哪些列，支持本地以及远程分页，可以对单元格按照自己的想法进行渲染，这些也算可以想到的功能。
再加上可编辑grid，添加新行，删除一或多行，提示脏数据，推拽改变grid大小，grid之间推拽一或多行，甚至可以在tree和grid之间进行拖拽，啊，这些功能实在太神奇了。更令人惊叹的是，这些功能竟然都在ext表格控件里实现了。

## 什么是ext ##

> -  1、ExtJS可以用来开发RIA也即富客户端的AJAX应用，是一个用javascript写的，主要用于创建前端用户界面，是一个与后台技术无关的前端ajax框架。因此，可以把ExtJS用在.Net、Java、Php等各种开发语言开发的应用中。ExtJs最开始基于YUI技术，由开发人员JackSlocum开发，通过参考JavaSwing等机制来组织可视化组件，无论从UI界面上CSS样式的应用，到数据解析上的异常处理，都可算是一款不可多得的JavaScript客户端技术的精品。
> - 2、Ext的UI组件模型和开发理念脱胎、成型于Yahoo组件库YUI和Java平台上Swing两者，并为开发者屏蔽了大量跨浏览器方面的处理。相对来说，EXT要比开发者直接针对DOM、W3C对象模型开发UI组件轻松。

## Hello word ##

目录结构，把 Ext 文件夹放入你的项目中
![](http://7xpw00.com1.z0.glb.clouddn.com/imageQQ%E6%88%AA%E5%9B%BE20160130160124.png)

index.jsp代码

	
	<%@ page contentType="text/html;charset=UTF-8" language="java" %>
	<!--ExtJs框架开始-->
	<script type="text/javascript" src="/Ext/adapter/ext/ext-base.js"></script>
	<script type="text/javascript" src="/Ext/ext-all.js"></script>
	<link rel="stylesheet" type="text/css" href="/Ext/resources/css/ext-all.css"/>
	<!--ExtJs框架结束-->
	<script type="text/javascript">
	    Ext.onReady(function () {
	        Ext.MessageBox.alert('标题', 'Hello World!');
	    });
	</script>
	<html>
	<head>
	    <title></title>
	</head>
	<body>
	<!--
	18 说明：
	19 (1)Ext.onReady():ExtJS Application的入口...就相当于Java或C#的main函数.
	20 (2)Ext.MessageBox.alert()：弹出对话框。
	21 -->
	</body>
	</html>

## Extjs 窗体 ： Window组件 ##

### 代码如下 ###

	<%@ page contentType="text/html;charset=UTF-8" language="java" %>
	<!--ExtJs框架开始-->
	<script type="text/javascript" src="/Ext/adapter/ext/ext-base.js"></script>
	<script type="text/javascript" src="/Ext/ext-all.js"></script>
	<link rel="stylesheet" type="text/css" href="/Ext/resources/css/ext-all.css"/>
	  <!--ExtJs框架结束-->
	  <script type="text/javascript">
	    Ext.onReady(function () {
	      var win = new Ext.Window({
	        title: '窗口',
	        width: 476,
	        height: 374,
	        html: '<div>这里显示内容</div>',
	        resizable: true,
	        modal: true,
	        closable: true,
	        maximizable: true,
	        minimizable: true
	      });
	      win.show();
	    });
	  </script>
	</head>
	<body>
	<!--
	说明：
	(1)var win = new Ext.Window({}):创建一个新的Window窗体对象。
	(2)title: '窗口'：窗体的标题。
	(3)width: 476,height: 374：宽度及高度。
	(4)html: '<div>这里是窗体内容</div>'：窗体内部显示的html内容。
	(5)resizable: true：是否可以调整窗体的大小，这里设置为 true。
	(6)modal: true：是否为模态窗体[什么是模态窗体？当你打开这个窗体以后，如果不能对其他的窗体进行操作，那么这个窗体就是模态窗体，否则为非模态窗体]。
	(7)closable:true：是否可以关闭，也可以理解为是否显示关闭按钮。
	(8)maximizable: true：是否可以最大化，也可以理解为是否显示最大化按钮。
	(9)minimizable: true：是否可以最小化，也可以理解为是否显示最小化按钮。
	(10)win.show()：窗体展示。
	-->
	</body>
	</html>

### window 组件常用的：属性、方法及事件 ###

一、属性

plain：布尔类型，true表示强制与背景色保持协调，默认值为false。 
resizable：布尔类型，用户是否可以调整窗体大小，默认值为true表示可以调整大小。 
maxinizable：布尔类型，true表示显示最大化按钮，默认值为false。 
maximized：布尔类型，true表示显示窗体时将窗体最大化，默认值为false。 
closable：布尔类型，true表示显示关闭按钮，默认值为true。 
bodyStyle：与边框的间距，如：bodyStyle:"padding:3px"。 
buttonAlign：窗体中button的对齐方式(left、center、right)，默认值为right。 
closeAction："close"释放窗体所占内存，"hide"隐藏窗体，建议使用"hide"。

二、方法 

show：打开窗体。 
hide：隐藏窗体。 
close：关闭窗体。

三、事件 

show：打开窗体时触法。 
hide：隐藏窗体时触法。 
close：关闭窗体时触法。

## Extjs 表单 : FormPanel ##

### 代码如下 ###


	<%@ page contentType="text/html;charset=UTF-8" language="java" %>
	<html>
	<head>
	  <title></title>
	<!--ExtJs框架开始-->
	<script type="text/javascript" src="/Ext/adapter/ext/ext-base.js"></script>
	<script type="text/javascript" src="/Ext/ext-all.js"></script>
	 <link rel="stylesheet" type="text/css" href="/Ext/resources/css/ext-all.css" />
	<!--ExtJs框架结束-->
	      <script type="text/javascript">
	      Ext.onReady(function () {
	            var form = new Ext.form.FormPanel({
	              frame: true,
	             title: '表单标题',
	              style: 'margin:10px',
	                html: '<div style="padding:10px">这里表单内容</div>'
	           });
	          var win = new Ext.Window({
	                title: '窗口',
	                width: 476,
	               height: 374,
	                html: '<div>这里是窗体内容</div>',
	               resizable: true,
	               modal: true,
	               closable: true,
	               maximizable: true,
	               minimizable: true,
	                items: form
	           });
	           win.show();
	        });
	    </script>
	</head>
	<body>
	<!--
	36 说明：
	37 (1)var form = new Ext.form.FormPanel({}):创建一个新的form表单对象。
	38 (2)title: '表单标题'：表单的标题，如果不加的话，不会出现上面的浅色表单标题栏。
	39 (3)style: 'margin:10px':表单的样式，加了个外10px的外边距。
	40 (4)html: '<div style="padding:10px">这里表单内容</div>'：表单内显示html的内容。
	41 -->
	</body>
	</html>

### form 组件常用的：属性、方法及事件 ###

一、属性

width:整型，表单宽度。  
height:整型，表单高度。  
url:字符串，表单提交地址。

二、方法

reset:表单重置。  
isValid:表单是否验证全部通过。  
submit:表单提交。

## Extjs 文本框 : TextField ##

### 代码如下 ###

	<%@ page contentType="text/html;charset=UTF-8" language="java" %>
	<html>
	<head>
	    <title></title>
	    <!--ExtJs框架开始-->
	    <script type="text/javascript" src="/Ext/adapter/ext/ext-base.js"></script>
	    <script type="text/javascript" src="/Ext/ext-all.js"></script>
	    <link rel="stylesheet" type="text/css" href="/Ext/resources/css/ext-all.css"/>
	    <!--ExtJs框架结束-->
	    <script type="text/javascript">
	        Ext.onReady(function () {
	            //初始化标签中的Ext:Qtip属性。
	            Ext.QuickTips.init();
	            Ext.form.Field.prototype.msgTarget = 'side';
	            //用户名input
	            var txtusername = new Ext.form.TextField({
	                width: 140,
	                allowBlank: false,
	                maxLength: 20,
	                name: 'username',
	                fieldLabel: '用户名称',
	                blankText: '请输入用户名',
	                maxLengthText: '用户名不能超过20个字符'
	            });
	            //密码input
	            var txtpassword = new Ext.form.TextField({
	                width: 140,
	                allowBlank: false,
	                maxLength: 20,
	                inputType: 'password',
	                name: 'password',
	                fieldLabel: '密码',
	                blankText: '请输入密码',
	                maxLengthText: '密码不能超过20个字符'
	            });
	            //表单
	            var form = new Ext.form.FormPanel({
	                frame: true,
	                title: '表单标题',
	                style: 'margin:10px',
	                html: '<div style="padding:10px">这里表单内容</div>',
	                items: [txtusername, txtpassword]
	            });
	            //窗体
	            var win = new Ext.Window({
	                title: '窗口',
	                width: 476,
	                height: 374,
	                html: '<div>这里是窗体内容</div>',
	                resizable: true,
	                modal: true,
	                closable: true,
	                maximizable: true,
	                minimizable: true,
	                items: form
	            });
	            win.show();
	        });
	    </script>
	</head>
	<body>
	<!--
	说明：
	(1)Ext.QuickTips.init()：QuickTips的作用是初始化标签中的Ext:Qtip属性，并为它赋予显示提示的动作。
	(2)Ext.form.Field.prototype.msgTarget = 'side'：TextField的提示方式为：在右边缘，如上图所示，参数枚举值为"qtip","title","under","side",id(元素id)，
	   side方式用的较多，右边出现红色感叹号，鼠标上去出现错误提示。
	(3)var txtusername = new Ext.form.TextField():创建一个新的TextField文本框对象。
	(4)allowBlank: false：不允许文本框为空。
	(5)maxLength: 20：文本框的最大长度为20个字符，当超过20个字符时仍然可以继续输入，但是Ext会提示警告信息。
	(6)name: 'password'：表单名称，这个比较重要，因为我们在与服务器交互的时候，服务端是按name接收post的参数值。
	(7)fieldLabel: '用户名'：文本框前面显示的文字标题，如“用户名”，“密码”等。
	(8)blankText: '请输入用户名'：当非空校验没有通过时的提示信息。
	(9) maxLengthText: '用户不能超过20个字符'：当最大长度校验没有通过时的提示信息。
	-->
	</body>
	</html>

### textfield组件常用的：属性、方法及事件 ###

一、属性

allowBlank：是否允许为空，默认为true
blankText：空验证失败后显示的提示信息
emptyText：在一个空字段中默认显示的信息
grow：字段是否自动伸展和收缩，默认为false
growMin：收缩的最小宽度
growMax：伸展的最大宽度
inputType：字段类型：默认为text
maskRe：用于过滤不匹配字符输入的正则表达式
maxLength：字段允许输入的最大长度
maxLengthText：最大长度验证失败后显示的提示信息
minLength：字段允许输入的最小长度
minLengthText：最小长度验证失败后显示的提示信息

## Extjs 按钮 ： Button ##

### 代码如下 ###

	<%@ page contentType="text/html;charset=UTF-8" language="java" %>
	<html>
	<head>
	  <title></title>
	  <!--ExtJs框架开始-->
	  <script type="text/javascript" src="/Ext/adapter/ext/ext-base.js"></script>
	  <script type="text/javascript" src="/Ext/ext-all.js"></script>
	  <link rel="stylesheet" type="text/css" href="/Ext/resources/css/ext-all.css" />
	  <!--ExtJs框架结束-->
	  <script type="text/javascript">
	    Ext.onReady(function () {
	      //初始化标签中的Ext:Qtip属性。
	      Ext.QuickTips.init();
	      Ext.form.Field.prototype.msgTarget = 'side';
	      //提交按钮处理方法
	      var btnsubmitclick = function () {
	        Ext.MessageBox.alert('提示', '你点了确定按钮!');
	      }
	      //重置按钮"点击时"处理方法
	      var btnresetclick = function () {
	        Ext.MessageBox.alert('提示', '你点了重置按钮!');
	      }
	      //重置按钮"鼠标悬停"处理方法
	      var btnresetmouseover = function () {
	        Ext.MessageBox.alert('提示', '你鼠标悬停在重置按钮之上!');
	      }
	      //提交按钮
	      var btnsubmit = new Ext.Button({
	        text: '提交',
	        handler: btnsubmitclick
	      });
	      //重置按钮
	      var btnreset = new Ext.Button({
	        text: '重置',
	        listeners: {
	          'mouseover': btnresetmouseover,
	          'click': btnresetclick
	        }
	      });
	      //用户名input
	      var txtusername = new Ext.form.TextField({
	        width: 140,
	        allowBlank: false,
	        maxLength: 20,
	        name: 'username',
	        fieldLabel: '用户名称',
	        blankText: '请输入用户名',
	        maxLengthText: '用户名不能超过20个字符'
	      });
	      //密码input
	      var txtpassword = new Ext.form.TextField({
	        width: 140,
	        allowBlank: false,
	        maxLength: 20,
	        inputType: 'password',
	        name: 'password',
	        fieldLabel: '密码',
	        blankText: '请输入密码',
	        maxLengthText: '密码不能超过20个字符'
	      });
	      //表单
	      var form = new Ext.form.FormPanel({
	        frame: true,
	        title: '表单标题',
	        style: 'margin:10px',
	        html: '<div style="padding:10px">这里表单内容</div>',
	        items: [txtusername, txtpassword],
	        buttons: [btnsubmit, btnreset]
	      });
	      //窗体
	      var win = new Ext.Window({
	        title: '窗口',
	        width: 476,
	        height: 374,
	        html: '<div>这里是窗体内容</div>',
	        resizable: true,
	        modal: true,
	        closable: true,
	        maximizable: true,
	        minimizable: true,
	        buttonAlign: 'center',
	        items: form
	      });
	      win.show();
	    });
	  </script>
	</head>
	<body>
	<!--
	说明：
	(1)var btnsubmit = new Ext.Button():创建一个新的Button按钮对象。
	(2)handler: btnsubmitclick：当用户点击的时候[即js中的onclick事件]执行方法btnsubmitclick。
	(3)listeners: {'mouseover': btnresetmouseover,'click': btnresetclick}：当用户点击的时候[即js中的onclick事件]执行方法btnresetclick，
	    鼠标悬停时执行方法btnresetmouseover。
	(4)handler与listeners的区别：
	    handler:执行的是首发事件，click是button这个组件的首发事件。这就是handler的运行方式：被某个组件的首要event所触发。
	            handler是一个特殊的listener。
	    listener：是一个事件名 + 处理函数的组合，事件监听，如上例代码所示，我们监听了两个事件"click"，与"mouseover"事件，并且会顺序执行。
	-->
	</body>
	</html>

### button组件常用的：属性、方法及事件 ###

一、属性

text:字符串，显示在按钮上的文字。  
minWidth: 整型，最小宽度。

二、事件

handler:首发方法处理事件。  
listeners:事件监听。

## Extjs 登录窗体：login ##

### 代码如下 ###

	<%--
	  Created by IntelliJ IDEA.
	  User: Dreamer
	  Date: 2016/1/30
	  Time: 18:11
	  To change this template use File | Settings | File Templates.
	--%>
	<%@ page contentType="text/html;charset=UTF-8" language="java" %>
	<html>
	<head>
	  <title></title>
	  <!--ExtJs框架开始-->
	  <script type="text/javascript" src="/Ext/adapter/ext/ext-base.js"></script>
	  <script type="text/javascript" src="/Ext/ext-all.js"></script>
	  <link rel="stylesheet" type="text/css" href="/Ext/resources/css/ext-all.css" />
	  <style type="text/css">
	    .loginicon
	    {
	      background-image: url(image/login.gif) !important;
	    }
	  </style>
	  <!--ExtJs框架结束-->
	  <script type="text/javascript">
	    Ext.onReady(function () {
	      //初始化标签中的Ext:Qtip属性。
	      Ext.QuickTips.init();
	      Ext.form.Field.prototype.msgTarget = 'side';
	      //提交按钮处理方法
	      var btnsubmitclick = function () {
	        if (form.getForm().isValid()) {
	          //通常发送到服务器端获取返回值再进行处理，我们在以后的教程中再讲解表单与服务器的交互问题。
	          Ext.Msg.alert("提示", "登陆成功!");
	        }
	      }
	      //重置按钮"点击时"处理方法
	      var btnresetclick = function () {
	        form.getForm().reset();
	      }
	      //提交按钮
	      var btnsubmit = new Ext.Button({
	        text: '提 交',
	        handler: btnsubmitclick
	      });
	      //重置按钮
	      var btnreset = new Ext.Button({
	        text: '重 置',
	        handler: btnresetclick
	      });
	      //用户名input
	      var txtusername = new Ext.form.TextField({
	        width: 140,
	        allowBlank: false,
	        maxLength: 20,
	        name: 'username',
	        fieldLabel: '用户名',
	        blankText: '请输入用户名',
	        maxLengthText: '用户名不能超过20个字符'
	      });
	      //密码input
	      var txtpassword = new Ext.form.TextField({
	        width: 140,
	        allowBlank: false,
	        maxLength: 20,
	        inputType: 'password',
	        name: 'password',
	        fieldLabel: '密　码',
	        blankText: '请输入密码',
	        maxLengthText: '密码不能超过20个字符'
	      });
	      //验证码input
	      var txtcheckcode = new Ext.form.TextField({
	        fieldLabel: '验证码',
	        id: 'checkcode',
	        allowBlank: false,
	        width: 76,
	        blankText: '请输入验证码！',
	        maxLength: 4,
	        maxLengthText: '验证码不能超过4个字符!'
	      });
	      //表单
	      var form = new Ext.form.FormPanel({
	        url: '******',
	        labelAlign: 'right',
	        labelWidth: 45,
	        frame: true,
	        cls: 'loginform',
	        buttonAlign: 'center',
	        bodyStyle: 'padding:6px 0px 0px 15px',
	        items: [txtusername, txtpassword, txtcheckcode],
	        buttons: [btnsubmit, btnreset]
	      });
	      //窗体
	      var win = new Ext.Window({
	        title: '用户登陆',
	        iconCls: 'loginicon',
	        plain: true,
	        width: 276,
	        height: 174,
	        resizable: false,
	        shadow: true,
	        modal: true,
	        closable: false,
	        animCollapse: true,
	        items: form
	      });
	      win.show();
	      //创建验证码
	      var checkcode = Ext.getDom('checkcode');
	      var checkimage = Ext.get(checkcode.parentNode);
	      checkimage.createChild({
	        tag: 'img',
	        src: 'image/checkcode.gif',
	        align: 'absbottom',
	        style: 'padding-left:23px;cursor:pointer;'
	      });
	    });
	  </script>
	</head>
	<body>
	<!--
	说明：
	(1)88行，iconCls: 'loginicon':给窗体加上小图标，样式在第12行定义。
	(2)Ext.getDom('checkcode')：根据ID获取Dom。
	(3)Ext.get(checkcode.parentNode)：根据Dom获取父节点。
	(4)checkimage.createChild()：创建子节点，标签为<img src='image/checkcode.gif'..../>。
	(5)form.getForm().isValid()：校验表单的验证项是否全部通过。
	(6)form.getForm().reset()：重置表单。
	-->
	</body>
	</html>


