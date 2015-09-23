Spring MVC 学习指南
===
关于SpringMVC的一些杂七杂八的东西,并不记录详细内容.这本书的示例下载:[Spring MVC 学习指南 下载](http://books.brainysoftware.com/download).数据绑定的部分在SpringMvc(1)中记录了,感谢[博客园Miss When...](http://www.cnblogs.com/liukemng/)
---

###一、WEB基础
#####1.HTTP的URL格式
`protocol://[host.]domain[:port][/context][/resource][?query string | path variable]`
或者使用IP Address的形式.这里,有必要对其中URL中的几个部分做一个说明.

+ context部分用来代表应用名称,一台WEB服务器可以运行多个上下文(应用),其中一个可以配置为默认上下文.
+ 一个context可以有一个或多个默认资源(一般为index.html或者default.htm或index.htm),当存在多个默认资源时,优先级最高的将会返回给客户端
+ 资源名后是查询语句(如百度那样的)或者路径参数(如分页)查询语句一般是Key/Value组,多个查询语句间用`&`分割,路径参数只有value部分,多个value用`\`分割

#####2.HTTP请求
一个请求包含三个部分 ***方法-URI-协议/版本***、***请求头信息***、***请求正文***。

示例：(这里使用了一个表格,当然实际中是没有的...)
```
POST /***/***.jsp HTTP/1.1   ***方法-URI-协议/版本***
Accept:***
Accept-Language:***
Connection:***
Host:***
User-Agent:***
Content-Length:***
Content-Type:***
Accept-Encoding:***

lastName=Bob&firstName=Mike     ***请求正文,与头信息之间空一行***
```

#####3.HTTP响应
同样包含三个部分: ***协议-状态码-描述***,***响应头信息***,***响应正文***
```
HTTP/1.1 200 OK     200为响应成功 OK描述
Server:
Date:
Content-Type:
Last-Modified:
Content-Length:

<html>
  <head>
    <body>
      Welcome!
    </body>
  <head>
</html>
```

#####4.关于Servlet
Servlet技术室java体系中开发Web应用的根本,Servlet是 **运行在Servlet容器中的Java程序,Servlet容器或Servlet引擎相当于一个Web服务器**

---

###二、Spring基础
#####1.依赖注入
注入依赖作为代码可测试性的一个解决方案已经被广泛应用,看完本书后,我才对它有了一个更清晰的理解,简单来说:

组件A与B,A依赖于B(可以简单理解为A中的某个方法需要使用B的实例),若B是一个实例类,那么很简单,直接new一个对象即可.

>倘若B是接口,具有多个实现,当然可以在A中使用B的某一个实现类,然后采用上述方法,
但这样也就造成了A的重用性降低,因为A不能够采用B的其它实现.

依赖注入采取的方法是: **接管对象的创建工作,即依赖注入框架分别创建对象A与B,将B对象注入到A中**.

其实看到这里,我还是不太明白,怎么个把B **注入**到A中.接下来就是注入的实现方式了:

1.setter
```java
// Spring首先创建B实现类的对象b,再创建A,通过setter将b注入
public void setB(B b){ // 这样在使用B的方法中就不用操心B的实现类的问题了
    this.b = b;
}
```
2.构造方法
```java
public A(B b){
    this.b = b;
}
```
3.基于field方式
2.5版本后,通过Autowired注解依赖注入(缺点是会对Spring产生依赖)