# 过滤器与监听器

## 一 、过滤器(filter)

### 1.1、说明

​	**在源数据和目的数据之间起过滤作用的中间组件**

​	当web容器接收到一个对资源的请求时，它将判断是否有过滤器与这个资源相关，如果有，那么容器将请求交给过滤器进行处理。在过滤器中，可以改变请求的内容或者重新设置请求的报头信息，然后再将请求发送给目标资源。当目标资源对请求作出响应时，容器同样会将响应转发给过滤器。然后发送到客户端

### 1.2、web.xml 中声明的每个 filter 在每个虚拟机中仅仅只有一个实例。

1. 加载和实例化     

   Web 容器启动时，即会根据 web.xml 中声明的 filter 顺序依次实例化这些 filter。


1. 初始化

​        Web 容器调用 init(FilterConfig) 来初始化过滤器。容器在调用该方法时，向过滤器传递 FilterConfig 对象，FilterConfig 的用法和 ServletConfig 类似。利用 FilterConfig 对象可以得到 ServletContext 对象，以及在 web.xml 中配置的过滤器的初始化参数。在这个方法中，可以抛出 ServletException 异常，通知容器该过滤器不能正常工作。此时的 Web 容器启动失败，整个应用程序不能够被访问。实例化和初始化的操作只会在容器启动时执行，而且只会执行一次。

1. doFilter

​        doFilter 方法类似于 Servlet 接口的 service 方法。当客户端请求目标资源的时候，容器会筛选出符合 filter-mapping 中的 url-pattern 的 filter，并按照声明 filter-mapping 的顺序依次调用这些 filter 的 doFilter 方法。在这个链式调用过程中，可以调用 chain.doFilter(ServletRequest, ServletResponse) 将请求传给下一个过滤器(或目标资源)，也可以直接向客户端返回响应信息，或者利用 RequestDispatcher 的 forward 和 include 方法，以及 HttpServletResponse 的 sendRedirect 方法将请求转向到其它资源。需要注意的是，这个方法的请求和响应参数的类型是 ServletRequest  和 ServletResponse，也就是说，过滤器的使用并不依赖于具体的协议。

1. 销毁

​        Web 容器调用 destroy 方法指示过滤器的生命周期结束。在这个方法中，可以释放过滤器使用的资源。

### 1.3、接口相关接口介绍

#### 3.1、Filter接口

1. init(FilterConfig filterConfig)

   ```
   初始化过滤器。filterconfig用来获取ServletContext,初始化参数。
   ```

2. doFilter(ServletRequest request,ServletResponse response,FilterChain chain);

   ```
   实现过滤功能。可以调用chain.doFilter(request,response)方法将请求传递给下一个过滤器或者目标资源，也可以直接向客户端返回响应信息。
   ```

3. destory()

   ```
   结束过滤器的声明周期。
   ```

#### 3.2、FilterConfig接口

1. getInitParameter(String name)

   ```
   返回名为name的初始化参数的值
   ```

2. getInitParameterNames()

   ```
   返回所有初始化参数的名字的枚举集合。
   ```

3. getServletContext()

   ```
   返回Servlet上下文对象的引用
   ```

#### 3.3、FilterChain接口

1. doFilter(ServletRequest request,ServletResponse response)

   ```
   调用该方法将使过滤器链中的下一个过滤器被调用，如果该方法时过滤器链中最后一个过滤器，那么目标资源被调用
   ```

### 1.4、简单使用

#### 4.1、Filter开发分为2步：

1. 编写java类实现Filter接口，并实现其doFilter方法。
   1. 在web.xml 文件中使用<filter>和<filter-mapping>元素对编写的filter类进行注册，并设置它所能拦截的资源。	

#### 4.2  JAVA代码

```
public class CharsetFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
    
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain filterChain) throws IOException, ServletException {
        // 对request、response进行一些预处理
        request.setCharacterEncoding("utf-8");
        response.setContentType("text/html ; charset=utf-8");
        //继续拦截
        filterChain.doFilter(request, response);
        
    }

    @Override
    public void destroy() {

    }
}





```



```
 <filter>
        <filter-name>charset-filter</filter-name>
        <filter-class>com.werner.demo.filter.CharsetFilter</filter-class>
        <init-param>
            <param-name>charset</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <init-param>
            <param-name>enable</param-name>
            <param-value>true</param-value>

        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>charset-filter</filter-name>
        <url-pattern>/servlet/*</url-pattern>
        <url-pattern>/l</url-pattern>
        <dispatcher>REQUEST</dispatcher>
    </filter-mapping>


    <servlet>
        <servlet-name>Filter</servlet-name>
        <servlet-class>com.werner.demo.servlet.HttpServletFilter</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>Filter</servlet-name>
        <url-pattern>/l</url-pattern>
    </servlet-mapping>

说明:
1. <filter>  ：配置 Filter 名称，实现类以及初始化参数。可以同时配置多个初始化参数。
2. <filter-mapping> ：配置什么规则下使用这个Filter 。
1、<url-pattern> ：配置url的规则，可以配置多个，也可以使用通配符（*）。例如 /jsp/* 适用于本ContextPath下以“/jsp/ ”开头的所有servlet路径， *.do 适用于所有以“ .do”结尾的servlet路径。

2、<dispatcher> ：配置到达servlet的方式，可以同时配置多个。有四种取值：REQUEST、FORWARD、ERROR、INCLUDE。如果没有配置，则默认为REQUEST。它们的区别是：
	1. REQUEST ：表示仅当直接请求servlet时才生效。
	2. FORWARD ：表示仅当某servlet通过forward转发到该servlet时才生效。
 	3. INCLUDE ：Jsp中可以通过<jsp:include/>请求某servlet， 只有这种情况才有效。
	4. ERROR ：Jsp中可以通过<%@page errorPage="error.jsp" %>指定错误处理页面，仅在这种情况下才生效。



```

### 1.5、Filter应用场景

**1、统一POST请求中文字符编码的过滤器 2、控制浏览器缓存页面中的静态资源的过滤器**

```
有些动态页面中引用了一些图片或css文件以修饰页面效果，这些图片和css文件经常是不变化的，所以为减轻服务器的压力，可以使用filter控制浏览器缓存这些文件，以提升服务器的性能。
```

**3、使用Filter实现URL级别的权限认证**

```
在实际开发中我们经常把一些执行敏感操作的servlet映射到一些特殊目录中，并用filter把这些特殊目录保护起来，限制只能拥有相应访问权限的用户才能访问这些目录下的资源。从而在我们系统中实现一种URL级别的权限功能。
```

**4、实现用户自动登陆**

```
首先，在用户登陆成功后，发送一个名称为user的cookie给客户端，cookie的值为用户名和md5加密后的密码。编写一个AutoLoginFilter，这个filter检查用户是否带有名称为user的cookie，如果有，则调用dao查询cookie的用户名和密码是否和数据库匹配，匹配则向session中存入user对象（即用户登陆标记），以实现程序完成自动登陆。
```

**5、认证Filter**

**6、日志和审核Filter**

**7、图片转换Filter**

**8、数据压缩Filter**

**8、密码Filter**

**9、令牌Filter**

## 二、监听器

### 2.1、说明

> Servlet API中定义了8个监听器接口，可以用于监听ServletContext,HttpSession,ServletRequest对象的生命周期事件。
>
> 1. ServletContextListener 监听Servlet上下文对象初始化或者被销毁
> 2. ServletContextAttributeListener 监听Servlet上下文中的属性列表的变化
> 3. HttpSessionListener 监听Session生命周期
> 4. HttpSessionActionListener 监听session被钝化或者激活
> 5. HttpSessionAttributeListener 监听Session属性列表发生的变化
> 6. HttpSessionBindingListener 监听Session中是否有对象绑定或者删除，该对象要实现这个接口
> 7. ServletRequestListener 监听ServletRequest对象生命周期
> 8. ServletRequestAttributeListener 监听ServletRequest属性列表发生的变化

### 2.2、详细解释

1. ServletContextListener 监听ServletContext的启动或者销毁

2. contextInitialized(ServletContextEvent sce)

   ```
           当web应用程序初始化进程正开始时，web容器调用这个方法，该方法将在所有的过滤器和Servlet初始化之前被调用。contextDestroyed(ServletContextEvent sce)当Servlet上下文将要关闭时，Web容器调用这个方法，该方法在所有Servlet和和过滤器销毁之后被调用。

     课堂例子：
         在Web应用程序启动时初始化DataSource对象，然后将其方法ServletContext中
     数据库支持更换
   ```

3. HttpSessionBindingListener，（无序配置）

   ```
     如果一个对象实现了HttpSessionBindingListener接口，当这个对象被绑定的Session或者从Session中被删除时，Servlet容器就会通知这个对象。
   ```

   - valueBound(HttpSessionBindingEvent event)

     ```
     当对象正在被绑定到Session中，Servlet容器调用这个方法通知该对象
     ```

   - valueUnBound(HttpSessionBindingEvent event)