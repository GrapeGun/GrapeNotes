Servlet
===

###1.Tomcat容器等级
![Tomcat容器等级](http://img.mukewang.com/56382cd50001a43912800720.jpg)

###2.Servlet继承关系
+ Servlet (interface) 三个接口方法:Init() service() destory()
+ GenericServlet (Abstract Class 实现Servlet) 与协议无关的Servlet
+ HttpServlet(Abstract Class 继承GenericServlet) 实现了http协议的Servlet
+ 自定义Servlet(继承HttpServlet) 重写doGet() doPost() 方法

###3.编写步骤
+ 继承HttpServlet
+ 重写doGet() | doPost()
+ 在web.xml中注册Servlet

###4.生命周期
![Servlet生命周期](http://img.mukewang.com/563970f90001c1dc12800720.jpg)
+ 初始化阶段,调用init(ServletConfig) 方法,整个生命周期init()方法只调用一次 
+ 相应客户端请求,调用service(request,response)方法.由service()方法根据提交方式选择执行doGet | doPost
+ 销毁阶段 destroy()方法

###5.Tomcat容器加载Servlet的三种情况
+ Servlet容器启动时自动装载某些Servlet,在web.xml文件中的`<servlet></servlet>`标签内部添加`<loadon-startup>1</loadon-startup>`,数字越小加载优先级高
+ Servlet容器启动后,客户首次向Servlet发送请求
+ Servlet类文件被更新后,重新装载Servlet

###6.Servlet与JSP九大内置对象

| JSP对象 | Servlet获取方法| 
| --- | --- | 
| out | resp.getWriter()方法| 
| request | service()方法中的req参数| 
| response | service()方法中的resp参数| 
| session | request.getSession()方法| 
| application | getServletContext()方法| 
| exception | Throwable| 
| page | this| 
| pageContext | PageContext| 
| Config | getServletConfig()方法| 

###7.Servlet获取初始化参数
通过ServletConfig接口获取web.xml中的初始化参数.
如
```xml
<!--web.xml文件中的初始化参数-->
<servlet>
    ...
    <init-param>
        <param-name>username</param-name>
        <param-value>admin</param-value>
    </init-param>
    <init-param>
        <param_name>password</param_name>
        <param-value>12344555</param-value>
    </init-param>

</servlet>
```

在Servlet类中可以通过`getInitParameter("username");`可以获取初始化参数
