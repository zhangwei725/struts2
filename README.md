# 简介

## 一、概要

Struts是Apache基于Model2模型（MVC设计模式）开发的一个开源的Web应用框架，由一组相互协作的类或组件、Servlet以及jsp标签库组成。
Struts采用拦截器的机制来处理用户的请求，也使得业务逻辑控制器能够与Servlet完全脱离，大大缩减了使用MVC模式开发web应用的时间，降低了程序的复杂度，提高了开发效率。
Struts具有如下一些特点：

1. 提供了拦截器，利用拦截器可以进行AOP（面向方面）编程，实现如权限拦截等功能
2. 提供了类型转换器，可以把特殊的请求参数转化成需要的类型
3. 提供支持多种表现层技术，如：JSP、FreeMarker等
4. 输入校验可以对指定的方法进行校验
5. 提供了全局范围、包范围和Action范围的国际化资源文件管理实现

## 二、什么是 Struts 2 的框架

Struts 2 是 Struts 1 的下一代产品，是在 struts 1和 WebWork 的技术基础上进行了合并的全新的 Struts 2框架。

其全新的 Struts 2 的体系结构与 Struts 1 的体系结构差别巨大。

Struts 2 以 WebWork 为核心，采用拦截器的机制来处理用户的请求，这样的设计也使得业务逻辑控制器能够与ServletAPI完全脱离开，所以 Struts 2 可以理解为 WebWork 的更新产品。

虽然从 Struts 1 到 Struts 2 有着太大的变化，但是相对于 WebWork，Struts 2 的变化很小。

## 三、Web 层的框架

1. Struts 1
2. Struts 2
3. Webwork
4. SpringMVC(重点)

## 四、Web 层框架的特点

1. 原理：前端控制器模式(附-前端控制器模式)
2. 核心：前端控制器（核心的控制器）
3. Struts 2 框架前端控制器就是过滤器（Filter）

## 五、工作原理图

### 1、原理图

1. 2.1.3-

   ![](http://opzv089nq.bkt.clouddn.com/17-10-21/83041106.jpg)

   ​

2. 2.1.3+

   ![](http://opzv089nq.bkt.clouddn.com/17-10-21/42242150.jpg)

### 2、工作流程

1. 客户端初始化一个指向Servlet容器（例如Tomcat）的请求 
2. Servlet容器通过web.xml映射请求，获得控制器的名字，并调用控制器FilterDispatcher（2.1版本以前）或StrutsPrepareAndExecuteFilter（2.1版本以后）
3. 接着FilterDispatcher被调用，FilterDispatcher询问ActionMapper来决定这个请是否需要调用某个Action 
4. 如果ActionMapper决定需要调用某个Action，FilterDispatcher把请求的处理交给ActionProxy 
5. ActionProxy通过Configuration Manager询问框架的配置文件，找到需要调用的Action类 
6. ActionProxy创建一个ActionInvocation的实例。 
7. ActionInvocation实例使用命名模式来调用，在调用Action的过程前后，涉及到相关拦截器（Intercepter）的调用。 
8. 一旦Action执行完毕，ActionInvocation负责根据struts.xml中的配置找到对应的返回结果。返回结果通常是（但不总是，也可 能是另外的一个Action链）一个需要被表示的JSP或者FreeMarker的模版。在表示的过程中可以使用Struts2 框架中继承的标签。在这个过程中需要涉及到ActionMapper 

## 七、核心jar介绍

下载地址：http://struts.apache.org/download.cgi

1. struts2-core-2.x.x.jar :　　　   

   Struts 2框架的核心类库

2. ognl-2.6.x.jar :　　　　　　　    

   对象图导航语言（Object Graph Navigation Language），struts2框架通过其读写对象的属性

3. freemarker-2.3.x.jar :　　　　   

   Struts 2的UI标签的模板使用FreeMarker编写

4. commons-logging-1.x.x.jar :     

   ASF出品的日志包，Struts 2框架使用这个日志包来支持Log4J和JDK 1.4+的日志记录。

5. commons-fileupload-1.2.1.jar： 

   文件上传组件，2.1.6版本后必须加入此文件

#  



