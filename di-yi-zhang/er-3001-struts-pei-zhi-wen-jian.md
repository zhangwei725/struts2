# Struts2 配置文件

## 一、概要

### 1、Struts 主要的配置文件

1. web.xml 设置过滤器以及annotation初始化参数 
2. struts.xml 主配置文件(重要)
3. struts.properties 默认属性文件 
4. struts-``default``.xml 默认配置文件 
5. struts-plugin.xml 插件配置文件

### 1、Struts2 主要配置加载次序

1. struts-default.xml
2. struts-plugin.xml
3. struts.xml
4. struts.properties

注： Web.xml`如果在多个文件中配置了同一个Struts2常量，则后一个文件中的配置的常量值将覆盖前面文件中配置的常量值。在不同文件中配置常量的方式是不一样的，但不管哪个文件中，配置Struts2常量都要指定两个属性：常量name和常量value，推荐在struts.xml文件中配置Struts2常量

## 二、Struts-default.xml(了解)

### 1、配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
        "-//Apache Software Foundation//DTD Struts Configuration 2.5//EN"
        "http://struts.apache.org/dtds/struts-2.5.dtd">
<struts>
    <bean class="" name="struts" scope="" type="" optional="" static=""/>
</struts>	
```

### 2、Bean元素属性

1. class：必选，指定了Bean实例的实现类 `
2. type：可选，通常是通过某个接口或者在此前定一个过的Bean 
3. name：可选，它指定的Bean实例的名字，对于有相同type的多个Bean，name必须唯一 `
4. scope：可选，指定Bean的作用域，只能是default、singleton、request、session和thread之一 
5. static：可选，它指定Bean是否使用静态方法注入。通常而言，当指定了type属性时，该属性就不应该设置为 `true`
6. optional：可选，指定Bean是否是一个可选Bean

## 三、default.properties 默认属性信息

1. 说明

   这个配置文件提供了一种机制来改变框架的默认行为。实际上，**struts.properties**配置文件中包含的所有属性也可以在**web.xml**中配置使用**init-param**，以及在**struts.xml**配置文件中使用**constant**标签

2. 默认属性

   ```properties
   ##字符集 
   struts.i18n.encoding=UTF-8 
   struts.objectFactory.spring.autoWire = name 
   struts.objectFactory.spring.useClassCache = true
   struts.objectFactory.spring.autoWire.alwaysRespect = false
   struts.multipart.parser=jakartastruts.multipart.saveDir=struts.multipart.maxSize=2097152 
   ##请求后缀 
   struts.action.extension=action,, 
   struts.serve.static=true
   struts.serve.static.browserCache=true
   struts.enable.DynamicMethodInvocation = false
   struts.enable.SlashesInActionNames = false
   struts.mapper.action.prefix.enabled = false
   struts.mapper.action.prefix.crossNamespaces = false
   struts.tag.altSyntax=true
   ##开发模式 
   struts.devMode = false
   struts.i18n.reload=false
   struts.ui.theme=xhtmlstruts.ui.templateDir=template 
   struts.ui.theme.expansion.token=~~~struts.ui.templateSuffix=ftl 
   struts.configuration.xml.reload=false
   struts.velocity.configfile = velocity.properties 
   struts.velocity.contexts = 
   struts.velocity.toolboxlocation= 
   struts.url.http.port = 80struts.url.https.port = 443 
   struts.url.includeParams = none 
   struts.dispatcher.parametersWorkaround = false
   struts.freemarker.templatesCache=false
   struts.freemarker.beanwrapperCache=false
   struts.freemarker.wrapper.altMap=true
   struts.freemarker.mru.max.strong.size=0 
   struts.xslt.nocache=false
   struts.mapper.alwaysSelectFullNamespace=false
   struts.ognl.allowStaticMethodAccess=false
   struts.el.throwExceptionOnFailure=false
   struts.ognl.logMissingProperties=false
   struts.ognl.enableExpressionCache=true
   struts.handle.exception=true
   ```

3. 常用的属性

   - struts.i18n.encoding=UTF-8            

     > 指定默认编码集,作用于HttpServletRequest的setCharacterEncoding方法 

   - struts.action.extension=action,

     > 该属性指定需要Struts 2处理的请求后缀，该属性的默认值是action，即所有匹配*.action的请求都由Struts2处理。如果用户需要指定多个请求后缀，则多个后缀之间以英文逗号（,）隔开

   - struts.serve.static.browserCache=true 

     > 设置浏览器是否缓存静态内容,默认值为true(生产环境下使用),开发阶段最好关闭 

   - struts.configuration.xml.reload=false  

     > 当struts的配置文件修改后,系统是否自动重新加载该文件,默认值为false(生产环境下使用)

   - struts.devMode = false   

     > 当struts的配置文件修改后,系统是否自动重新加载该文件,默认值为false(生产环境下使用) 	

   - struts.multipart.maxSize

     > 文件上传最大大小

   - struts.multipart.saveDir 

     > 上传文件默认保存路径

   - struts.configuraction.xml.reload

     > 设置配置文件自动重新加载

## 四、Struts.xml 配置信息(重要)

1. 配置文件

   ```xml
   <struts> 
       <!--开发模式—> 
       <constant name="struts.devMode" value="true"></constant> 
      	<!-- 支持动态调用 -->
       <constant name="struts.enable.DynamicMethodInvocation" value="true"/>
       <!--配置编码—> 
       <constant name="struts.i18n.encoding" value="utf-8"></constant> 
       <!--定义包—> 
       <package name="default" namespace="/" extend="struts-default"> 
           <!--动作对应控制层的每个Action类—> 
           <action name="通过url访问的名字" class="包名+类名"> 
               <result name="返回值的名字" type="返回的数据类型">对应的jsp文件</result> 
           </action> 
       </package> 
   </struts>
   ```

