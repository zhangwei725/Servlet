# 二、MVC发展史历程

## 1. JSP + Java Bean\(Model1\)

1. 模型的结构

   !\[\]\(http://opzv089nq.bkt.clouddn.com/17-7-15/83213509.jpg\)![](http://opzv089nq.bkt.clouddn.com/17-7-15/83213509.jpg)

2. 产生的问题

   如果有好几千个jsp， 这些jsp互相调用\(通过GET/POST\), 到了最后调用关系无人能搞懂。

## 2.JSP和JavaBean+Servlet\(Model2\)

1. 模型的结构

   !\[\]\(http://opzv089nq.bkt.clouddn.com/17-7-15/6405306.jpg\)![](http://opzv089nq.bkt.clouddn.com/17-7-15/6405306.jpg)

2. 说明

   1. JavaBean作为Model层，定义bean来表示数据和封装业务逻辑

      1. 定义数据Bean来表示需要显示给用户的结果

      2. 定义业务Bean来封装业务逻辑,DAO

   2. 使用Servlet处理用户请求

      1. 对输入数据的检查和转换

      2. 通过JavaBean访问数据库

      3. 初始化JSP页面中要用到的JavaBean或对象,保存在作用域中

      4. 根据处理中不同的分支和结果，决定转向那个JSP等

   3. JSP作为View层，负责生成交互后返回的界面

      1. 它主要通过信息共享，获取Servlet生成的对象或JavaBean，从中取出数据，插入到HTML页面中

## 2. EJB\(Enterprise Java bean\)

### 2.1. EJB到底是什么？

1. 官方解释:商务软件的核心部分是它的业务逻辑

2. EJB是sun的JavaEE服务器端组件模型，设计目标与核心应用是部署分布式应用程序。简单来说就是把已经编写好的程序（即：类）打包放在服务器上执行。凭借java跨平台的优势，用EJB技术部署的分布式系统可以不限于特定的平台。EJB \(Enterprise JavaBean\)是J2EE\(javaEE\)的一部分，定义了一个用于开发基于组件的企业多重应用程序的标准。其特点包括网络服务支持和核心开发工具\(SDK\)。 在J2EE里，Enterprise Java Beans\(EJB\)称为Java 企业Bean，是Java的核心代码，分别是会话Bean（Session Bean），实体Bean（Entity Bean）和消息驱动Bean（MessageDriven Bean）。\(百科\)

3. EJB要解决的问题

   1. 数据存储\(Persistent\)

   2. 分布式 \(Distributed\)

   3. 事务处理\(Transactional\)

   4. 安全性 \(Secure\)

   5. 高可用性

   6. 可扩展 \(Scalable\)

4. 相关技术

   1. JDBC\( Java database connectivity\)

      ```

      ```

   2. JNDI

      ```

      ```

   3. RMI\(**Remote Method Invoke 远程方法调用**\)

      ```









      ```

   4. JTA\(Java Transaction API\)

      ```

      ```

   5. JMS\(Java Message Service\)

      ```

      ```

   6. Java mail

      ```

      ```

5. 什么是"服务集群"？什么是"企业级开发"？ 什么分布式?

   1. 分布式：一个业务分拆多个子业务，部署在不同的服务器上

      ```

      ```

   2. 集群：同一个业务，部署在多个服务器上

      ```

      ```

### 2.2.EJB的缺点

1. 巨大而复杂的规格说明：对于一个复杂的分布式系统，有一个文档进行说明是一件很合理的事情。但是，并不是所有的文档信息都是真正必需的，EJB的文档反而成了一个非常不便利的工具。

2. 庞大的文件：在你开发一个项目之前，你通常需要阅读1000多页的文档。这是部署EJB时非常痛苦的事情。

3. 增加程序调试时间：制定EJB解决方案比远比使用普通的Java代码所需的时间长，调试EJB程序也比调试普通的Java代码所需要的时间也长。主要原因是我们不知道bug是出在代码本身还是出在容器上。

4. EJB代码更加复杂：举个例子，为了实现一个session bean，你必须写三个类，为了实现一个实体bean，你需写四个类。增加几个部署描述语句，比如说最简单的"Hello world"程序需要10个文件，而不是一个文件。

5. 重复设计的危险：导致这种结果的原因是复杂的文档所致。。如果你没有完全理解EJB的概念，你就不会很好的驾奴它，让它你所用，反之为你所累。

6. 维护困难：EJB是一个不断更新的技术，在新技术不断推出的时候，代码需要升级，这就就需要额外的努力和成本来使你的程序和新的EJB容器兼容。

### 2.3.总结

​ EJB实现原理： 就是把原来放到客户端实现的代码放到服务器端，并依靠RMI进行通信。

​ RMI实现原理 ：就是通过Java对象可序列化机制实现分布计算。

​ 服务器集群： 就是通过RMI的通信，连接不同功能模块的服务器，以实现一个完整的功能。













  


