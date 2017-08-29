## 一、概要

> Servlet体系是建立在Java多线程的基础之上的,它的生命周期是由Tomcat来维护的。当客户端第一次请求Servlet的时候,tomcat会根据web.xml配置文件实例化servlet，当又有一个客户端访问该servlet的时候，不会再实例化该servlet，也就是多个线程在使用这个实例

## 二、Servlet线程池

> Serlvet采用多线程来处理多个请求同时访问，Tomcat容器维护了一个线程池来服务请求。线程池实际上是等待执行代码的一组线程叫做工作组线程(Worker Thread)，Tomcat容器使用一个调度线程来管理工作组线程(Dispatcher Thead)
>
> 当容器收到一个Servlet请求，Dispatcher线程从线程池中选出一个工作组线程，将请求传递 给该线程，然后由该线程来执行Servlet的service方法。当这个线程正在执行的时候，容器收到另一个请求，调度者线程将从线程池中选出另外一个 工作组线程来服务则个新的请求，容器并不关心这个请求是否访问的是同一个Servlet还是另一个Servlet。当容器收到对同一个Servlet的多个请求的时候，那这个servlet的service方法将在多线程中并发的执行

## 三、Servlet线程安全问题

### 1、多线程和单线程Servlet具体区别：

1. 多线程下每个线程对局部变量都会有自己的一份copy，这样对局部变量的修改只会影响到自己的copy而不会对别的线程产生影响线程安全的。
2. 但是对于成员变量来说，由于servlet在Tomcat中是以单例模式存在的，所有的线程共享实例变量。多个线程对共享资源的访问就造成了线程不安全问题

### 2、模拟场景

```
 protected void doPost(HttpServletRequest request,  
            HttpServletResponse response) throws ServletException, IOException {  
        message = request.getParameter("message");  
        PrintWriter printWriter = response.getWriter();  
        try {  
            Thread.sleep(5000);  
        } catch (InterruptedException e) {  
            e.printStackTrace();  
        }  
        printWriter.write(message);  
    }  
```

### 3、常用来传递数据的对象线程安全问题

1. ServletContext

   > 它是线程不安全的，多线程下可以同时进行读写，因此我们要对其读写操作进行同步或者深度的clone。  

2. HttpSession

   > 同样是线程不安全的，和ServletContext的操作一样。 

3. ServletRequest

   > 它是线程安全的，对于每一个请求由一个工作线程来执行，都会创建一个ServletRequest对象，所以ServletResquest只能在一个线程中被访问，而且他只在service()方法内是 有效的

### 4、如何处理

1. 对于线程安全问题最简单的方式就是加锁:

   > 将存在线程安全问题的代码放到同步代码块中，这样线程访问时就需要排队拿到钥匙，只有上一个
   > 线程访问完毕，才会释放掉锁，先一个线程才可以进入。但是存在明显的缺点：就是效率太低了

2. 实现singleThreadModel接口

   > servlet实现singleThreadModel接口后，每个线程都会创建servlet实例，这样每个客户端请求就不存在共享资源的问题，但是servlet响应客户端请求的效率太低，在Servlet2.4中已不再提倡使用

3. 使用同步的集合类

   > 使用Vector代替ArrayList，使用Hashtable代替HashMap

4. 不要在Servlet中创建自己的线程来完成某个功能

   > Servlet本身就是多线程的，在Servlet中再创建线程，将导致执行情况复杂化，出现多线程安全问题