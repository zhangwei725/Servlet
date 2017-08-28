# WEB

## 一、Web简介

### 1、 简介

1. 早期的web应用主要是静态页面的浏览，这些静态页面使用HTML语言编写，放在服务器上，用户使用浏览器通过HTTP协议请求服务器上的web页面，服务器上的web服务器软件接受到用户发送的请求后，读取请求URI所标识的资源，加上消息包头发送给客户端的浏览器，浏览器解析响应中的HTML数据，向用户呈现多姿多彩的HTML页面。
2. 但是随着网络的发展，很多线下业务开始向网上发展，基于Internet的web应用也变得越来越复杂用户所访问的资源已不仅仅局限于服务器硬盘上存放的静态网页，更多的应用需要根据用户的请求动态生成网页信息，复杂的还需要从数据库中提取信息，经过一定的运算，生成一个页面返回给客户如何才能实现？
3. 利用已经实现HTTP协议的服务器端软件，这些软件预先给我们留出了扩展的接口，我们只需要按照一定的规则取提供相应的扩展功能，当这类web服务器接受到客户端请求后，判断请求是否是访问我们提供的扩展功能，如果是，将请求交由我们编写的程序去处理，处理完成后，程序将处理结果交回web服务器软件，web服务器软件拿到结果后，再将结果作为相应信息返回给客户端。
4. 早起使用的web服务器扩展机制是CGI，它允许用户调用web服务器上的CGI程序，CGI全称 CommonGateway Interface 公共网关接口，大多数的CGI程序使用Perl来编写，也有通过C,Python或PHP编写，用户通过单击某个连接或者直接在浏览器的地址栏输入URL来访问CGI程序，web服务器接受到请求后，发现这个请求是给CGI程序的，于是就启动并运行这个CGI程序，对用于请求进行处理。CGI程序解析请求中的CGI数据，处理数据，并且产生一个响应，这个响应被返回给web服务器，web服务器包装这个响应，以HTTP响应的形式发送给Web浏览器。但是CGI编写困难，对用户请求的响应时间较长以进程方式运行导致性能受限制。
5. 1997年，SUN公司推出了Servlet技术，作为java阵营的CGI解决方案，作为对微软ASP（1996年推出）的推出，SUN公司于1998年推出了JSP技术，允许在HTML页面中嵌入java脚本代码，从而实现动态网页的功能。此外，PHP（1994年发明）技术也是类似于APS,JSP服务器端页面编写技术

### 2、Web应用历史

#### 2.1、单机程序

> ​    软件从附着于电脑硬件之日起，就在不断的进行着自我完善和演变。从其使用模式的角度出发，可以简单分为单机程序和网络程序。发展到今时今日仍有大量的不依赖网络的单机程序被我们使用，如记事本、Excel、PPT、ZIP压缩等软件都是大家熟知的装机必备软件

#### 2.2、网络程序

> ​    当电脑越来越多的参与到日常生产生活中，单机程序已经不能满足企业的需要。企业级应用要求能够最大程度的让更多的客户端参与到协同办公之中，所以依赖于网络的程序开始大力发展起来。

#### 2.3、主机+终端模式

> ​    最早的网络程序是基于主机+终端模式的，也就是整个应用中只有一台大型主机，各个操作地点都是使用一条专线与主机相连，终端不提供任何运算和界面，类似于Unix形式，所有的运算和处理都由主机来完成。主机一般处理能力非常强大，并且稳定，主要机型都是由IBM这样的大公司提供。
>
> ![](http://opof44evx.bkt.clouddn.com/17-5-9/83457195-file_1494318720996_f497.jpg)
>
> ​                            **图-1 主机-终端模式**
>
> ​    但上述模式中，主机的高昂的价格以及扩展难、维护费用高等弊端并不是一般企业所能承受，所以除银行、航空订票、证券等大企业在使用以外，大多数企业开始转投CS架构的程序，即客户端服务器架构。

#### 2.4、C/S架构

##### 1、说明

> ​    CS架构的发展过程经历了两层CS架构，三层CS架构以及多层CS架构的演变。  
> ​    两层的CS架构是由客户端和后面的数据库组成的。数据库用于存放数据，并且使用数据库编程语言编写业务逻辑，客户端则使用VB、VC、Delphi这样的可视化编程方便的语言来开发客户端的输入输出界面。用户通过界面向服务器发送请求，服务器发回的数据则通过界面进行显示，服务器的角色就由数据库来充当。这样做的好处就是开发效率高，满足企业需求。但是这种架构存在着很大的弊端，第一是可移植性差，如当数据库从SQL Server更换为Oracle时就必须将业务逻辑用新的语言再重新编写一遍；第二则是大型系统做不了，因为客户端与数据库需要建立持续的连接，而数据库能够支持的最大连接数是有限制的。所以在2000年这样的架构流行之后，慢慢的就开始向三层CS架构转变。  
> ​    三层的CS架构指的是客户端+应用服务器+数据库，即将混合在数据库端的业务逻辑从中分离出来，放入到应用服务器中，数据库只负责数据的管理、存储及检索。客户端负责界面。三层之中的应用服务器其实也是程序，类似于前面讲过的TCP、Socket编程，任何支持TCP编程的语言都可以作为应用服务器。三层CS架构的工作流程。  
> ![](http://opof44evx.bkt.clouddn.com/17-5-9/2217302-file_1494318727491_25d7.jpg)
>
> ​                            **C / S 三层架构图**
>
> ​    用户通过GUI（图形用户界面）进行操作，然后调用客户端的通信模块，通信模块依据自定义协议将请求数据打包，通过网络发送该请求，到达应用服务器时，应用服务器同样也有一个通信模块，将收到的数据包按照协议进行拆包，调用相应的业务处理模块，处理数据，其中可能需要访问数据库来完成数据的获取，将处理完的结果再次发送给通信模块，通信模块将结果按照自定义协议进行打包，然后将数据包发送给客户端的通信模块，客户端进行拆包获取响应数据，将结果显示在界面上，更新界面上的数据显示。
>
> ​    这样的程序结构虽然在一定程度上降低了对数据库编程的依赖，并且能够适应大型的应用程序，但数据通信模块的增加却提升了开发的难度以及整体架构的复杂度。

##### 2、优缺

**优点**

1. 能充分发挥客户端PC的处理能力，很多工作可以在客户端处理后再提交给服务器，所以CS客户端响应速度快。
2. 操作界面漂亮、形式多样，可以充分满足客户自身的个性化要求。  
3. C/S结构的管理信息系统具有较强的事务处理能力，能实现复杂的业务流程。
4. 安全性能可以很容易保证，C/S一般面向相对固定的用户群，程序更加注重流程，它可以对权限进行多层次校验，提供了更安全的存取模式，对信息安全的控制能力很强。一般高度机密的信息系统采用C/S结构适宜。

**缺点**

1. 需要专门的客户端安装程序，分布功能弱，针对点多面广且不具备网络条件的用户群体，不能够实现快速部署安装和配置。
2. 兼容性差，对于不同的开发工具，具有较大的局限性。若采用不同工具，需要重新改写程序。开发、维护成本较高，需要具有一定专业水准的技术人员才能完成，发生一次升级，则所有客户端的程序都需要改变。。
   1. 用户群固定。由于程序需要安装才可使用，因此不适合面向一些不可知的用户，所以适用面窄，通常用于局域网中

#### 2.5、B/S架构

##### 1、说明

> ​    为了降低三层CS架构中与通信有关的复杂度，BS架构开始成为了网络程序中一大重要的架构类型。  
> BS架构即Browser + Web Server + DB  
> ![](http://opof44evx.bkt.clouddn.com/17-5-9/5701839-file_1494318731368_169c6.jpg)
>
> ​                                 **图-3 B/S三层架构图**
>
> ​    由于三层CS架构中，自定义协议提升了整体的复杂度，那么就将自定义协议变成标准的HTTP协议。于是客户端使用HTTP协议进行数据打包拆包的程序即各厂商依据标准开发的浏览器，Web服务器也是基于HTTP协议由一些厂商提供，如IIS，Apache等。这样基于浏览器和服务器的架构中，由于协议已被限定，所以与通信有关的数据打包拆包的过程都不用我们开发人员来编写程序，只需要考虑将HTTP协议解析出来的数据进行业务处理，以及将什么样的结果提供给响应即可。也就是开发过程中只需要考虑7,8,9这三个步骤即可。于是大大降低了网络程序的开发难度，所以这种架构得到了大量的应用。

##### 2、优缺点

**优点**

1. 分布性强，客户端零维护。只要有网络、浏览器，可以随时随地进行查询、浏览等业务处理。 
2. 业务扩展简单方便，通过增加网页即可增加服务器功能。  
3. 维护简单方便，只需要改变网页，即可实现所有用户的同步更新。 
4. 开发简单，共享性强。

**缺点**

1. 个性化特点明显降低，无法实现具有个性化的功能要求。 
2. 在跨浏览器上，BS架构不尽如人意。
3. 客户端服务器端的交互是请求-响应模式，通常动态刷新页面，响应速度明显降低（Ajax可以一定程度上解决这个问题）。无法实现分页显示，给数据库访问造成较大的压力。 
4. 在速度和安全性上需要花费巨大的设计成本。
5. 功能弱化，难以实现传统模式下的特殊功能要求。

## 二、什么WEB应用程序

1. WEB应用程序指供浏览器访问的程序，通常也简称为web应用。例如有x.html 、x.html…..多个web资源，这多个web资源用于对外提供服务，此时应把这多个web资源放在一个目录中，以组成一个web应用程序
2. 一个web应用由多个静态web资源和动态web资源组成，如:html、css、js文件，Jsp文件、java程序、支持jar包、配置文件等等。
3. Web应用开发好后，若想供外界访问，需要把web应用所在目录交给web服务器管理

## 三、WEB应用程序的开发

### 1、概要

​ Web应用程序指供浏览器访问的程序，通常也简称为web应用

### 2、静态web

1. 定义

   指web页面中供人们浏览的数据始终是不变,例如 .htm、.html，这些是网页的后缀，用户直接访问这些文件就能看到内容

2. 流程示例图

   ![](http://opzv089nq.bkt.clouddn.com/17-8-26/76985718.jpg)

3. 缺点

   1、Web页面中的内容无法动态更新，所有的用户每时每刻看见的内容和最终效果都是一样的。

   2、静态WEB无法连接数据库，无法实现和用户的交互

4. 静态WEB想达到动态效果需要用到的技术

   > JavaScript\(常用\)
   >
   > JScript
   >
   > ScriptEase
   >
   > VBScript

### 3、动态web

1. 定义

   指web页面中浏览的数据是由程序产生的，不同时间点,不同地点,不同人访问同一个web页面看到的内容和界面可能不一样,而且动态WEB具有交互性，WEB的页面的内容可以动态更新

2. 流程示意图\(java为例\)

   ![](http://opzv089nq.bkt.clouddn.com/17-8-26/35084213.jpg)

   > 动态WEB中，程序依然使用客户端和服务端，客户端依然使用浏览器（IE、FireFox等），通过网络\(Network\)连接到服务器上，使用HTTP协议发起请求（Request），现在的所有请求都先经过一个WEB Server Plugin（服务器插件）来处理，此插件用于区分是请求的是静态资源\(_.htm或者是_.htm\)还是动态资源。
   >
   > 如果WEB Server Plugin发现客户端请求的是静态资源\(_.htm或者是_.html\)，则将请求直接转交给WEB服务器，之后WEB服务器从文件系统中取出内容，发送回客户端浏览器进行解析执行。
   >
   > 如果WEB Server Plugin发现客户端请求的是动态资源（_.jsp、_.asp/_.aspx、_.php），则先将请求转交给WEB Container\(WEB容器\)，在WEB Container中连接数据库，从数据库中取出数据等一系列操作后动态拼凑页面的展示内容，拼凑页面的展示内容后，把所有的展示内容交给WEB服务器，之后通过WEB服务器将内容发送回客户端浏览器进行解析执行

3. 动态web技术

   * ASP
   * PHP
   * JSP

## 三、什么WEB 服务器

### 1、概念

> 1. 一台负责提供网页的电脑，主要是各种编程语言构建而成的，通过HTTP协议传给客户端（一般是指网页浏览器）。
> 2. 一个提供网页的服务器程序
> 3. 服务器是一种被动程序：只有当Internet上运行在其他计算机中的浏览器发出请求时，服务器才会响应

### 2、常见的WEB服务器

* Tomcat服务器

  > Tomcat是一个实现了JAVA EE标准的最小的WEB服务器，是Apache 软件基金会的Jakarta 项目中的一个核心项目，由Apache、Sun 和其他一些公司及个人共同开发而成。因为Tomcat 技术先进、性能稳定，而且开源免费，因而深受Java 爱好者的喜爱并得到了部分软件开发商的认可，成为目前比较流行的Web 应用服务器。学习JavaWeb开发一般都使用Tomcat服务器，该服务器支持全部JSP以及Servlet规范

* Apache服务器

  > Apache HTTP Server（简称Apache）是Apache软件基金会的一个开放源代码的网页服务器软件，可以在大多数电脑操作系统中运行，由于其跨平台和安全性被广泛使用，是最流行的Web服务器软件之一。它快速可靠，并且可以通过简单API扩充，将Python/Perl等解析器编译到服务器中

* IBM WebSphere服务器

  > WebSphere Application Server 是一种功能完善、开放的Web应用程序服务器，是IBM公司电子商务计划的核心部分，它是基于 Java 的应用环境，用于建立、部署和管理 Internet 和 Intranet Web 应用程序。这一整套产品进行了扩展，以适应 Web应用程序服务器的需要，范围从简单到高级直到企业级

* WebLogic服务器

  > 是美商Oracle的主要产品之一，系购并得来。是商业市场上主要的Java（J2EE）应用服务器软件之一，是世界上第一个成功商业化的J2EE应用服务器，目前已推出到12c（12.1.1）版。而此产品也延伸出WebLogic Portal, WebLogic Integration等企业用的中间件（但目前Oracle主要以Fusion Middleware融合中间件来取代这些WebLogic Server之外的企业包），以及OEPE（Oracle Enterprise Pack for Eclipse）开发工具。  
  > WebLogic最早由WebLogic Inc.开发，后并入BEA公司，最终BEA公司又并入Oracle公司

* Nginx服务器

  > Nginx（发音同engine x）是一个 Web服务器，也可以用作反向代理，负载平衡器和 HTTP缓存。该软件由 Igor Sysoev 创建，并于2004年首次公开发布。同名公司成立于2011年，以提供支持。  
  > Nginx 是免费的开源软件，根据类似 BSD许可证的条款发布。大部分 Web服务器通常使用 NGINX 作为负载均衡器。

* IIS服务器

  > Microsoft的Web服务器产品为Internet Information Services （IIS），IIS 是允许在公共Intranet或Internet上发布信息的Web服务器。ⅡS是目前最流行的Web服务器产品之一，很多著名的网站都是建立在ⅡS的平 台上。IIS提供了一个图形界面的管理工具，称为Internet信息服务管理器，可用于监视配置和控制Internet服务

* Lighttpd服务器

  > Lighttpd是由一个德国人写的开源软件，其目标是提供一个专门针对高性能网站，安全、快  
  > 速、兼容性好并且灵活的Web Server环境。它具有内存开销低、CPU占用率低、效能好，以及  
  > 模块丰富等特点。支持FastCGI、CGI. Auth、输出压缩\(output compress \)、URL重写及Alias  
  > 等重要功能。Lighttpd跟Nginx一样，也是一款轻量级Web服务器，是Nginx的竞争对手之一



