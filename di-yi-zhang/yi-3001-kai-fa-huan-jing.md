# Tomcat

## 一、简介

Tomcat可以运行Servlet和JSP的一个小型的轻量级应用服务器，运行时占用系统资源小、扩展性好、支持负载平衡与邮件服务等开发应用系统中的常用功能，适用于中小型系统和并发访问用户不太多的场合，是开发和调试JSP程序的首选。Tomcat是Sun的JSWDK（Java Server Web Development Kit）中的Servlet容器，属于Apache软件基金会（Apache Software Foundation）的Jakarta项目中的一个核心项目，由Apache、Sun和其他一些公司和个人共同开发而成。Tomcat既是一个开放源码、免费支持JSP和Servlet技术的容器，同时又是一个Web服务器软件，Servlet和JSP的最新规范都可以在Tomcat的新版本中得到实现

以下是Servlet、JSP、EL、omcat、JDK版本之间的关系

| Servlet | JSP  |  EL  | Tomcat | Java |
| :-----: | :--: | :--: | :----: | :--: |
|   4.0   | 2.4  | 3.1  | 9.0.x  |  8   |
|   3.1   | 2.3  | 3.0  | 8.5.x  |  7   |
|   3.1   | 2.3  | 3.0  | 8.0.x  |  7   |
|   3.0   | 2.2  | 2.2  | 7.0.x  |  6   |
|   2.5   | 2.1  | 2.1  | 6.0.x  |  5   |
|   2.4   | 2.0  | N/A  | 5.5.x  | 1.4  |
|   2.3   | 1.2  | N/A  | 4.1.x  | 1.3  |
|   2.2   | 1.1  | N/A  | 3.3.x  | 1.1  |

## 二、Tomcat下载、安装、启动

### 1、[下载](http://tomcat.apache.org/)

1. tar.gz（zip）文件是Linux操作系统下的安装版本
2. exe文件是Windows系统下的安装版本
3. zip文件是Windows系统下的压缩版本

### 2、安装

1. 解压到本地,注意解压缩Tomcat的目录不要含有空格或中文

### 3、启动

1. 在Tomcat的目录下，双击bin/startup.bat (如果使用linux 双击bin/startup.sh)。
2. 在浏览器地址栏输入 http://localhost:8080/ ，如果出现tomcat网站主页，则说明启动成功。

### 4、常见问题

1. 窗口一闪然后消失 

   > 记事本打开startup.bat 在文件末尾加入 pause指令，再次运行，读取错误原因，根据原因google解决


1. 端口占用问题（Tomcat的默认端口号为8080）

   > 发现端口被占用后，通过cmd命令行，查看占用端口进程，CMD命令：netstat -ano （xp win7 通用），找到8080端口号的进程，记住它的PID(进程标识符），然后在任务管理器中找到该PID，关闭该PID对应的进程。
   > 如果在任务管理器发现该占用8080端口的进程的PID是4，映像名称是System，这个进程无法关闭，如果出现这种情况证明了一个服务占用端口。这时候通过services.msc关闭www服务即可

## 三、Tomcat目录结构

### 1、目录结构图

![](http://opzv089nq.bkt.clouddn.com/17-8-26/82884026.jpg)



### 2、server.xml详解

#### 1、主要的层次结构

1. 结构图

   ![](http://opzv089nq.bkt.clouddn.com/17-8-26/61589781.jpg)

2. 说明

   1、bin

   > 该目录下存放的是二进制可执行文件会有startup.bat和shutdown.bat文件（其余文件无需理会），startup.bat用来启动Tomcat，但需要先配置JAVA_HOME环境变量才能启动，shutdawn.bat用来停止Tomcat；

2、conf

   > 这是一个非常非常重要的目录，这个目录下有四个最为重要的文件：

   - server.xml：

     > 配置整个服务器信息。例如修改端口号，添加虚拟主机等；以后会详细介绍这个文件；

- tomcat-users.xml：

  > 存储tomcat用户的文件，这里保存的是tomcat的用户名及密码，以及用户的角色信息。可以按着该文件中的注释信息添加tomcat用户，然后就可以在Tomcat主页中进入Tomcat Manager页面了；

- web.xml：

  > 部署描述符文件，这个文件中注册了很多MIME类型，即文档类型。这些MIME类型是客户端与服务器之间说明文档类型的，如用户请求一个html网页，那么服务器还会告诉客户端浏览器响应的文档是text/html类型的，这就是一个MIME类型。客户端浏览器通过这个MIME类型就知道如何处理它了。当然是在浏览器中显示这个html文件了。但如果服务器响应的是一个exe文件，那么浏览器就不可能显示它，而是应该弹出下载窗口才对。MIME就是用来说明文档的内容是什么类型的！  

- context.xml

  > 对所有应用的统一配置，通常我们不会去配置它。

   3、lib

> Tomcat的类库，里面是一大堆jar文件。如果需要添加Tomcat依赖的jar文件，可以把它放到这个目录中，当然也可以把应用依赖的jar文件放到这个目录中，这个目录中的jar所有项目都可以共享之，但这样你的应用放到其他Tomcat下时就不能再共享这个目录下的Jar包了，所以建议只把Tomcat需要的Jar包放到这个目录下，各个应用需要的jar各个应用自己管理。

   4、 logs

> 这个目录中都是日志文件，记录了Tomcat启动和关闭的信息，如果启动Tomcat时有错误，那么异常也会记录在日志文件中。

   5、temp

> 存放Tomcat的临时文件，这个目录下的东西可以在停止Tomcat后删除！

   6、webapps

> 存放web项目的目录，其中每个文件夹都是一个项目；如果这个目录下已经存在了目录，那么都是tomcat自带的。项目。其中ROOT是一个特殊的项目,在地址栏中没有给出项目目录时，对应的就是ROOT项目。http://localhost:8080/app，进入示例项目。其中app就是项目名，即文件夹的名字。

   7、work

> 运行时生成的文件，最终运行的文件都在这里。通过webapps中的项目生成的！可以把这个目录下的内容删除，再次运行时会生再次生成work目录。当客户端用户访问一个JSP文件时，Tomcat会通过JSP生成Java文件，然后再编译Java文件生成class文件，生成的java和class文件都会存放到这个目录下。

#### 2、Server配置

1. 概念

   > Server元素代表整个Catalina Servlet容器，它是Tomcat实例的顶层元素，由org.apache.catalina.Server接口来定义。<Server>元素可包含一个或多个<Service>元素，但<Server>元素不能作为任何其他元素的子元素。

2. 格式

   > <Server port=”8005” shutdown=”SHUTDOWN” debug=”0”>

3. 说明

   > className 指定实现org.apache.catalina.Server接口的类，默认值是 org.apache.catalina.StandardServer
   >
   > port 指定Tomcat服务器监听shutdown命令的端口。终止Tomcat服务器运行时，必须在Tomcat服务器所在的机器上发出shutdown命令。该属性是必须设定的。
   > shutdown 指定终止Tomcat服务器运行时，发给Tomcat服务器的shutdown监听端口的字符串，该属性是必须设定的。
   >
   > debug 日志记录的调试信息记录等级。 	

#### 3、Service配置

1. 概念

   > 对应Service组件，是包含在Server层中的一个逻辑功能层。它包含一个Engine层，以及一个或多个连接器（Connector）。Service组件将一个或多个Connector组件绑定到Engine层上，Connector组件侦听端口，获得用户请求，并将请求交给Engine层处理，同时把处理结果发给用户，从而实现一个特定的实际功能。Tomcat提供了Service接口的默认实现，所以通常也不需要定制,<Service>元素是有org.apache.catalina.Service接口定义，它包含一个<Engine>元素，以及一个或多个<Connector>元素，这些元素共享同一个<Engine>元素

2. 格式

   > <Service name="Catalina">
   > <Service name=”Apache”>
   > 第一个<Service>处理所有直接有Tomcat服务器接收到的Web客户请求，
   > 第二个<Service>处理由Apache服务器转发过来的Web客户请求

3. 说明

   > name 定义Service的名字

#### 4、Connector配置

1. 概念

   Connector元素代表与客户程序交互的组件，负责管理Tomcat的工作线程和读/写连接到不同用户的端口的请求/响应

2. 格式

   ```
    <!--8080默认的端口 -->
    <Connector port="8080" protocol="HTTP/1.1"
     connectionTimeout="20000"
     redirectPort="8443" />

    <!--80新增的80端口 -->
    <Connector port="80" protocol="HTTP/1.1"
     connectionTimeout="20000"
     redirectPort="8443" />
   ```

3. 说明

   > enableLookups 如果设置为true，表示支持域名解析，可以把IP地址解析为主机名。Web应用中调用request.getRemostHost方法则返回客户端主机名。该属性默认值为true。
   >
   > redirectPort 指定转发端口。如果当前端口只支持non-SSL请求，在需要安全通信的场合，将把客户请求转发给基于SSL的redirectPort端口。
   >
   > port 设定TCP/IP端口号，默认为8080。
   >
   > address 如果服务器有两个以上的IP地址，该属性可以设定端口监听的IP地址，默认情况下，端口会监听服务器上所有的IP地址。
   >
   > protocol 设定HTTP协议，默认值为HTTP/1.1。
   >
   > maxThreads 设定处理客户请求的线程的最大数目，这个值也决定了服务器可以同时相应客户请求的最大人数目，默认值为20。
   >
   > acceptCount 设定在监听端口队列中的最大客户请求数，默认值为10，如果队列已满，客户请求将被拒绝。
   >
   > connectionTimeout 定义建立客户连接超时的时间，以毫秒为单位。如果设置为-1，表示不限制建立客户连接的时间。

#### 5、Engine配置

1. 概念

   > 该层是请求分发处理层，可以连接多个Connector。它从Connector接收请求后，解析出可以完成用户请求的URL，根据该URL可以把请求匹配到正确的Host上，当Host处理完用户请求后，Engine层把结果返回给适合的连接器，再由连接器传输给用户。该层的接口一般不需要定制，特殊情况下，用户可以通过实现该接口来提供自定义的引擎,
   >
   > 每个<Service>元素只能包含一个<Engine>元素。<Engine>元素处理在同一个<Service>中所有的<Connector>元素接收到的客户请求

2. 格式

   > <Engine name=”Catalina” defaultHost=”localHost” debug=”0”>


1. 说明

   > name 定义Engine的名字
   > defaultHost 指定处理客户请求的默认主机名，在<Engine>的<Host>子元素定义这一主机。

#### 6、Host配置

1. 概念

   > 对应Host组件，该层表示一个虚拟主机，一个Engine层可以包含多个Host层，每个Host层可以包含一个或多个Context层，对应不同的Web应用。因为Tomcat给出的Host接口的实现（类StandardHost）提供了重要的附加功能，所以用户通常不需要定制Host。
   >
   >  <Host>元素由org.apache.catalina.Host接口定义。一个<Engine>元素可以包含多个<Host>元素，每个<Host>元素定义了一个虚拟主机，它可以包含一个或多个Web应用

2. 格式

   > <Host name=”localhost” debug=”0” appBase=”webapps” unpackWARs=”true” autoDeploy=”true”></Host>

3. 说明

   > name 定义虚拟主机的名字
   >
   > appBase  指定虚拟主机的目录，可以指定绝对目录，也可以指定相对于<CATALINA_HOME>的相对目录，如果此项没有设定，默认值为<CATALINA_HOME>/webapps
   >
   > unpackWARs 如果此项设为true，表示将吧Web应用的WAR文件先展开为开放目录结构后再运行。如果设为false，将直接运行WAR文件。
   >
   > autoDeploy 如果此项设为true，表示当Tomcat服务器处于运行状态时，能够监测appBase下的文件，如果有新的Web加入进来的话，会自动发布这个Web应用。
   >
   > alias 指定虚拟主机的别名，可以指定多个别名。
   >
   > deployOnStartUp 如果此项设置为true，表示Tomcat服务器启动时会自动发布webBase目录下的所有Web应用，如果Web应用在server.xml中没有相应的<Context>元素，将采用Tomcat默认的Context，
   >
   > deployOnStartUp的默认值为true。

#### 7、Context

1. 概念

   > 每一个Context都描述了一个Tomcat的Web应用程序的目录

2. 格式

   > <Context path="/aa" docBase="C:\AA"  />

3. 说明

   > path。这是Context在Web服务时的虚拟目录位置和目录名。
   >
   > docBase。这是Context的目录。可以是绝对目录也可以是基于ContextManage的根目录的相对目录。
   >
   > debug。日志记录的调试信息记录等级。 
   >
   > reloadable。这是为了方便Servlet的开发人员而设置的，当这个属性开关打开的时候，Tomcat将检查Servlet是否被更新而决定是否自动重新载入它

## 四、部署应用程序

就是将 Web 应用（第三方的 WAR 文件，或是你自己定制的 Web 应用）安装到 Tomcat 服务器上的整个过程。

在 Tomcat 服务器上，可以通过多种方法部署 Web 应用

1. 静态部署。在启动 Tomcat 之前安装 Web 应用。
2. 动态部署。使用 Tomcat 的 Manager 应用直接操控已经部署好的 Web 应用（依赖 auto-deployment 特性）







