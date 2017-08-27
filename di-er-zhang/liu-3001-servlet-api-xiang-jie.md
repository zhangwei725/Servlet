## Servlet重点接口详解

## 一、概要

### 1、体系结构

> ![](http://opzv089nq.bkt.clouddn.com/17-8-27/26008918.jpg)

## 一、Servlet接口

### 1、 概要

> Servlet API主要由两个Java包组成:javax.servlet和javax.servlet.http
>
> 在javax.servlet包中定义了Servlet接口及相关的通用接口和类；
>
> 在javax.servlet.http包中主要定义了与HTTP协议相关的HttpServlet类，HttpServletRequest接口和HttpServletResponse接口

### 2、API介绍

1. init\(ServletConfig config\)

   ```
   在Servlet实例化之后，Servlet容器会调用init()方法，来初始化该对象，
   主要是为了让servlet对象在处理客户请求前完成一些初始化工作。例如建立数据库的连接，获取配置信息等。
   对于每个Servlet实例，init()方法只能被调用一次。Servlet容器通过config参数向Servlet传递配置信息。
   可以通过config获取web应用程序的初始化参数，还可以获取ServletContext对象
   ```

2. service\(ServletRequest req,ServletResponse res\)

   ```
   容器调用service()方法处理客户端的请求，在调用service()方法被容器调用之前，
   必须确保Init()方法正确完成，容器会构造一个ServletRequest对象和ServletResponse对象作为参数传递给service()方法。
   ```

3. destroy\(\);

   ```
   当容器检测到一个Servlet对象应该从服务中被移除的时候，容器会调用该对象的destory方法，
   以便让Servlet对象可以释放它所使用的资源，保存数据到数据库中。
   ```

4. ServletConfig getServletConfig\(\)

   ```
   返回容器调用init()方法时传递给Servlet对象的ServletConfig对象
   ```

5. String getServletInfo\(\)

   ```
   返回一个String类型的字符串，包含了关于Servlet的信息，例如版权，作者等。
   ```

## 二、ServletRequest

### 1、概要

> ServletRequest 对象，用来封装请求数据

### 2、API介绍

1. Object getAttribute\(String name\)

   ```
    返回以name为名字的属性的值，如果不存在，返回null
   ```

2. Enumeration getAttributeNames\(\);

   ```
   返回请求中所有可用的属性的名字。返回一个枚举集合
   ```

3. void removeAttribute\(String name\);

   ```
   移除请求中名字为name的属性
   ```

4. void setAttribute\(String key,Object val\);

   ```
   在请求中保存一个键值对。
   ```

5. getCharacterEncoding\(\)

   ```
    返回请求正文使用的字符编码的名字
   ```

6. int getContentLength\(\)

   ```
   以字节为单位，返回请求正文的长度。
   ```

7. String getContentType\(\)

   ```
   返回正文的MIME类型
   ```

8. ServletInputStream getInputStream\(\)

   ```
   得到从请求的字节输入流
   ```

9. BufferedReader getReader\(\)

   ```
   从请求中得到字符输入流
   ```

10. String getLocalAddr\(\)

    ```
     返回接收到请求的网络接口的IP地址。
    ```

11. String getRemoteAddr\(\)

    ```
    返回发送请求的客户端或最后一个代理服务器的IP地址
    ```

12. String getRemoteHost\(\)

    ```
    返回发送请求的客户端或者最后一个代理服务器的完整限定名
    ```

13. String getLocalName\(\)

    ```
    返回接收到请求的IP接口的主机名
    ```

14. int getLocalPort\(\)

    ```
    返回接收到请求的网络接口的IP端口号
    ```

15. int getRemotePort\(\)

    ```
    返回发送请求的客户端或最后一个代理服务器的IP源端口。
    ```

16. String getParameter\(String name\)

    ```
    返回请求中name参数的值，如果name参数有多个值，该方法返回值列表中的第一个值。如果请求中没有找到该参数，返回null
    ```

17. Enumeration getParameterNames\(\)

    ```
    返回请求中包含的所有的参数的名字。
    ```

18. String\[\] getParameterValues\(String name\);

    ```
    返回请求中name参数所有的值
    ```

19. String getProtocal\(\)

    ```
    返回请求使用的协议的名字和版本 例如: HTTP/1.1
    ```

20. RequestDispatcher getRequestDispatcher\(String path\)

    ```
    返回RequestDispather对象，作为path所定位的资源的封装。
    ```

21. String getServerName\(\)

    ```
    返回请求发送到服务器的主机名
    ```

22. int getServerPort\(\)

    ```
    返回请求发送到服务器的端口号
    ```

23. setCharacterEncoding\(String env\)

    ```
    覆盖在请求正文中所使用的字符编码的名字。
    ```

## 三、ServletResponse

### 1、概要

> Servlet通过ServletResponse对象来生成响应结果

### 2、API介绍

1. flushBuffer\(\)

   ```
   强制把任何在缓存中的内容发送到客户端
   ```

2. int getBufferSize\(\)

   ```
   返回实际用于响应的缓存的大小，如果没有使用缓存，这个方法返回0
   ```

3. String getCharacterEncoding\(\)

   ```
   返回响应中发送的正文所使用的字符编码（MIME字符集）
   ```

4. String getContentType\(\)

   ```
   返回响应中发送的正文所使用的MIME类型
   ```

5. ServletOutputStream getOutputStream\(\)

   ```
   返回字节输出流
   ```

6. PrintWriter getWriter\(\)

   ```
   返回字符输出流
   ```

7. boolean isCommitted\(\)

   ```
   返回一个布尔值，指示是否已经提交了响应。
   ```

8. void reset\(\)

   ```
   清除在缓存中的任何数据，包括状态代码和消息报头。
   ```

9. void resetBuffer\(\)

   ```
   清除在缓存中的响应内容，保留状态代码和消息包头。
   ```

10. void setBufferSize\(int size\)

    ```
    设置响应正文的缓存大小。
    ```

11. void setCharacterEncoding\(String charset\)

    ```
    设置发送到客户端的响应的字符编码
    ```

12. void setContentLength\(int len\)

    ```
    设置内容正文的长度
    ```

13. void setContentType\(String type\)

    ```
    设置要发送到客户端的响应的内容类型。例如："text/html;charset=UTF-8"
    ```

## 四、ServletConfig

### 1、概要

​    Servlet容器使用ServletConfig对象在Servlet初始化期间向它传递配置信息，一个ServletConfig对象。

### 2、API介绍

1. String getInitParameter\(String name\);

   ```
   返回名字为name的初始化参数的值，初始化参数在web.xml配置文件中进行配置。
   ```

2. Enumeration getInitParameterNames\(\)

   ```
   返回Servlet所有初始化参数的名字的枚举集合。
   ```

3. ServletContext getServletContext\(\)

   ```
   返回Servlet上下文对象的引用。
   ```

4. String getServletName\(\)

   ```
   返回Servlet实例的名字，这个名字是web应用程序的部署描述符中指定。
   ```

## 五、HttpServlet\(重点\)

#### 1、概要

> HttpServlet类是GenericServlet类的子类。HttpServlet类为Serlvet接口提供了与HTTP协议相关的通用实现，也就是说，HttpServlet对象适合运行在与客户端采用HTTP协议通信的Servlet容器或者Web容器中。
>
> **在我们自己开发的Java Web应用中，自定义的Servlet类一般都扩展自HttpServlet类**。
>
> HttpServlet类实现了Servlet接口中的service\(ServletRequest , ServletResponse\)方法，而该方法实际调用的是它的重载方法HttpServlet.service\(HttpServletRequest, HttpServletResponse\)；
>
> 在上面的重载service\(\)方法中，首先调用HttpServletRequest类型的参数的getMethod\(\)方法，获得客户端的请求方法，然后根据该请求方式来调用匹配的服务方法；如果为GET方式，则调用doGet\(\)方法，如果为POST方式，则调用doPost\(\)方法。
>
> HttpServlet类为所有的请求方式，提供了默认的实现doGet\(\),doPost\(\),doPut\(\),doDelete\(\)方法；这些方法的默认实现都会向客户端返回一个错误。
>
> **对于HttpServlet类的具体子类，一般会针对客户端的特定请求方法，覆盖HttpServlet类中的相应的doXXX方法**。如果客户端按照GET或POST方式请求访问HttpsServlet，并且这两种方法下，HttpServlet提供相同的服务，那么可以只实现doGet\(\)方法，并且让doPost\(\)方法调用doGet\(\)方法。

## 六、HttpServletRequest\(重点\)

#### 1、概要

> HttpServletRequest接口最常用的方法就是获得请求中的参数，这些参数一般是客户端表单中的数据。同时，HttpServletRequest接口可以获取由客户端传送的名称，也可以获取产生请求并且接收请求的服务器端主机名及IP地址，还可以获取客户端正在使用的通信协议等信息。下表是接口HttpServletRequest的常用方法

#### 2、常用API介绍

| 方    法 | 说    明 |
| --- | --- |
| getAttributeNames\(\) | 返回当前请求的所有属性的名字集合 |
| getAttribute\(String name\) | 返回name指定的属性值 |
| getCookies\(\) | 返回客户端发送的Cookie |
| getsession\(\) | 返回和客户端相关的session，如果没有给客户端分配session，则返回null |
| getsession\(boolean create\) | 返回和客户端相关的session，如果没有给客户端分配session，则创建一个session并返回 |
| getParameter\(String name\) | 获取请求中的参数，该参数是由name指定的 |
| getParameterValues\(String name\) | 返回请求中的参数值，该参数值是由name指定的 |
| getCharacterEncoding\(\) | 返回请求的字符编码方式 |
| getContentLength\(\) | 返回请求体的有效长度 |
| getInputStream\(\) | 获取请求的输入流中的数据 |
| getMethod\(\) | 获取发送请求的方式，如get、post |
| getParameterNames\(\) | 获取请求中所有参数的名字 |
| getProtocol\(\) | 获取请求所使用的协议名称 |
| getReader\(\) | 获取请求体的数据流 |
| getRemoteAddr\(\) | 获取客户端的IP地址 |
| getRemoteHost\(\) | 获取客户端的名字 |
| getServerName\(\) | 返回接受请求的服务器的名字 |
| getServerPath\(\) | 获取请求的文件的路径 |

#### 3、详解

ServletRequest代表一个HTTP请求，请求在内存中是一个对象，这个对象是一个容器，可以存放请求参数和属性。

1. 请求对象何时被创建，当通过URL访问一个JSP或者Servlet的时候，也就是当调用Servlet的service\(\)、doPut\(\)、doPost\(\)、doXxx\(\)方法时候的时候，执行Servlet的web服服务器就自动创建一个ServletRequest和ServletResponse的对象，传递给服务方法作为参数。

2. 请求对象由Servlet容器自动产生，这个对象中自动封装了请求中get和post方式提交的参数，以及请求容器中的属性值，还有http头等等。当Servlet或者JSP得到这个请求对象的时候，就知道这个请求时从哪里发出的，请求什么资源，带什么参数等等。

3. ServletRequest的层次结构

   javax.servlet.ServletRequest  
   javax.servlet.http.HttpServletRequest

4. 通过请求对象，可以获得Session对象和客户端的Cookie。

5. 请求需要指定URL，浏览器根据URL生成HTTP请求并发送给服务器，请求的URL有一定的规范：

## 七、HttpServletResponse\(重点\)

#### 1、概要

> 在Servlet中，当服务器响应客户端的一个请求时，就要用到HttpServletResponse接口。设置响应的类型可以使用setContentType\(\)方法。发送字符数据，可以使用getWriter\(\)返回一个对象。下表是接口HttpServletResponse的常用方法

#### 2、API介绍

| 方    法 | 说    明 |
| --- | --- |
| addCookie\(Cookie cookie\) | 将指定的Cookie加入到当前的响应中 |
| addHeader\(String name,String value\) | 将指定的名字和值加入到响应的头信息中 |
| containsHeader\(String name\) | 返回一个布尔值，判断响应的头部是否被设置 |
| encodeURL\(String url\) | 编码指定的URL |
| sendError\(int sc\) | 使用指定状态码发送一个错误到客户端 |
| sendRedirect\(String location\) | 发送一个临时的响应到客户端 |
| setDateHeader\(String name,long date\) | 将给出的名字和日期设置响应的头部 |
| setHeader\(String name,String value\) | 将给出的名字和值设置响应的头部 |
| setStatus\(int sc\) | 给当前响应设置状态码 |
| setContentType\(String ContentType\) | 设置响应的MIME类型 |

#### 3、详解

1. ServletResponse 也是由容器自动创建的，代表Servlet对客户端请求的响应，响应的内容一般是HTML，而HTML仅仅是响应内容的一部分。请求中如果还包含其他资源会依次获取，如页面中含有图片，会进行第二个http请求用来获得图片内容。
2. 相应对象有以下功能：
   1. 向客户端写入Cookie
   2. 重写URL
   3. 获取输出流对象，向客户端写入文本或者二进制数据
   4. 设置响应客户端浏览器的字符编码类
   5. 设置客户端浏览器的MIME类型。

## 八、RequestDispatcher接口\(重点\)

### 1、概要

> ​该对象由Servlet容器创建，用于封装一个由路径标识的服务器资源。利用该对象可以将请求转发给其他的Servlet或jsp页面。

### 2、常用API介绍

1. forword\(ServletRequest req,ServletResponse resp\)

   ```
   该方法用于将请求从一个Servlet传递给服务器上的另外的Servlet，jsp或者html文件，在forword方法调用之后，
   之前在响应缓存中没有提交的内容将会被自动清除。该方法将请求转发给其他Servlet，
   将又被调用的Servlet负责对请求做出响应，原先Servlet执行终止。
   ```

2. include\(ServletRequest req,ServletResponse resp\)

   ```
          该方法用于将请求转发给其他Servlet，被调用的Servlet对该请求做出的响应将并入原来的响应对象中，原先的Servlet还可以继续输出响应信息。
   ```

3. forword\(\),include\(\)

   ```
   属于内部请求，浏览器地址不发生改变，客户端仅向服务器端发送一次请求
   ```

4. sendRedirect\(\)

   ```
   该方法由HttpServletResponse对象提供，
   表示重定向到另外一个请求中。属于外部请求。重定向相当于重新向服务器发送请求，浏览器地址发送改变
   ```



