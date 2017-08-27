# Servlet开发基础

## 一、什么是servlet

### 1、扩展web服务器功能

> ​	在BS架构中，早期的Web服务器只能处理静态资源的请求，也就是无法根据请求进行计算后再生成相应的HTML内容。为了补充Web服务器的这个缺陷，于是增强服务器功能的CGI技术最早产生了。			CGI（Common GatewayInterface通用网关接口）也是一种规范，可以使用不同的语言来开发，如Perl，C，Java等都可以。当客户端请求静态资源时，Web服务器会自己处理并返回，当客户端请求动态资源时，Web服务器会把请求转交给扩展程序来处理，并将扩展程序的处理结果返回给客户端。但是CGI技术开发复杂，性能较差，只要有一个请求到达，Web服务器就会单独分配一个进程来进行处理，可移植性不好，所以慢慢就由后来的Servlet技术所取

> ​	Servle技术是使用Java语言开发的一套组件规范，不再像CGI技术那样需要分配单独的进程来处理请求，而是单独分配一个线程来处理，于是大大提升了处理效率。并且Java语言是跨平台的语言，也提升了Web服务器扩展程序的可移植性，已经取代了CGI技术，成为BS架构中的主流技术。所有后续的BS架构中的主流框架本质上都是基于Servlet来实现的

### 2、组件规范

> ​	组件规范是依靠一套API来实现的，也就是说开发中只要基于Sun公司提供的这套API，按照一定的规则来编写程序，那么就可以实现针对Web服务器的功能扩展。
>
> ​	是组件只是对部分功能的一个实现，不能单独运行，必须放在一定的环境中才能运行。而这个针对各个组件进行管理、创建、销毁的运行环境即容器。

### 3、Servlet运行原理

> ​	Servlet作为补充Web服务器功能的组件，需要依赖于Servlet容器才能运行，它的运行原理如图 – 4所示。
>
> ![](http://opof44evx.bkt.clouddn.com/17-5-9/76433272-file_1494318735556_13dcb.jpg)
>
> ​									**图-4 web容器**
>
> ​	在浏览器中输入请求地址后，浏览器会依据IP地址及端口号找到对应的Web服务器，如果请求的是静态资源，Web服务器直接提供响应；如果请求的是动态资源，则直接将请求传递给容器来处理
>
> ​	能够充当Servlet容器这个角色的有很多软件，如Tomcat、Weblogic、JBoss等。而这些Servlet容器不仅仅具备了管理Servlet组件的功能，也具备了Web服务器的一些功能，所以很多时候只要安装一个Tomcat软件就同时具备了Web服务器及Servlet容器的双重功能。
>
> ​	从上图中可以看出 Tomcat 的心脏是两个组件：Connector 和 Container，关于这两个组件将在后面详细介绍。Connector 组件是可以被替换，这样可以提供给服务器设计者更多的选择.
>
> ​	Connector 组件是 Tomcat 中两个核心组件之一，它的主要任务是负责接收浏览器的发过来的 tcp 连接请求，创建一个 Request 和 Response 对象分别用于和请求端交换数据，然后会产生一个线程来处理这个请求并把产生的 Request 和 Response 对象传给处理这个请求的线程，处理这个请求的线程就是 Container 组件要做的事了。
>
> ​	Container 是容器的父接口，所有子容器都必须实现这个接口，Container 容器的设计用的是典型的责任链的设计模式，它有四个子容器组件构成，分别是：Engine、Host、Context、Wrapper。其中Context 代表 Servlet 的 Context，它具备了 Servlet 运行的基本环境，理论上只要有 Context 就能运行 Servlet 了。

### 4、Java Servlet

> Java服务器小程序,是一个基于java技术的web组件，运行在服务器端，由Servlet容器所管理，用于生成动态的内容，Servlet是平台独立的java类，编写一个Servlet实际上就是按照Servlet规范编写一个Java类，Servlet被编译为平台独立的字节码，可以被动态地加载到支持java技术的web服务器中运行。

### 5、Servlet容器

> Servlet容器也叫Servlet引擎，是Web服务器或者应用服务器的一部分，用户在发送请求和响应之上提供网络服务，解码基于MIME的请求，格式化基于MIME的请求，Servlet不能独立运行，必须被部署到Servlet容器中，由容器实例化和调用Servlet的方法，Servlet容器在Servlet的生命周期内包含和管理Servlet
>
> 注:**MIME类型**是一种通知客户端其接收文件的多样性的机制:文件后缀名在网页上并没有明确的意义。因此，使服务器设置正确的传输类型非常重要，所以正确的MIME类型与每个文件一同传输给服务器。在网络资源进行连接时，浏览器经常使用MIME类型来决定执行何种默认行为

## 二、开发步骤

### 1、在工程根目录下的pom.xml文件中<dependencys>标签下添加Servlet依赖包

```
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
    </dependency>
```

### 2、编写java类，继承HttpServlet类,重写doGet和doPost方法

```java
public class FirstServlet extends HttpServlet {
    @Override
    public void init() throws ServletException {
        super.init();
        System.out.println("初始化Servlet");
    }

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("Get请求");
        PrintWriter writer = resp.getWriter();
        writer.write("<H1>first web</H1>");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("Post请求");
        doGet(req, resp);
    }

    @Override
    public void destroy() {
        super.destroy();
        System.out.println("Servlet被销毁");
    }
}
```

### 3、在webapp/WEB-INF/web.xml文件中进行配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
                      http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">
    <servlet>
        <servlet-name>FirstServlet</servlet-name>
        <servlet-class>com.werner.web.controller.FirstServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>FirstServlet</servlet-name>
        <url-pattern>/first</url-pattern>
    </servlet-mapping>
</web-app>
```

### 4、将项目部署到Tomcat中

### 5、打开浏览器访问Servlet

​	在浏览器输入 http://localhost:8080/app/first	

## 三、请求资源路径

### 1、什么是请求资源路径

​	在浏览器地址栏中输入的地址格式： http://ip:port/app/xxxx.html

​	其中app/xxxx.html 即为请求资源路径

### 2、容器对请求的处理过程

1. 浏览器依据ip，port建立与Servlet容器之间的连接，然后将请求资源路径app/xxxx.html发送过去给容器
2. 容器依据应用名“/app”找到应用所在的文件夹，容器会默认请求的是一个Servlet，查找web.xml文件中所有的Servlet配置“”,看是否有匹配的Servlet

### 3  执行步骤详解

​	例如我们访问路径:http://localhost:8080/app/first

1. http: 协议名称

2. localhost : 到本地的host文件查找是否存在改域名对应的IP地址

3. 8080 : 找到tomcat服务器

4. /app : 在tomcat的webapps目录下面找app的目录

5. /first : 资源名称

   > 1、在WebApp工程中的web.xml中查找是否有匹配的url-pattern的内容（/first）
   > 2、如果找到匹配的url-pattern,则使用当前servlet-name的名称到web.xml文件中查询是否相同名称的servlet配置
   > 3、如果找到，则取出对应的servlet配置信息中的servlet-class内容(com.werner.web.controller.FirstServlet)

6. 通过反射:

   > 构造**first**的对象
   >
   > 然后调用**first**里面的方法

## 四、匹配Servlet的规则

1. 精确匹配

   > <servlet-mapping>
   >
   > <url-pattern>/first</url-pattern>
   >
   > <!--浏览器输入: http://localhost:8080/app/first -->
   >
   > <url-pattern>/demo.html</url-pattern>
   >
   > <!--浏览器输入: http://localhost:8080/app/demo.html -->
   >
   >   <url-pattern>/shop/detail.html</url-pattern>
   >
   > <!--浏览器输入: http://localhost:8080/app/shop/detail.html -->
   >
   > </servlet-mapping>

2. 通配符匹配

   使用“*”来匹配0个或多个字符,例如：

   > <url-pattern>/*</url-pattern>
   >
   > 代表输入任何不同的url地址都匹配成功
   >
   > http://localhost:8080/app/abc.html 匹配成功
   >
   > http://localhost:8080/app/abc/def   也匹配成功

3. 路径匹配

   > <servlet-mapping>
   >
   > ​	<servlet-name>Test3Servlet</servlet-name>
   > ​	<url-pattern>/test/*</url-pattern>
   >
   > </servlet-mapping>
   >
   > 只要是访问路径是app/test的全部都会匹配成功
   >
   > http://localhost:8080/app/test/test.html
   > http://localhost:8080/app/test/test.jsp
   > http://localhost:8080/app/test/detail.html
   > http://localhost:8080/app/test/action
   > http://localhost:8080/app/test/action/aaa

4. 后缀匹配 (扩展名匹配)

   使用“*.”开头的任意多个字, 例如：

   > <servlet-mapping>
   >
   > ​	<servlet-name>TestServlet</servlet-name>
   >
   > ​	<url-pattern>*.do</url-pattern>
   >
   > ​	<url-pattern>*.jsp</url-pattern>
   >
   > </servlet-mapping>
   >
   > *.do会匹配以”.do”结尾的所有请求,
   >
   > http://localhost:8080/app/xxx.do     匹配成功 
   >
   > http://localhost:8080/app/shop/xxx.do也匹配成功
   >
   > *.jsp会匹配以”.jsp”结尾的所有请求,都会被配到
   >
   > http://localhost:8080/app/1.jsp
   >
   > http://localhost:8080/app/test.jsp

5. 匹配任意的url

   > <url-pattern>配置成如下两种的任意一种
   >
   > <url-pattern>/</url-pattern>
   >
   > <url-pattern>/*</url-pattern>
   >
   > 注意:
   >
   > 1、当url-pattern配置成/*的时候，Tomcat会将所有的请求交给对应的Servlet进行处理，
   > 2、当url-pattern配置成/的时候，多数情况下与/*效果一致，但是，当访问的路径正好对应jsp文件时，Tomcat会访问真实的jsp文件而不是把请求交给对应的Servlet处理

6. 注意事项

   1、<url-pattern>要么以/开头，要么以*开头：例如

   > <url-pattern>/app/*.jsp</url-pattern>  错误
   >
   > <url-pattern>/*.jsp</url-pattern> 错误
   >
   > <url-pattern>he*.jsp</url-pattern>错误

   2、<url-pattern>/aa/*/bb</url-pattern>

   > 这个是精确匹配，url必须是 /aa/*/bb，这里的*不是通配的含义

7. 示例代码

   1. web.xml 配置

      ```
      <servlet>  
        <servlet-name>Test</servlet-name>  
        <servlet-class>com.werner.servlet.TestServlet</servlet-class>  
       </servlet>  
       <servlet>  
        <servlet-name>DoTestServlet</servlet-name>  
        <servlet-class>com.werner.servlet.TestServlet</servlet-class>  
       </servlet>  
       <servlet-mapping>  
        <servlet-name>Test</servlet-name>  
        <url-pattern>*.html</url-pattern>  
       </servlet-mapping>  
       <servlet-mapping>  
        <servlet-name>DoTestServlet</servlet-name>  
        <url-pattern>*.do</url-pattern>  
       </servlet-mapping>
      ```

   2. 映射关系

      |  序号  |   匹配   | 优先级  |
      | :--: | :----: | :--: |
      |  1   | /abc/* |  1   |
      |  2   |   /*   |  2   |
      |  3   |  /abc  |  3   |
      |  4   |  *.do  |  4   |

   3. 映射关系归纳

      ```
      当请求URL为"/abc/a.html"时，"/abc/*"和"/*"都可以匹配这个URL 则Servlet引擎将调用1
      当请求URL为"/abc/a.do"时，"/abc/*"和"/*.do"都可以匹配这个URL 则Servlet引擎将调用1
      当请求URL为"/a.do"时，"/*"和"/*.do"都可以匹配这个URL 则Servlet引擎将调用2
      当请求URL为"/abc"时，"/abc/*"和"/abc"都可以匹配这个URL 则Servlet引擎将调用3
      ```

## 五、无匹配的Servlet的处理

1. 说明

   Servlet的缺省路径（<url-pattern>/</url-pattern>）是在tomcat服务器内置的一个路径。该路径对应的是一个DefaultServlet (缺省Servlet) 这个缺省的Servlet的作用是用于解析web应用的静态资源文件


1. 源码

   ```
      <servlet>
           <servlet-name>default</servlet-name>
           <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
           <init-param>
               <param-name>debug</param-name>
               <param-value>0</param-value>
           </init-param>
           <init-param>   
               <param-name>listings</param-name>
               <param-value>false</param-value>
           </init-param>
           <load-on-startup>1</load-on-startup>
       </servlet>
   ……

   <servlet-mapping>
           <servlet-name>default</servlet-name>
           <url-pattern>/</url-pattern>
   </servlet-mapping>

   1、servlet的映射路径为一个（/），称之为缺省的servlet。则这个servlet就成为了当前web应用程序的缺省servlet。
   2、缺省的servlet的作用：凡是在web.xml文件总找不到匹配的<servlet-mapping>元素的URL，他们的方位请求都将交给缺省的servlet处理。也就是说，缺省的servlet用于处理所有其他servlet不处理的访问请求。
   3、当访问tomcat服务中的某个静态html文件和图片时，实际上是在访问这个缺省的servlet。
   4、把上述中的代码注释掉，重启tomcat服务器，输入[http://localhost:8080](http://localhost:8080/) ，将发现页面中的图片将显示不出，也即是因为这个原因
   ```

七，HttpServlet继承关系图

> ![](http://opzv089nq.bkt.clouddn.com/17-8-27/2803985.jpg)

## 六、思考题

URL输入http://localhost:8080/app/index.html 如何读取文件？

1. 到当前app应用下的web.xml文件查找是否有匹配的url-pattern。
2. 如果没有匹配的url-pattern，则交给tomcat的内置的DefaultServlet处理
3. DefaultServlet程序到app应用的根目录下查找是否存在一个名称为index.html的静态文件
4. 如果找到该文件，则读取该文件内容，返回给浏览器
5. 如果找不到该文件，则返回404错误页面
6. 结论: 先找动态资源，再找静态资源总结

