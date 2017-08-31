## 会话跟踪\(重点\)

## 一、什么是会话跟踪

> HTTP协议本身是基于请求/响应模式的，无状态的协议。也就是说，当客户端的请求到来，服务器端做出响应之后，连接就被关闭了。用户活动发送在多个请求和响应之中，作为web服务器来说，必须采用一种机制唯一的标识一个用户，同时记录该用户的状态
>
> Cookie通过在客户端记录信息确定用户身份，Session通过在服务器端记录信息确定用户身份
>
> 1. Session技术：会话数据保存在服务器端
> 2. Cookie技术: 会话数据保存在浏览器客户端。

## 二、Session

### 1、概要

> session 机制是⼀一种服务器器端的机制，服务器器使⽤用⼀一种类似于散列列表的结构 （也可能就是使⽤用散列列表）来保存信息。
>
> 当程序需要为某个客户端的请求创建⼀一个 session 的时候， 服务器首先检查 这个客户端的请求⾥是否已包含了⼀个 session 标识 - 称为 session id ，如果 已包含一个 session id 则说明以前已经为此客户端创建过 session ，服务器就 按照 session id 把这个 session 检索出来使⽤用（如果检索不到，可能会新建一 个），如果客户端请求不包含 session id ，则为此客户端创建一个 session 并 且⽣成一个与此 session 相关联的 session id ，session id 的值应该是⼀个既 不会重复，又不容易被找到规律以仿造的字符串，这个 session id 将被在本次 响应中返回给客户端保存.

### 2、如何在客服端保存有三种方式

1. 通过cookie保存\(有可能被用户禁用\)
2. 隐藏在表单中保存
3. 重定向保存

### 3、获取session

```java
/**
 *返回此次请求相关联的Session,如果没有客户端分配Session,则创建一个新的Session
 */
HttpSession session = HttpServletRequest.getHttpSession();
/**
 *返回此次请求相关联的Session,如果没有客户端分配Session,当create为true，创建一个新的
 *Session,如果create为false，返回null
 */
HttpSession session = HttpSession getSession(boolean create);
```

### 4、常用API

| 返回值 | 方法名 | 说明 |
| :--- | --- | --- |
| void | setAttribute\(String name,Object val\) | 保存信息 |
| Object | getAttribute\(String name\) | 读取信息 |
| Enumeration | getAttributeNames\(\) | 获取所有session中的name |
| void | removeAttribute\(String name\) | 移除对应的信息 |
| long | getCreationTime\(\) | 获取Session创建的时间 |
| String | getId\(\) | 返回分配给Session的唯一标识符 |
| long | getLastAccessedTime\(\) | 返回客户端最后一次发送与Session相关的请求的时间 |
| long | getMaxInactiveInterval\(\) | getMaxInactiveInterval\(\) 返回以毫秒为单位的最大间隔时间，这个时间是Servlet\(容器在客户的两个连续请求之间保持Session打开的最大时间间隔，如果超过这个事件，Servlet容器将使Session失效 |
| void | setMaxInactiveInterval\(int interval\) | setMaxInactiveInterval\(int interval\) 如果设置一个负值，表示Session永远不会失效 |
| ServletContext | getServletContext\(\) | 返回Session所属的ServletContext对象 |
| void | invalidate\(\) | invalidate\(\) 使会话失效 |

### 5、使用步骤

1. 实例化Seesion对象

   ```java
   //得到session对象。如果得不到session对象，返回null。主要用于判断是否可以得到session对象的
   HttpSession session = request.getSession()  
   //创建或得到session对象。如果得不到session对象，创建 新的session对象。主要是用于创建session对象的。
   HttpSession session = request.getSession(boolean create)
   ```

2. HttpSession作为域对象保存会话数据

   ```java
   //保存数据
   void setAttribute(java.lang.String name, java.lang.Object value)  
   //得到数据
   Object getAttribute(java.lang.String name)     
   //清除数据
   void removeAttribute(java.lang.String name)
   ```

### 6、生命周期

1. session对象什么创建？

   ```
   执行request.getSession()方法时
   ```

2. session对象什么销毁？

   > 默认情况下，session对象在30分钟之后服务器自动销毁。
   >
   > 手动设置session有效时长  
   > void setMaxInactiveInterval\(int interval\) -以秒为单位

## 三、cookie

### 1、概要

> 一种会话数据管理技术，该技术把会话数据保存在浏览器客户端
>
> Cookie具有不可跨域名性
>
> Cookies 是一中由服务器发送给客户的片段信息，存储在客户端浏览器的内容中或硬盘上在客户随后对该服务器的请求中发回它，Cookies以键值对的形式记录会话跟踪的内容，服务器利用响应报头 Set-Cookie来发送Cookie信息，在Servlet规范中，用于会话跟踪的Cookie的名字必须是JSESSIONID。由于涉及隐私权和安全性方面的问题，用户在使用浏览器时，可以选择禁用Cookie，web服务器就无法利用Cookie跟踪用户的会话了，要解决这个问题需要重写URL服务器可以根据Cookie来跟踪客户状态，这对于需要区别客户的场合（如电子商务,推荐等功能）特别有用

### 2、cookie的构成

> Cookie 的内容主要包括
>
> 1. 名字
> 2. 值
> 3. 过期时间
> 4. 路径和域
>    * 域和路径的主要作用限制cookie的使用范围 
>    * 域指的 baidu.com
>    * 路径就是跟在域名后面的 URL 路径，比如 / app /login 等等

### 3、API列表

| 方法 | &描述 |
| --- | --- |
| public void setDomain\(String pattern\) | 该方法设置 cookie 适用的域，例如xxx.com。 |
| public String getDomain\(\) | 该方法获取 cookie 适用的域，例如 xxx.com。 |
| public void setMaxAge\(int expiry\) | 该方法设置 cookie 过期的时间（以秒为单位）。如果不这样设置，cookie 只会在当前 session 会话中持续有效。 |
| public int getMaxAge\(\) | 该方法返回 cookie 的最大生存周期（以秒为单位），默认情况下，-1 表示 cookie 将持续下去，直到浏览器关闭。 |
| public String getName\(\) | 该方法返回 cookie 的名称。名称在创建后不能改变。 |
| public void setValue\(String newValue\) | 该方法设置与 cookie 关联的值。 |
| public String getValue\(\) | 该方法获取与 cookie 关联的值。 |
| public void setPath\(String uri\) | 该方法设置 cookie 适用的路径。如果您不指定路径，与当前页面相同目录下的（包括子目录下的）所有 URL 都会返回 cookie。 |
| public String getPath\(\) | 该方法获取 cookie 适用的路径。 |
| public void setSecure\(boolean flag\) | 该方法设置布尔值，表示 cookie 是否应该只在加密的（即 SSL）连接上发送。 |
| public void setComment\(String purpose\) | 设置cookie的注释。该注释在浏览器向用户呈现 cookie 时非常有用。 |
| public String getComment\(\) | 获取 cookie 的注释，如果 cookie 没有注释则返回 null。 |

### 4、使用

##### 1、工作流程

1. 首先浏览器向服务器发出请求。
2. 服务器就会根据需要生成一个Cookie对象，并且把数据保存在该对象内。
3. 然后把该Cookie对象放在响应头，一并发送回浏览器。
4. 浏览器接收服务器响应后，该Cookie保存在浏览器端。
5. 当下一次浏览器再次访问那个服务器，就会把这个Cookie放在请求头内一并发给服务器
6. 服务器从请求头提取出该Cookie，判别里面的数据，然后作出相应的动作。

##### 2、Cookie操作

1. 写Cookie

   > 1&gt; 实例化cookie对象  
   >   Cookie cookie = new Cookie\("user", "xiaozhang"\);  
   > 2&gt; 添加进响应发送给客服端  
   >   resp.addCookie\(cookie\);  
   > 3&gt;当servle向客服端写cookie的时候,可以通过Cookie类的setMaxAge\(int expiry\)来设置cookie的有效期,下面是参数说明  
   >   3.1&gt; 如果expiry &gt; 0，就指示浏览器在客户端硬盘上保存Cookie的时间为expriy秒。  
   >   3.2&gt; 如果expiry = 0，就指示浏览器删除当前Cookie。  
   >   3.3&gt; 如果expiry &lt; 0，就指示浏览器不要把Cookie保存到客户端硬盘。Cookie仅仅存在于当前浏览器进程中，当浏览器进程关闭，Cookie也就消失。  
   >   3.4&gt; Cookie默认的有效期为-1。对于来自客户端的Cookie，Servlet可以通过Cookie类的getMaxAge\(\)方法来读取Cookie的有效期

2. 读写客服端Cookie

   ```
      通过HttpServletRequest对象
      Cookie[] cookies = request.getCookies();
           if (cookies != null) {
               for (Cookie cookie : cookies) {
                   //获得cookie的key
                   String name = cookie.getName();
                   // 获得cookie对应的name的值
                   String value = cookie.getValue();
                   //获得cookie的有效期
                   int maxAge = cookie.getMaxAge();
               }
           } else {
               System.out.println("没有cookie");
           }
   ```

3. 修改cookie

   ```java
   Cookie updateCookie = null; 
   Cookie[] cookies = request.getCookies();
           if (cookies != null) {
               for (Cookie cookie : cookies) {
                   if (cookie.getValue().equals("xiaoming")) {
                      //修改Cookie的值
                       cookie.setValue("laowang");
                   }
               }
           }
    if (updateCookie != null) {
        //通过响应添加返回给客服端
    }
   ```

4. 删除cookie

   ```
   cookie.setMaxAge(0);
   ```

## 四、其它

### 1、Cookie和Session的区别：

1. **Cookie中只能保存ASCII字符串，Session中可以保存任意类型的数据**，甚至Java Bean乃至任何Java类、对象等
2. **隐私策略不同**。Cookie存储在客户端，对客户端是可见的，可被客户端窥探、复制、修改。而Session存储在服务器上，不存在敏感信息泄露的风险
3. **有效期不同**。Cookie的过期时间可以被设置很长。Session依赖于名为JSESSIONI的Cookie，其过期时间默认为-1，只要关闭了浏览器窗口，该Session就会过期，因此Session不能完成信息永久有效。如果Session的超时时间过长，服务器累计的Session就会越多，越容易导致内存溢出。
4. **服务器压力不同**。每个用户都会产生一个session，如果并发访问的用户过多，就会产生非常多的session，耗费大量的内存。因此，诸如Google、Baidu这样的网站，不太可能运用Session来追踪客户会话。
5. **浏览器支持不同**。Cookie运行在浏览器端，若浏览器不支持Cookie，需要运用Session和URL地址重写。
6. **跨域支持不同**。Cookie支持跨域访问（设置domain属性实现跨子域），Session不支持跨域访问



