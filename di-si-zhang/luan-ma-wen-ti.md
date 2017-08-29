# 乱码问题解决方案汇总

## 一、输出乱码问题

### 1、 response对象输出中文，产生乱码。

#### 1.1、解决方案

> 1. response.setHeader\("Content-Type", "text/html;charset=UTF-8"\);
> 2. 获取字符串的byte数组采用的编码    "乱码文字".getByte\("utf-8"\);

#### 1.2、解决方法

> 1. 设置浏览器打开文件时采用的编码
>    response.setHeader\("Content-Type", "text/html;charset=UTF-8"\);
> 2. 设置response缓冲区的编码
>    response.setCharacterEncoding\("UTF-8"\);
> 3. 简写的方式（等于上面的两句）
>    response.setContentType\("text/html;charset=UTF-8"\);

#### 1.3、输入中文乱码

> str=URLDecoder.decode\("中文","utf8"\)

### 2、请求乱码

#### 1、get提交方式的乱码处理方式

##### 1.1、说明

> 如果使用get方式提交中文，接受参数的页面也会出现乱码，这个乱码的原因也是tomcat的内部    编码格式iso8859-1导致。Tomcat会以get的缺省编码方式iso8859-1对汉字进行编码，编码后追加到url，导致接受页面得到的参数为乱码。

##### 1.2、解决办法

1. 对接受到的字符进行解码，再转码。
2. Get走的是url提交，而在进入url之前已经进行了iso8859-1的编码处理。要想影响这个编码则需要在server.xml的Connector节点增加useBodyEncodingForURI=”true”属性配置，即可控制tomcat对get方式的汉字编码方式，上面这个属性控制get提交也是用request.setCharacterEncoding \(“UTF-8”\)所设置的编码格式进行编码。所以自动编码为utf-8，接受页面正常接受就可以了。

#### 2、使用Post方式提交后接收到的乱码问题

##### 2.1、 说明

​    通过jsp，html，或servlet中的表单元素把参数提交给对应的jsp或者servlet时，在接收的jsp或servlet中接收到的参数中文显示乱码

##### 2.2、解决办法

1. 接受参数时进行编码转换 String str = new String\(request.getParameter\(“something”\).getBytes\(“ISO-8859-1”\),”utf-8”\) ； 这样的话，每一个参数都必须这样进行转码。很麻烦。但确实可以拿到汉字。

2. 接受此参数的页面，执行请求的编码代码， request.setCharacterEncoding\(“UTF-8”\)，把提交内容的字符集设为UTF－8。这样的话，直接使用 String str = request.getParameter\(“UTF－8”\)；即可得到汉字参数。但每页都需要执行这句话。这个方法也就对post提交的有效 果，对于get提交和上传文件时的enctype=”multipart/form-data”是无效的。稍后下面单独对这个两个的乱码情况再进行说明。

3. 为了避免每页都要写request.setCharacterEncoding\(“UTF-8”\)，建议使用过滤器对所有jsp进行编码处理。\(推荐方案\)

   1、对请求过滤

   1. 在web.xml里面加

      ```
       <filter>
              <filter-name>char</filter-name>
              <filter-class>CharacterFilter</filter-class>
       </filter>
       <filter-mapping>
              <filter-name>char</filter-name>
              <url-pattern>/*</url-pattern>
       </filter-mapping>
      ```

   2. java代码\`

      ```
      public class CharacterFilter implements Filter {
           public void init(FilterConfig filterConfig) throws ServletException {

          }
           public void doFilter(ServletRequest request, ServletResponse response,FilterChain chain) throws IOException, ServletException {
              request.setCharacterEncoding("UTF-8");
              chain.doFilter(request, response);
          }
            public void destroy() {

          }
      }
      ```

   2、对请求和输出过滤

   1. 在web.xml里面加

      ```
       <filter>
              <filter-name>char</filter-name>
              <filter-class>CharacterEncoding</filter-class>
       </filter>
       <filter-mapping>
              <filter-name>char</filter-name>
              <url-pattern>/*</url-pattern>
       </filter-mapping>
      ```

   2. java代码

      ```
      public class CharacterEncoding implements Filter {  
          protected FilterConfig filterConfig ;  
          String encoding = null;  

          public void destroy() {  
              this.filterConfig = null;  
          }  
          /** 
           * 初始化 
           */  
          public void init(FilterConfig filterConfig) {  
              this.filterConfig = filterConfig;  
          }  

          /** 
           * 将 inStr 转为 UTF-8 的编码形式 
           *  
           * @param inStr 输入字符串 
           * @return UTF - 8 的编码形式的字符串 
           * @throws UnsupportedEncodingException 
           */  
          private String toUTF(String inStr) throws UnsupportedEncodingException {  
              String outStr = "";  
              if (inStr != null) {  
                  outStr = new String(inStr.getBytes("iso-8859-1"), "UTF-8");  
              }  
              return outStr;  
          }  

          /** 
           * 中文乱码过滤处理 
           */  
          public void doFilter(ServletRequest servletRequest,  
                  ServletResponse servletResponse, FilterChain chain) throws IOException,  
                  ServletException {  
              HttpServletRequest request = (HttpServletRequest) servletRequest;  
              HttpServletResponse response = (HttpServletResponse) servletResponse;  

              // 获得请求的方式 (1.post or 2.get), 根据不同请求方式进行不同处理  
              String method = request.getMethod();  
              // 1. 以 post 方式提交的请求 , 直接设置编码为 UTF-8  
              if (method.equalsIgnoreCase("post")) {  
                  try {  
                      request.setCharacterEncoding("UTF-8");  
                  } catch (UnsupportedEncodingException e) {  
                      e.printStackTrace();  
                  }  
              }  
              // 2. 以 get 方式提交的请求  
              else {  
                  // 取出客户提交的参数集  
                  Enumeration<String> paramNames = request.getParameterNames();  
                  // 遍历参数集取出每个参数的名称及值  
                  while (paramNames.hasMoreElements()) {  
                      String name = paramNames.nextElement(); // 取出参数名称  
                      String values[] = request.getParameterValues(name); // 根据参数名称取出其值  
                      // 如果参数值集不为空  
                      if (values != null) {  
                          // 遍历参数值集  
                          for (int i = 0; i < values.length; i++) {  
                              try {  
                                  // 回圈依次将每个值调用 toUTF(values[i]) 方法转换参数值的字元编码  
                                  String vlustr = toUTF(values[i]);  
                                  values[i] = vlustr;  
                              } catch (UnsupportedEncodingException e) {  
                                  e.printStackTrace();  
                              }  
                          }  
                          // 将该值以属性的形式藏在 request  
                          request.setAttribute(name, values);  
                      }  
                  }  

              }  
              // 设置响应方式和支持中文的字元集  
              response.setContentType("text/html;charset=UTF-8");  
              // 继续执行下一个 filter, 无一下个 filter 则执行请求  
              chain.doFilter(request, response);  
          }  
      }
      ```

# 三、JavaScript乱码问题

### 1、概要

​    使用javascript编码不给浏览器插手的机会，编码之后再向服务器发送请求，然后在服务器中解码。在掌握该方法的时候，Javascript编码的三个方法：escape\(\)、encodeURI\(\)、encodeURIComponent\(\)

### 2、 解决方案

#### 2.1、 encodeURI

> 对整个URL进行编码，它采用的是UTF-8格式输出编码后的字符串。不过encodeURI除了ASCII编码外对于一些特殊的字符也不会进行编码如：! @ \# $& \* \( \) = : / ; ? + '。

#### 2.2、encodeURIComponent

> 把URI字符串采用UTF-8编码格式转化成escape格式的字符串。相对于encodeURI，encodeURIComponent会更加强大，它会对那些在encodeURI\(\)中不被编码的符号（; / ? : @ & = + $ , \#）统统会被编码。但是encodeURIComponent只会对URL的组成部分进行个别编码，而不用于对整个URL进行编码。对应解码函数方法decodeURIComponent

## 四、jsp 与 Servlet

### 4.1 、浏览器显示乱码

```
<%@ page language="java" import="java.util.*" 
    contentType="text/html; charset=utf-8" %>
<html>  
    <head>  
    <title>中文显示示例</title>
    </head>   
    <body>  
    这是一个中文显示示例：  
    <%  
        String str = "带你装逼带你飞";  
        out.print(str);  
    %>  
    </body>  
</html>
```

### 4.2、  生成Servlet乱码

```
<%@ page language="java" import="java.util.*"  
pageEncoding="utf-8" %>  
<html>  
    <head>  
    <title>中文显示示例</title>  
    </head>   
    <body>  
    这是一个中文显示示例：  
    <%  
        String str = "中文";  
        out.print(str);  
    %>  
    </body>  
</html>
```

### 4.3、请求乱码

参照乱码解决2

## 五、HTML乱码问题

```
<html>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
<head>
等同于H5的
<!DOCTYPE HTML>
<html>
<meta charset="UTF-8">
<head>
```

## 六、数据库中的中文

### 1、说明

> 大部分数据库都支持以unicode编码方式，所以解决Java与数据库之间的乱码问题比较明智的方式是直接使用unicode编码与数据库交互。很多数据库驱动自动支持unicode，如Microsoft的SQLServer驱动。其他大部分数据库驱动，可以在驱动的url参数中指定，

### 2、数据库相关

1. 尽量在数据库安装过程中会选择编码方式，此时选择utf-8格式

2. 创建数据库

   > 在创建数据库的过程中  
   > CREATE DATABASE 数据库名 DEFAULT CHARACTER SET utf8 COLLATE utf8\_general\_ci;创建表
   >
   > 在创建表生成的SQL后面加上  
   > ENGINE=InnoDB DEFAULT CHARSET=utf8;

### 3、通过jdbc连接时候

1. mysql乱码

   ```
   jdbc:mysql://localhost:3306/数据库名字?useUnicode=true&amp;characterEncoding=UTF-8
   ```

## 七、文件编码

### 1、项目中的Java、jsp等文件均采用UTF-8编码

#### 1.1、Eclipse设置方式

```
Project --> Properties --> Resources --> Text file encoding 或者

Window --> Preferences --> General --> Workspace --> Text fileencoding
```

#### 1.2、IEDA 设置方式

```
File -->Setting --> Editor --> File Encodings --> 三个地方全部使用改成  
[Global Encoding]
[Project Encoding] 
[default  Encoding for properties files ] 最下面
```

#### 1.3、Jsp文件中pageEncoding与contentType属性

一般地，我们都会在jsp文件的开头加上这样的声明：

> &lt;%@ page language="java"pageEncoding="UTF-8" contentType="text/html; charset=UTF-8"%&gt;
>
> jsp文件的执行过程如下：  
> （1）jsp编译成.java。它会根据pageEncoding的设定编码格式读取jsp，结果是由指定的编码方案翻译成统一的UTF-8编码的java源码（即.java）。  
> （2）.java编译成.class。将第一步编译的UTF-8编码的java源码编译成UTF-8编码的二进制码（即.class）。  
> （3）Tomcat执行第二步编译好的二进制码，输出结果，并告诉浏览器我是以何种方式编码的（contentType的charset属性）。  
> 应该注意的是，contentType的默认设定为"text/html;charset=ISO-8859-1"，也就是说默认是ISO-8859-1编码格式。

## 八、Tomcat配置文件

> 对于以GET方式发送的请求中包含中文参数的情形，可能经常会出现乱码现象。这是因为Tomcat默认是按ISO-8859-1的方式对URL进行解码，而ISO-8859-1并未包括中文字符，这样的话中文字符肯定就不能被正确解析了。
>
> 解决方法：tomcat安装目录 --&gt; conf --&gt; server.xml，设置useBodyEncodingForURI或者URIEncoding属性：
>
> ```
> <Connector port="8080"protocol="HTTP/1.1" connectionTimeout="20000" 
> redirectPort="8443"useBodyEncodingForURI="true" 
> URIEncoding="UTF-8" />
> ```
>
> 注:URIEncoding与useBodyEncodingForURI的区别：
>
> 1. URIEncoding的作用是对GET请求中的参数按照设定的方式进行编码，如UTF-8；而        useBodyEncodingForURI="true"的作用是指定请求参数的编码采用请求体的编码方式。
> 2. 两个属性选其一进行配置即可。URIEncoding参数的配置具有全局性，它指定对所有GET方式请求进行统一的编解码；而useBodyEncodingForURI具有更大的灵活性，由于不用的页面可以采取不同的编码方式，因而请求参数也就可以随着页面编码方式的变化而变化

## 九、数据库乱码

### 1、安装数据的时候选择UTF-8

### 2、mysql数据库解决方案

#### 2.1、创建数据库的时候

```
CREATE   DATABASE  数据库名

CHARACTER   SET   'utf8 ' 

'utf8_general_ci ';
```

#### 2.2、建表的时候:

```
 CREATE   TABLE  表名( 

      ID  varchar(40)   NOT   NULL   default   ' ', 

      USER_ID   varchar(40)   NOT   NULL   default   ' ', 

 ) ENGINE=InnoDB   DEFAULT   CHARSET=utf8;
```

#### 2.3、连接数据库的时候

```
jdbc:mysql://localhost:3306/database?useUnicode=true&characterEncoding=UTF-8
```

### 3、Oracle数据库

1. 启动数据库 SQL&gt; Alter database open;
2. 修改字符集 SQL&gt;ALTER DATABASE CHARACTER SET AL32UTF8;
3. 重启数据库

## 十、避免乱码注意事项

> 1. 尽量使用统一的编码，如果你是重头开发一个系统，特别是Java开发的，推荐从页面到数据库再到配置文件都使用UTF-8进行编码，安全第一。
> 2. SetCharacterEncodingFilter的使用，这个东西不是万能的，但是没有它就会很麻烦，如果是基于Servlet开发的东西，能用的就给它用上，省心。不过有一个注意的地方，这个Filter只是对POST请求有效，GET一律忽略，不信你可以debug一下，看看它怎么做的，至于为什么不过滤get请求，好象是它对GET请求是无能为力的。
> 3. 就如上面所说，GET请求有问题，尽量使用POST请求，这个也是Web开发的一个基本要领：
> 4. JavaScript和Ajax乱码的避免，注意JavaScript默认是ISO8859的编码，避免JS/AJAX乱码和GET一样，不要在URL里面使用中文，实在避免不了，就只能在生成链接的时候转码，绝对不能想当然的认为SetCharacterEncodingFilter会帮你做什么事情。
> 5. 尽早统一开发环境，早点模拟真实环境测试，但凡软件开发都是这么干的，但仍然值得注意。我这出现过一次状况，程序是在Win下编译的，拿去Linux上测试没问题，等实际部署的时候代码是在Linux下编译，结果乱码，秋后算帐总觉得有点晚。



