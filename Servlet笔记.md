---
title: 2018-3-18 Servlet笔记 
tags: Servlet,Java,
grammar_cjkRuby: true
---

## 一、 Servlet概述
Jsp的前身就是Servlet。
Servlet是在服务器上运行的小程序。一个Servlet就是一个Java类，并且可以通过  ==“请求-响应”== 编程模式来访问的这个驻留在服务器内存里的Servlet程序。

## 二、Tomcat容器等级
四个等级。 Containner，Engine，HOST，Servlet，Context（上下文）
Servlet容器管理Context容器，一个Context对应一个Web工程

## 三、Servlet程序编写
1.	创建一个自己的Servlet类，继承HttpServlet
2.	重写doGet（）或者doPost（）方法。具体重写哪个取决去提交请求的方式。
3.	在web.xml中注册Servlet
主要是以下两个标签
```html
<servlet-name>HelloServlet</servlet-name>
<servlet-class>servlet.HelloServlet</servlet-class>
```
4.	通过web页面就可以进行Servlet的访问

#### 代码实例
```java
/*
/* 自己编写一个Servlet类，继承HttpSelvet
 */
package servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletException;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

public class HelloServlet extends HttpServlet {

	@Override
	protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		System.out.println("处理get请求");//这句话输出在控制台
		resp.setCharacterEncoding("utf-8");//注意编码问题
		resp.setContentType("text/html;charset=UTF-8");
		PrintWriter out = resp.getWriter();
		out.println("<strong>欢迎光临</strong><br>");//打印在web页面
	}

	@Override
	protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
		super.doPost(req, resp);
	}

}

```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://xmlns.jcp.org/xml/ns/javaee" xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd" id="WebApp_ID" version="3.1">
  <display-name>ServletDemo</display-name>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>default.html</welcome-file>
    <welcome-file>default.htm</welcome-file>
    <welcome-file>default.jsp</welcome-file>
  </welcome-file-list>
  
  <servlet>
  	<servlet-name>HelloServlet</servlet-name>
  	<servlet-class>servlet.HelloServlet</servlet-class>
  </servlet>
  <servlet-mapping>
    <servlet-name>HelloServlet</servlet-name>
    <url-pattern>/servlet/HelloServlet</url-pattern>
  </servlet-mapping>
</web-ap
```
```html
<%@ page language="java" contentType="text/html; charset=utf-8" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<title>Insert title here</title>
</head>
<body>
	<h1>my first Servlet</h1>
	<hr>
	<a href="servlet/HelloServlet">get()请求 HelloServlet</a>
</body>
</html>
```

*注意事项：*

1. 记得转换编码为UTF-8
2. xml文件中的 servlet-class 和 url-pattern必须与index中的链接地址一样，指向servlet类的全地址，src为项目根目录.
3. 编写Servlet的doPost方法时，需要抛出ServletExcpetion和IOException异常，

## 四、Servlet执行流程

![enter description here][1]


  
  ## 五、Sevlet生命周期
  1.初始化   先执行构造方法，然后执行init(ServletConfig) 
  2. 加载。
  3. 服务。响应客户端请求
  4. 销毁。客户端关闭的时候，调用destroy()
  
  ![生命周期][2]
  
## 六、Tomcat装载Servlet三种情况
* Servlet容器启动时自动装在某些Servlet，实现它只需在web.xml文件中的`<Servlet></Servlet>` 添加如下代码：
                  `<loaden-startup>1</loaden-startup>` 
	   数字越小，表示优先级越高
* 在Servlet容器启动后，客户首次向Servlet发送请求
* 当Servlet文件被更新后，自动重新装载Servlet（Servlet被装载后，容器创建一个Servlet实例并且调用inti()进行初始化， ==inti()方法在Servlet整个生命周期中 只调用一次==，会长期保存在服务器内存中，直到客户端关闭。）


  
  ## 七、Servlet与JSP九大内置对象的对应关系
  ![enter description here][3]

*注意：out 与 resp.getWriter 类型不不一样，只是功能一样*
  
  ### Servlet实例——获取表单数据
  代码github地址：
  [https://github.com/Bosssheep/LearningProject/tree/master/ServletGetInfoDemo][4]
  遇到的错误以及解决方法：
>  Server Tomcat v8.0 Server at localhost failed to start.

有两种问题：1、xml写错了
                      2、一个新的servlet名字跟以前写过的重复了，也就是已经生成过了。
					 
查到一个小伙伴分享的方法非常便捷

[错误记录--server tomcat v8.0 server at localhost failed to start][5]

## 八、 Servlet路径跳转问题
### 跳转地址写法：
目录结构：
![enter description here][6]

`<a href="/servlet/HelloServlet"> ` 是错误的， 第一个/表示服务器的根目录
`<a href="servlet/HelloServlelt">`是正确的，用相对路径
还可以用绝对路径，如下：
```html
<%String path = request.getContextPath();%> <!--path表示项目根目录-->
<a href="<%=path%>+/servlet/HelloServlelt">  
```
**注意：** 而在xml文件中，地址却是必须带 “ / ”的，这里的/表示项目根目录
比如：`<url-pattern>/servlet/HelloSevlet</url-pattern>`

### Servlet重定向方式跳转到指定jsp

`response.sendRedirect("test.jsp");` 这种写法是错误的，指的是当前路径(Project/servlet/)下的test.jsp
`response.sendRedirect(request.getContextPath()+"/test.jsp");` 这样才是正确的。request.getContextPath()获取上下文对象

### 服务器内部跳转
`request.getRequestDispatcher("/test.jsp").forward(request,response);`这里的 / 表示项目根目录
`request.getRequestDispatcher("../test.jsp").forward(request,response);`

**总结 :
页面中的/代表服务器根目录，web.xml中/代表项目根目录，请求重定向的/代表服务器根目录，请求转发的/代表项目根目录**



### Sevlet Demo——一个登陆页面
demo地址：
[https://github.com/Bosssheep/LearningProject/tree/master/ServletLoginDemo][7]


  [1]: ./images/QQ%E6%88%AA%E5%9B%BE20180318101641.png "执行流程"
  [2]: ./images/QQ%E6%88%AA%E5%9B%BE20180318101944.png "生命周期"
  [3]: ./images/QQ%E6%88%AA%E5%9B%BE20180318104241.png "对应关系"
  [4]: https://github.com/Bosssheep/LearningProject/tree/master/ServletGetInfoDemo
  [5]: http://blog.csdn.net/love521334421/article/details/49004093
  [6]: ./images/QQ%E6%88%AA%E5%9B%BE20180318131910.png "目录结构"
  [7]: https://github.com/Bosssheep/LearningProject/tree/master/ServletLoginDemo