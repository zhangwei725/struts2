# 二、入门教程

## 一、使用步骤

1. 创建maven web项目
2. 导入依赖库
3. 在web.xml加入Struts 2的过滤器
4. 编写Action类
5. 编写Struts 2配置文件，分为struts.xml

## 二、步骤详解

### 1、创建maven web项目 (略)

### 2、在pom.xml中添加依赖包

```
 <!-- https://mvnrepository.com/artifact/org.apache.struts/struts2-core -->
        <dependency>
            <groupId>org.apache.struts</groupId>
            <artifactId>struts2-core</artifactId>
            <version>2.5.13</version>
        </dependency>
        <dependency>
```

### 3、配置web.xml

1. 配置的时候注意struts的版本 过滤的class也不一样

   ```
   	<!--2.1.3之前-->
       <filter>
           <filter-name>struts2</filter-name>
           <filter-class>org.apache.struts2.dispatcher.FilterDispatcher</filter-class>
       </filter>
        <!--2.1.3-2.5.x之前-->
       <filter>
           <filter-name>struts2</filter-name>
           <filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
       </filter>
        <!--2.5.x-->
       <filter>
           <filter-name>struts2</filter-name>
           <filter-class>org.apache.struts2.dispatcher.filter.StrutsPrepareAndExecuteFilter</filter-class>
       </filter>
       
   <filter-mapping>
           <filter-name>struts2</filter-name>
           <url-pattern>/*</url-pattern>
   </filter-mapping>
   ```

### 4、配置struts2.xml文件

#### 4.1、示例代码

```
<?xml version="1.0" encoding="utf-8" ?>
<!DOCTYPE struts PUBLIC
        "-//Apache Software Foundation//DTD Struts Configuration 2.5//EN"
        "http://struts.apache.org/dtds/struts-2.5.dtd">
<struts>
    <package name="" extends="" namespace="" >
        <action name="" class="" method="">
            <result name="" type=""></result>
        </action>
    </package>
</struts>
```

#### 4.2、标签说明

1. package标签

   要配置Action的标签，那么必须要先配置package标签，代表的包的概念

   - name属性

     > 包的名称，要求是唯一的，管理action配置 

   - extends属性

     > 继承，可以继承其他的包，只要继承了，那么该包就包含了其他包的功能，一般都是继承struts-default

   - namespace属性

     > 名称空间，一般<action标签中的name属性共同决定访问路径（通俗话：怎么来访问action），常见的配置如下
     >
     > 根名称空间 : namespace="/"       
     >
     > 带有名称的名称空间 : namespace="/xxx"   

   - abstract属性

     > 抽象的。这个属性基本很少使用，值如果是true，那么编写的包是被继承的

2. action标签

   代表配置 action 类，包含的属性

   - name 

     > 和package标签的 namespace 属性一起来决定访问路径的

   - class 

     > 配置Action类的全路径（默认值是 ActionSupport 类）

   - method

     > Action类中执行的方法，如果不指定，默认值是 execute

3. result标签

   action 类中方法执行，返回的结果跳转的页面

   - name     

     > 结果页面逻辑视图名称

   - type 

     > 结果类型（默认值是转发，也可以设置其他的值）

