SpringMVC(一)
===
###一、SpringMVC初识
SpringMVC 具有基于注解的controller（其action的声明据说与.net MVC相似）。它属于SpringFrameWork的后续产品，提供了构建 Web 应用程序的全功能 MVC 模块，与Struts2一样是一种优秀MVC框架，不同的是自Spring2.5引入了注解式controller及Spring 3以后的不断完善，使得采用SpringMVC框架开发结构清晰明了，效率大大提高。

![SpringMVC 响应流程](http://images.cnitblog.com/blog/205051/201405/131204110311354.jpg)

1. 用户请求通过`DispatcherServlet`到对应的**注解映射处理器**
2. 注解映射器`DefaultAnnotationHandlerMapping`找到对应的Controller
3. XXController返回对应的Controller到`DispatcherServlet` 
4. `DispatcherServlet`通过**方法适配器**调用XXController对应的方法
5. 方法适配器`AnnotationMethodHandlerAdapter`返回`ModelAndView`到`DispaterServlet`
6. `DispatcherServlet`通过`ViewResolver`解析视图
7. `ViewResolver`将具体的View返回到`DispatcherServlet`并响应用户请求

---
###二、创建一个HelloWorld项目

#####1. 准备

将相关jar包导入到项目中/WebRoot/WEB-INF/lib目录中.(springframework改版后使用maven来管理jar包,这里先给出了一个完整的下载链接)

spring-framework下载:[spring-framework](http://repo.springsource.org/libs-release-local/org/springframework/spring/)

#####2. 配置web.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="2.5" 
 xmlns="http://java.sun.com/xml/ns/javaee" 
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" 
 xsi:schemaLocation="http://java.sun.com/xml/ns/javaee 
 http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd">
 
    <welcome-file-list>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>

    <servlet>
      <servlet-name>SpringMVCDemo</servlet-name>
      <servlet-class>
        org.springframework.web.servlet.DispatcherServlet
      </servlet-class>
      <init-param>
        <param-name>contextConfigLocation</param-name>
        <!--指定springmvc-servlet的配置文件 -->
        <param-value>classpath:springmvc-servlet.xml</param-value>
      </init-param>
      <load-on-startup>1</load-on-startup> <!--load-on-start必须放在最后 -->
    </servlet>
    <!--Spring MVC配置文件结束-->
    <!-- 下面指定配置的是哪个servelet -->
    <servlet-mapping>
        <servlet-name>SpringMVCDemo</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>
</web-app>
```

#####3. springmvc-serlet.xml配置示例

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:p="http://www.springframework.org/schema/p" 
  xmlns:context="http://www.springframework.org/schema/context"
  xmlns:util="http://www.springframework.org/schema/util"
  xmlns:mvc="http://www.springframework.org/schema/mvc"
  xsi:schemaLocation="
  http://www.springframework.org/schema/beans
  http://www.springframework.org/schema/beans/spring-beans.xsd
  http://www.springframework.org/schema/util
  http://www.springframework.org/schema/util/spring-util.xsd
  http://www.springframework.org/schema/context 
  http://www.springframework.org/schema/context/spring-context.xsd
  http://www.springframework.org/schema/mvc
  http://www.springframework.org/schema/mvc/spring-mvc.xsd" >
  
  <!-- 默认的注解映射的支持 -->
  <mvc:annotation-driven/>
  <!-- 当请求路径是/时,转到/hellowrod/index -->
  <!-- helloworld对应相应的HelloWorldController,index对应相应的methord -->
  <mvc:view-controller path="/" view-name="forward:/helloworld/index"/>
  <!-- 开启Controller的注解支持 -->
  <!-- use-default-filters="fasle" 只扫描指定的注解 -->
  <context:component-scan base-package="com.grapegun.spring.controllers"  use-default-filters="false">
    <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
  </context:component-scan>
  <!-- 视图解析器 -->
  <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
   <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
   <property name="contentType" value="text/html"/>
   <property name="prefix" value="/WEB-INF/views/"/>
   <property name="suffix" value=".jsp"/>
  </bean>
  
</beans>
```

#####4. HelloWorldController类的实现

```java
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestMethod;
import org.springframework.web.servlet.ModelAndView;

/**
 * @description Controller HelloWorld
 * @author FGN
 * @date 2015-9-15 下午04:46:46
 */
@Controller
@RequestMapping(value="/helloworld") // 注解通过RequestMapping找到该Controller
public class HelloWorldController {
  @RequestMapping(value="/index",method={RequestMethod.GET})
  public ModelAndView index(){
    ModelAndView modelAndView = new ModelAndView();
    modelAndView.addObject("message","Hello World !"); // 此处类似与Androi中Intent
    modelAndView.setViewName("index");
    return modelAndView;    
  }  
}

```

#####5. view

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
<!-- 从controller中的ModelAndView中获取 -->
    ${message} 
</body>
</html>
```

---
###三、URL到Antion的映射规则


---
###四、数据绑定

#####1. 常用的数据绑定的注解一览

  + @RequestParam: 绑定单个请求数据,可以是URL中的数据,表单提交的数据或者上传的文件
  + @PathVarialbe: 绑定URL模板变量值
  + @CookieValue:绑定Coolie数据
  + @RequestHeader:绑定请求头数据
  + @ModelAttributes:绑定数据到Model
  + @SessionAttributes:绑定数据到Session
  + @RequestBody & @RequestPart

#####2. ***@RequestParam***

>将数据绑定在单个对象

在之前的Demo中,设置springmvc-servlet.xml,设置方法如下,并导入支持文件上传的包(commons-fileupload-1.3.1.jar和commons-io-2.4.jar).

```xml
<!-- 支持上传文件 -->  
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">  
    <!-- 设置上传文件的最大尺寸为1MB -->  
    <property name="maxUploadSize">  
        <value>1048576</value>  
    </property>
    <property name="defaultEncoding"> 
        <value>UTF-8</value> 
    </property>
</bean>
```

添加输入表单jsp

```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>parambind</title>
</head>
<body>
   <form action="parambind?urlParam=AAA" method="post" enctype="multipart/form-data">
     <input type="text" name="formParam"/><br/>
     <input type="file" name="formFile"/><br/>
     <input type="submit" value="Submit"/>"
   </form>
</body>
</html>

```

然后在Controller中添加DataBindController

```java
@Controller
@RequestMapping(value="/databind")
public class DataBindController {
  /**
   * 填写信息
   * @return
   */
  @RequestMapping(value="/parambind",method={RequestMethod.GET})
  public ModelAndView paramBind(){
    ModelAndView modelAndView = new ModelAndView();
    modelAndView.setViewName("parambind");
    return modelAndView;
  }
  
  /**
   * 参数列表中使用的是注解自动绑定数据
   * 代码中则使用手动获取数据
   */
  @RequestMapping(value = "/parambind", method = { RequestMethod.POST })
  public ModelAndView paramBind(HttpServletRequest request,
      @RequestParam("urlParam") String urlParam,
      @RequestParam("formParam") String formParam,
      @RequestParam("formFile") MultipartFile formFile) {
    String urlParam1 = ServletRequestUtils.getStringParameter(request,
        "urlParam", null);
    String formParam1 = ServletRequestUtils.getStringParameter(request,
        "formParam1", null);
    MultipartFile formFile1 = ((MultipartHttpServletRequest) request)
        .getFile("formFile");

    ModelAndView modelAndView = new ModelAndView();
    // 自动
    modelAndView.addObject("urlParam", urlParam);
    modelAndView.addObject("formParam", formParam);
    modelAndView.addObject("formFile", formFile.getOriginalFilename());
    // 手动
    modelAndView.addObject("urlParam1", urlParam1);
    modelAndView.addObject("formParam1", formParam1);
    modelAndView.addObject("formFile1", formFile1.getOriginalFilename());
    modelAndView.setViewName("parambindresult");

    return modelAndView;
  }
  
}
```

之后在JSP页面中用EL表达式输出.
```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>parambindresult</title>
</head>
<body>  
  自动
  ${urlParam}
  ${formParam}
  ${formFile}
  手动
  ${urlParam1}
  ${formParam1}
  ${formFile1}  
</body>
</html>
```

> 将数据绑定到model对象

将model加入到相应的action参数.此处首先创建一个AccountModel作为测试用Model,该Model中有两个属性userName与password,分别为他们设置setter与getter.

在DataBindController中添加填写表单的Action
```java
@RequestMapping(value = "/modelautobind", method = { RequestMethod.GET })
public String modelAutoBind(HttpServletRequest request, Model model) {
  // 参数列表中,Model是必须的
  // 在显示modelautobind.jsp之前,初始一个AccountModel实例
  // 注意该实例的名称为accountmodel
  model.addAttribute("accountmodel", new AccountModel()); 
  return "modelautobind";
}
```
在jsp页面中使用spring表单,具体代码如下
```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<!-- 使用spring form表单 -->
<%@ taglib prefix="form" uri="http://www.springframework.org/tags/form" %>
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
    <!-- 此处modelAttribute的值与Action中创建的Attribute一致 -->
    <form:form modelAttribute="accountmodel" method="post">     
        用户名：<form:input path="userName"/><br/>
        密 码：<form:password path="password"/><br/>
        <input type="submit" value="Submit" />
    </form:form>  
</body>
</html>
```

接下来用户填写完成后提交表单,Controller中相应的post方法的Action
```java
@RequestMapping(value = "/modelautobind", method = { RequestMethod.POST })
public String modelAutoBind(HttpServletRequest request, Model model,
    AccountModel accountModel) {
  // 方法的参数中,必须要有Model
  model.addAttribute("accountmodel", accountModel);
  return "modelautobindresult";
}
```

该Action会返回到modelautobinderesult.jsp界面
```html
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
    用户名：${accountmodel.userName}<br/>
    密 码：${accountmodel.password}
</body>
</html>

```

#####3. ***@CookieValue*** 

绑定Cookie中的数据.例如我们要获取Cookie中的sessionId,并显示在jsp中,添加cookiebind的Action
```java
@RequestMapping(value = "/cookiebind", method = { RequestMethod.GET })
public String cookieBind(
    HttpServletRequest request,
    Model model,
    @CookieValue(value = "JSESSIONID", defaultValue = "") String jsessionId) {
  model.addAttribute("jsessionId", jsessionId);
  return "cookiebindresult";
}
```

jsp页面就是显示jsessionId `${jsessionId}`

#####4. ***@RequestHeader*** 

绑定请求头中的数据.如使用@RequestHeader来获取User-Agent,同样添加requestheaderbind的Action
```java
@RequestMapping(value = "/requestheaderbind", method = { RequestMethod.GET })
public String requestHeaderBind(
    HttpServletRequest request,
    Model model,
    @RequestHeader(value = "User-Agent", defaultValue = "") String userAgent) {
  model.addAttribute("userAgent", userAgent);
  return "requestheadlerbindresult";
}
```

#####5. ***@ModelAttribute*** 

绑定数据到数据模型中.之前的AccountModel例子中,使用的是`mode.addAttribute("accountmodel",accountModel)`方法,使用ModelAttribute注解可以更简单的将数据添加到Model中去.
```java
@RequestMapping(value = "/modelautobind", method = {RequestMethod.POST})
public String modelAttributeBind(
  HttpServletRequest request,
  Model model,
  @ModelAttribute("accountmodel") AccountModel accountModel){
    return "modelautobindresult";
}
```

#####6. ***@SessionAttribute*** 

Model中数据的作用域是Request级别,即在一个Request中无法获取其它Request的Model数据.可以使用@SessionAttributes将数据存储到session,这样可以保存多次请求间的数据.可以使用这种方式来实现分步骤提交表单.

下例实现分为两步把数据绑定到AccountModel中

>Step1.在DataBindController上添加:

```java
@SessionAttributes(value = "sessionaccountmodel")
```

>Step2.在Controller中添加两个action,分别是usernamebind与passwordbind.

```java
//Controller中新添加代码如下
@RequestMapping(value = "/usernamebind", method = { RequestMethod.GET })
public String userNameBind(Model model, AccountModel accountModel) {
  // 初始化一个AccountModel实例
  // 在Controller上将其放入了SessionAttributes中
  model.addAttribute("sessionaccountmodel", new AccountModel());
  return "usernamebind";
}

@RequestMapping(value = "/usernamebind", method = { RequestMethod.POST })
public String userNameBindPost(
    @ModelAttribute(value = "sessionaccountmodel") AccountModel accountModel) {
  return "redirect:passwordbind";
}

@RequestMapping(value = "/passwordbind", method = { RequestMethod.GET })
public String passwordBind(
    @ModelAttribute("sessionaccountmodel") AccountModel accountModel) {
  return "passwordbind";
}

@RequestMapping(value = "passwordbind", method = { RequestMethod.POST })
public String passwordBinderPost(@ModelAttribute("sessionaccountmodel") AccountModel accountModel,
    SessionStatus status) {
  // 销毁@SessionAttributes中存储的对象
  status.setComplete();
  return "sessionmodelbindresult";
}
```

>Step3.相应的输入表单的jsp为

```html
<!-- usernamebind.jsp -->
<form:form modelAttribute="sessionaccountmodel" method="post">     
    用户名：<form:input path="username"/><br/>
    <input type="submit" value="Submit" />
</form:form>
<!-- passwordbind.jsp -->
<form:form modelAttribute="sessionaccountmodel" method="post">
    密 码：<form:password path="password"/><br/>
    <input type="submit" value="Submit" />
</form:form> 
```

在上例中,由于在Controller上指定了@SessionAttributes,所以在@ModelAttribute("xxx")注解参数会直接在@SessionAttributes中查找名称为"xxx"的对象.