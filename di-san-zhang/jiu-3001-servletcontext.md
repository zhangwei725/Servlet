# ServletContext详解

## 一、什么是Servlet上下文

ServletContext,是一个全局的储存信息的空间，服务器开始，其就存在，服务器关闭，其才释放。

Request，一个用户可有多个；session，一个用户一个；而servletContext，所有用户共用一个。所以，为了节省空间，提高效率，ServletContext中，要放必须的、重要的、所有用户需要共享的线程又是安全的一些信息。

换一种方式说吧，运行在Java虚拟机中的每一个Web应用程序都有一个与之相关的Servlet上下文。ServletContext对象是Web服务器中的一个已知路径的根，Servlet上下文被定位于[http://localhost:8080/appname](http://localhost:8080/appname). 以 /项目名 请求路径（称为上下文路径）开始的所有请求被发送到与此ServletContext关联的Web应用程序。一个ServletContext对象表示了一个Web应用程序的上下文

Servlet上下文提供对应用程序中所有Servlet所共有的各种资源和功能的访问。Servlet上下文API用于设置应用程序中所有Servlet共有的信息。Servlet可能需要共享他们之间的共有信息。运行于同一服务器的Servlet有时会共享资源，如JSP页面、文件和其他Servlet

举例：

​    比如，做一个购物类的网站，要从数据库中提取物品信息，如果用session保存这些物品信息，每个用户都访问一边数据库，效率就太低了；所以要用来Servlet上下文来保存，在服务器开始时，就访问数据库，将物品信息存入Servlet上下文中，这样，每个用户只用从上下文中读入物品信息就行了

​创建时机：加载web应用时创建ServletContext对象。

​得到对象： 从ServletConfig对象的getServletContext方法得到。

## 二、开发中常用场景

### 1、数据共享

1. 多个web组件之间使用它实现数据共享

   ```
     ServletContext context = this.getServletContext();  //servletContext域对象
     context.setAttribute("data", "共享数据"); //向域中存了一个data属性
     在另一个servlet中，可以使用如下语句来获取域中的data属性
     ServletContext context = this.getServletContext();
     String value = (String) context.getAttribute("data");  //获取域中的data属性
   ```

   > 注意事项
   >
   > 当要给jsp传递数据的时候千万不能用上面的方式 。用 ServletContext来存放数据，然后再根据ServletContext来获取。 这样有线程安全的问题。因为所有的访问都是用的同一个ServletContext 后面的可能会把之前的覆盖掉应该使用request方法

2. 通过servletContext对象获取到整个web应用的配置信息

   ```
     String url = this.getServletContext().getInitParameter("url");
     String username = this.getServletContext().getInitParameter("username");
     String password = this.getServletContext().getInitParameter("password");
   ```

3. 数据转发

   ```
   this.getServletContext().setAttribute("data","serlvet数据转发");
   RequestDispatcher rd = this.getServletContext().getRequestDispatcher("/shop.jsp");
   rd.forward(request, response);
   ```

### 2、通过servletContext对象读取资源文件

> 在实际开发中，用作资源文件的文件类型，通常是：xml、properties，而读取xml文件必然要进行xml文档的解析，所以以下例子只对properties文件进行读取\(在一个web工程中，只要涉及到写地址，建议最好以/开头\)  在web工程中，我们一般来说，是不能采用传统方式读取配置文件的，因为相对的是jvm的启动目录\(tomcat的bin目录\)，所以我们要使用web绝对目录来获取配置文件的地址

1. 使用ServletContext的getResourceAsStream方法：返回资源文件的读取字节流

   ```java
   InputStream in = this.getServletContext().getResourceAsStream("/WEB-INF/classes/db.properties");
   Properties prop = new Properties();  
   prop.load(in);
   String url = prop.getProperty("url");
   ```

1. 使用ServletContext的getRealPath方法，获得文件的完整绝对路径path，再使用字节流读取path下的文件

   ```java
     String path = this.getServletContext().getRealPath("/WEB-INF/classes/db.properties");

        String filename = path.substring(path.lastIndexOf("\")+1); 

        //相比第一种方法的好处是：除了可以获取数据，还可以获取资源文件的名称

        FileInputStream in = new FileInputStream(path);

        Properties prop = new Properties();

        prop.load(in);

        String url = prop.getProperty("url");
   ```

1. 使用ServletContext的getResource方法，获得一个url对象，调用该类的openStream方法返回一个字节流，读取数据

   ```java
     URL url = this.getServletContext().getResource("/WEB-INF/classes/db.properties");
     InputStream in = url.openStream();
     Properties prop = new Properties();
     prop.load(in);
     String url1 = prop.getProperty("url");
   ```

2. 不同位置的资源文件的读取方式

   1. 当资源文件在包下面时

      ```
      InputStream in = this.getServletContext().getResourceAsStream("/WEB-INF/classes/cn/werner/context/db.properties");
      ```

   2. 资源文件在WEB-INF下

      ```
      InputStream in = this.getServletContext().getResourceAsStream("/WEB-INF/db.properties");
      ```

   3. 资源文件在web工程中

      ```java
      InputStream in = this.getServletContext().getResourceAsStream("/db.properties");
      ```

## 三、其它

### 1、得到当前web应用的路径

> java.lang.String getContextPath\(\)

### 2、得到web应用的初始化参数

> java.lang.String getInitParameter\(java.lang.String name\)
>
> java.util.Enumeration getInitParameterNames\(\)

### 3、域对象有关的方法

> void setAttribute\(java.lang.String name, java.lang.Object object\)
>
> java.lang.Object getAttribute\(java.lang.String name\)
>
> void removeAttribute\(java.lang.String name\)

### 4、转发（类似于重定向）

> RequestDispatcher getRequestDispatcher\(java.lang.String path\)

### 5、得到web应用的资源文件

> java.lang.String getRealPath\(java.lang.String path\)
>
> java.io.InputStream getResourceAsStream\(java.lang.String path\)



