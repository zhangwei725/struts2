# 前端控制器模式

## 一、 什么是前端控制模式

> 前端控制器设计模式用于提供集中式请求处理机制，以便所有请求将由单个处理程序处理。此处理程序可以执行请求的身份验证/授权/记录或跟踪，然后将请求传递到相应的处理程序。 主要包含以下三部分内容
>
> - **前端控制器** - 用于处理应用程序（基于Web或基于桌面）的各种请求的单个处理程序。
> - **分发器** - 前端控制器可以使用可以将请求分派到相应的特定处理器的分派器对象。
> - **视图** - 视图是进行请求的对象。

## 二、为什么要使用前端控制

### 1、说明

> 都是为了解决某些问题而使用的。Front Controller模式用来解决什么问题呢？
>
> 我们在开发WEB应用系统（但不拘于WEB应用）时，存在很多不恰当的设计方法，比如让客户端（Client，一般指浏览器）可以直接访问各个视图（view，JSP等）。这样逻辑被分散到各个视图中，从而产生了各种问题：
>
> 1. 对已有的功能修改困难，可维护性低。假如session管理，一旦session内容需要发生改变，则需要修改所有view中的相关代码。
> 2. 很难增加新的功能，缺乏可扩展性。例如，我们需要在已有的系统中加入安全控制功能，控制用户对某些页面的访问，因为没有统一的处理入口，我们需要在所有的view中都加上认证代码。
>
> 对于系统中需要提供的类似以下功能：
>
> 认证,日志, 页面导航与转发等, Session管理, 国际化或本地化处理, 其它共通处理等，都会存在代码分散重复，可维护性低，缺乏可扩展性等问题。

### 2、解决方法

使用Front Controller，强制分离view的显示逻辑与业务处理逻辑。

前端控制器

Controller作为所有request的最初访问点。

### 3、实现方式

Front Controller模式可以有多种实现方式：

**Servlet**（+ Dispatcher，+Command，+…） **Front Controller策略** 
**Filter Controller策略** 
**JSP Front Controller策略** 

等，一般推荐使用前2种实现方法。

### 4、优点：

使用Front Controller模式有以下好处

1. 集中控制
2. 提高可管理性和安全控制能力
3. 提高可重用性可扩展性

## 三、示例代码

### 1、前端控制器模式

1. 创建视图类

   ```java
   //管理员界面
   public class Manager {

       public void show() {
           System.out.println("欢迎进入管理员界面");
       }
   }

   //普通用户界面
   public class IndexView {

       public void show(){
           System.out.println("欢迎进入首页");
       }
   }
   ```


1. 创建分发器类

   ```java
   public class Dispatcher {
       private IndexView indexView;
       private ManagerView mv;
       public Dispatcher() {
           indexView = new IndexView();
           mv = new ManagerView();
       }
       
       public void dispatch(String request) {
           if ("admin".equalsIgnoreCase(request)) {
               indexView.show();
           } else {
               mv.show();
           }
       }
   }
   ```

2. 创建前端控制器 - FrontController

   ```java
   public class FrontController {

       private Dispatcher dispatcher;

       public FrontController(){
           dispatcher = new Dispatcher();
       }

       private boolean isAuthenticUser(){
           System.out.println("successfully.");
           return true;
       }
       private void trackRequest(String request){
           System.out.println("Page requested: " + request);
       }

       public void dispatchRequest(String request){
           trackRequest(request);
           if(isAuthenticUser()){
               dispatcher.dispatch(request);
           }
       }
   }
   ```

### 2、Servlet简单的控制器

1.  先定义一个IAction接口

   ```
   /**
    *注意:真正执行客户端请求的对象必须实作 IAction 接口
    */
   public interface IAction {
       public void execute(HttpServletRequest req,
                           HttpServletResponse res);
   }
   ```

2.  控制转发类

   ```java
   /**
    * 这个类将根据客户端请求的Servlet路径来了解实际该调用的Action对象，
    * 如果找不到，则转发至预设的页面
    */
   public class Invoker {
       private Map<String, IAction> actions;
       private String defaultPage;
       public Invoker() {
           actions = new HashMap();
       }
       public void addCommand(String actionName,
                              IAction action) {
           actions.put(actionName, action);
       }
       public void request(HttpServletRequest req,
                           HttpServletResponse res)
               throws ServletException, IOException {
           IAction action =
                   actions.get(req.getServletPath());
           if (action != null) {
               action.execute(req, res);
           } else {
               RequestDispatcher dispatcher =
                       req.getRequestDispatcher(defaultPage);
               dispatcher.forward(req, res);
           }
       }
       public String getDefaultPage() {
           return defaultPage;
       }
       public void setDefaultPage(String defaultPage) {
           this.defaultPage = defaultPage;
       }
   }
   ```

3. DispatcherServlet(Controller)

   ```java
   public class DispatcherServlet extends HttpServlet {
       private Invoker invoker;

       @Override
       public void init() throws ServletException {
           invoker = new Invoker();
           invoker.addCommand("/welcome.action",
                   new WelcomeAction());
           invoker.addCommand("/login.action",
                   new LoginAction());
           invoker.setDefaultPage("/welcome.action");
       }
       @Override
       public void doPost(HttpServletRequest req,
                          HttpServletResponse res)
               throws ServletException, IOException {
           invoker.request(req, res);
       }

       @Override
       public void doGet(HttpServletRequest req,
                         HttpServletResponse res)
               throws ServletException, IOException {
           doPost(req, res);
       }
   } 


   public class LoginAction implements IAction {
       @Override
       public void execute(HttpServletRequest req, HttpServletResponse res) {
           System.out.println("");

       }
   }
   public class WelcomeAction implements IAction {
       @Override
       public void execute(HttpServletRequest req, HttpServletResponse res) {

       }
   }
   ```

4. web.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                         http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
     version="3.1">
   <description></description> 
   <display-name></display-name>
     <servlet> 
     <servlet-name>dispatcher</servlet-name> 
       	<servlet-class>com.werner.demo.DispatcherServlet</servlet-class> 
      </servlet> 
       <servlet-mapping> 
     		<servlet-name>dispatcher</servlet-name> 
       	<url-pattern>*.action</url-pattern> 
     </servlet-mapping>
   </web-app> 
   ```

   ​