---
layout: post
title: SpringMVC入门
category: Web
tags: SpringMVC
keywords: SpringMVC
description: 
---

##创建SpringMVC项目##

步骤1：创建Web项目

步骤2：在Web.xml中配置SpringMVC核心组件**DispatcherServlet**
    
    <servlet>
	    <servlet-name>dispatcher</servlet-name><!--配置servlet名称-->
	    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
	    <init-param><!--参数初始化-->
	      <param-name></param-name><!--参数名（对应该servlet内部成员变量）-->
	      <param-value></param-value><!--参数值-->
	    </init-param>
	    <load-on-startup>1</load-on-startup><!--表示容器启动时初始化该Servlet-->
    </servlet>
    <servlet-mapping>
	    <servlet-name>dispatcher</servlet-name>
	    <url-pattern>/</url-pattern><!--“/” 是用来定义默认servlet映射的。也可以如“*.html”表示拦截所有以html为扩展名的请求。(可以自由设定)-->
    </servlet-mapping>

步骤3：根据`步骤2`配置，接下来配置`[DispatcherServlet的Servlet名字]-servlet.xml`配置文件，本例中文件名`dispatcher-servlet.xml`

	<context:component-scan base-package="com.springmvc"/><!--设置扫描路径，以识别该路径下的如：@Controller, @RestController等注解-->
    <mvc:annotation-driven/><!--启用注解驱动，相当于注册了DefaultAnnotationHandlerMapping和AnnotationMethodHandlerAdapter两个bean-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver"><!--视图解析器-->
        <property name="prefix" value="/WEB-INF/pages/"/><!--prefix是视图文件的前缀，即目录名-->
        <property name="suffix" value=".jsp"/><!--suffix是视图文件的后缀，即扩展名，如使用JSP文件，则为“.jsp”-->
    </bean>

步骤4：创建控制器实体类`HelloController.java`

	package com.springmvc;
	import org.springframework.stereotype.Controller;
	import org.springframework.ui.ModelMap;
	import org.springframework.web.bind.annotation.RequestMapping;
	import org.springframework.web.bind.annotation.RequestMethod;
	
	@Controller
	@RequestMapping("/HelloController")
	public class HelloController {
	    @RequestMapping(method = RequestMethod.GET) //设置请求方式为Get
	    public String printWelcome(ModelMap model) {
	        model.addAttribute("message", "Hello lk!");//ModelMap对象主要用于传递控制方法处理数据到结果页面
	        return "Hello"; //跳转到视图页面/WEB-INF/pages/Hello.jsp。（即URL= prefix前缀+视图名称 +suffix后缀组成）
			//return "/WEB-INF/pages/Hello.jsp"; xml中不加前/后缀，按照此种方式也可以找到对应视图文件
	    }
	}

步骤5：创建`Hello.jsp`，接收来自控制器返回的结果

	<%@ page contentType="text/html;charset=UTF-8" language="java" %>
	<html>
		<head>
		    <title>Title</title>
		</head>
		<body>
			<P>${message}</P><!--获取HelloController返回的数据处理结果-->
		</body>
	</html>

步骤6：结果展示。输入：`Http://localhost:8080/HelloController`。页面显示`Hello lk!`字符串。

#内容扩展1：接收路径参数#

路径参数：将参数写在请求路径中传递。可以用来做伪静态

步骤1：在上述`HelloController.java`文件中添加如下方法：

	@RequestMapping(value ="/page/{name}/{age}" method = RequestMethod.GET) //准备通过请求路径传递参数name和age
    public String getWebPathParam(ModelMap model, @PathVariable("name") String name, @PathVariable("age") int age) {
        model.addAttribute("name", name);
		model.addAttribute("age", age);
        return "demo1"; 
    }

步骤2：创建对应的`demo1.jsp`页面

	<%@ page contentType="text/html;charset=UTF-8" language="java" %>
	<html>
		<head>
		    <title>Title</title>
		</head>
		<body>
			姓名： ${name}<br/>
			年龄： ${age}
		</body>
	</html>

步骤3：在浏览器中输入：`Http://localhost:8080/HelloController/page/xingming/nianling`

#内容扩展2：接收url参数#

接收url参数：按照正常get、post请求接收参数

步骤1：在上述`HelloController.java`文件中添加如下方法：

	@RequestMapping(value ="/demo2" method = RequestMethod.GET) //准备通过请求路径传递参数name和age
    public String getWebUrlParam(ModelMap model, @RequestParam String name, @RequestParam int age) {
        model.addAttribute("name", name);
		model.addAttribute("age", age);
        return "demo2"; 
    }

步骤2：创建对应的`demo2.jsp`页面

	<%@ page contentType="text/html;charset=UTF-8" language="java" %>
	<html>
		<head>
		    <title>Title</title>
		</head>
		<body>
			姓名： ${name}<br/>
			年龄： ${age}
		</body>
	</html>

步骤3：在浏览器中输入：`Http://localhost:8080/HelloController/demo2?name=xingming&age=nianling`

#内容扩展3：接收表单数据#

接收表单参数：获取html中表单提交数据，响应请求

步骤1：在上述`HelloController.java`文件中添加如下方法：

	@RequestMapping(value ="/demo3" method = RequestMethod.GET) 
    public String addUser(ModelMap model) {

        return "demo3"; 
    }

步骤2：创建对应的`demo3.jsp`页面

	<%@ page contentType="text/html;charset=UTF-8" language="java" %>
	<html>
		<head>
		    <title>Title</title>
		</head>
		<body>
			<form action="demo2", method="get"><!--demo2进行页面跳转响应，get或post请求都可以-->
				姓名：<input type="text" name="name"><br>
				年龄：<input type="text" name="age"><br>
				<input type="submit">
			</form>
		</body>
	</html>

步骤3：在浏览器中输入：`Http://localhost:8080/HelloController/demo3`

步骤4：这时候会出现demo3表单，在两个输入框分别输入姓名和年龄，`submit`提交，页面跳转到`demo2`。浏览器地址变`demo2`的请求地址

步骤5：学习使用SpringMVC表单。将`demo3.jsp`页面改成SpringMVC表单，如下：

	<%@ page contentType="text/html;charset=UTF-8" language="java" %>
	<html>
		<head>
		    <title>Title</title>
		</head>
		<body>
			<form：form action="demo2", method="get" modelAttribute="user"><!--modelAttribute,准备从ModelMap中获取对应user对象，进行默认值填充-->
				姓名：<form：input path="name"/><br>
				年龄：<form：input path="age"/><br>
				<input type="submit"/>
			</form：form>
		</body>
	</html>

步骤6：创建`User.java`文件，如下：
	public class User {
	
		private String name = "小名"	；
		
		private int age = 20;
		
		public String getName() {
			return name；
		}

		public void setName(String name) {
			this.name = name；
		}

		public String getAge() {
			return age；
		}

		public void setAge(int age) {
			this.age = age;
		}

	}

步骤6：修改`HelloController.java`中`addUser`方法:
	
	@RequestMapping(value ="/demo3" method = RequestMethod.GET) 
    public String addUser(ModelMap model) {
		User user = new User;
		user.setName("小红")；
		//这里不对age进行设置，让页面使用默认值：20
		mode1.addAttribute("user", user);
        return "demo3"; 
    }

步骤7：在浏览器中输入：`Http://localhost:8080/HelloController/demo3`，可以看到存在默认填充
