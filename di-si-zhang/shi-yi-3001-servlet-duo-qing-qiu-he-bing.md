# 单Servlet实现多请求

## 一、为什么要将Servlet合并

> 在实际应用过程中，经常遇到一次请求就要对应一个servlet，比如做一个简单的登录注册，那么登录对应一个servlet，注册也需要一个servlet，登录成功后，还需要添加，删除，编辑等等，对需要一个对应的servlet，那么有没有什么办法，可以把这些请求用一个servlet来处理，下面我们就来看看。

## 二、原理介绍:

### 1、分析

1. 我们一般使用的servlet是继承了HttpServlet，里边有两个方法，一个doGet一个是doPost
2. 服务器执行时，会调用里边的service方法根据请求方式调这两个方法，

### 2、具体实现思路

1. 根据这个我们可以改下service方法,我们改写了service方法，实现获取一个method的参数，获取到了

2. 使用前面学过的反射，因为后边写的servlet会继承这个改写后的servlet，使用需要拿到的类就是当前这个类，

3. 拿到这个类后，获取method参数的方法，如果没有就表示不存在这个方法，不能调用，存在的话，给两个参数，

4. 一个HttpServletreRuest.class和HttpServletResonse.class调用使用当前对象来调用，

5. 然后写一个servlet继承自定义的这个servlet把你的请求业务写成一个方法，

6. 例如：

   login\(HttpServletreRuest req, HttpServletResonse res \);

   register\(HttpServletreRuest req,HttpServletResonse res \);  等等 ，

7. 最后每次请求，只要给请求的servlet带一个 method= login 或者 method=register的参数，就会自动去调用对应的方法，这样就达到了，一个servlet处理多个请求

## 三、示例代码

1. 封装代码

   ```
   public class BaseServlet extends HttpServlet {  
       @Override  
       protected void service(HttpServletRequest req, HttpServletResponse resp)  
               throws ServletException, IOException {  
           String name = req.getParameter("method");//获取方法名  
       //其中method可以是可以任意取得，如getParameter("service")等  
           if(name == null || name.isEmpty()){  
               throw new RuntimeException("method parameter does not exist");  
           }  
           Class c = this.getClass();//获得当前类的Class对象  
           Method method = null;  
           try {  
               //获得Method对象  
               method =  c.getMethod(name,HttpServletRequest.class,HttpServletResponse.class);  
           } catch (Exception e) {  
           }  
           try {  
               method.invoke(this, req,resp);//反射调用方法  
           } catch (Exception e) {  
               throw new RuntimeException(e);  
           }  
       }  
   }
   ```

2. 继承代码

   ```
   http://localhost:8080/app/base?method=login
   http://localhost:8080/app/base?method=register
   public class AccountServlet extends BaseServlet {

       public void login(HttpServletRequest request, HttpServletResponse response) {
           //登陆的方法
       }

       public void register(HttpServletRequest request, HttpServletResponse response) {
   //            注册的方法
       }
   }
   ```

3. web.xml 注册

   ```
      <!--账号管理Servlet-->
       <servlet>
           <servlet-name>AccountServlet</servlet-name>
           <servlet-class>com.werner.demo.AccountServlet</servlet-class>
       </servlet>
       <servlet-mapping>
           <servlet-name>AccountServlet</servlet-name>
           <url-pattern>/account</url-pattern>
       </servlet-mapping>
   ```



