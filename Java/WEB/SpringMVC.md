SpringMVC
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
(个人觉得使用Maven创建管理项目结构更好一些,此处先从基础一步一步来)

1. 准备

    将相关jar包导入到项目中/WebRoot/WEB-INF/lib目录中.(springframework改版后使用maven来管理jar包,这里先给出了一个完整的下载链接)

    spring-framework下载:[spring-framework](http://repo.springsource.org/libs-release-local/org/springframework/spring/)

2. 配置web.xml文件

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
        <!--指定springservlet的配置文件 -->
        <param-value>calsspath:springservlet-config.xml</param-value>
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

3. springmvc-serlet.xml配置示例

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
