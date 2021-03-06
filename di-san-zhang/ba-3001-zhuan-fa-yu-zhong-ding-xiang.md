# 重定向转发机制

## 一、重定向

### 1、什么是重定向

> 服务器向浏览器发送一个302状态码以及一个Location消息头,浏览器收到返回消息会立即向重定向地址发出请求

### 2、为什么要使用重定向技术

URL重定向技术,我们在网站建设中，时常会遇到需要网页重定向的情况：

1. 网站调整（如改变网页目录结构）；
2. 网页被移到一个新地址；
3. 网页扩展名改变

这种情况下，如果不做重定向，则用户收藏夹或搜索引擎数据库中旧地址只能让访问客户得到一个404页    面错误信息，访问流量白白丧失；再者某些注册了多个域名的网站，也需要通过重定向让访问这些域名的用户自动跳转到主站点等

### 1.2 重定向原理

> ![](http://opzv089nq.bkt.clouddn.com/17-8-27/67295304.jpg)

### 3、如何实现重定向

> response.sendRedirect\(String url\);

### 4、特点

1. 重定向地址可以是任意地址
2. 重定向后浏览器的地址会发生变化
3. web组件不会共享同一个request和response
4. 至少两次请求

## 二、转发

### 1、什么是转发

> 一个web组件将未完成的处理通过容器转交给另外一个web组件继续完成；
>
> 例如：一个组件获取数据后，将数据转发给另外一个jsp来展示这些数据

### 2、原理

> ![](http://opzv089nq.bkt.clouddn.com/17-8-27/32914761.jpg)

### 3、如何实现转发

1. 绑定数据到request对象

   ```
   request.setAttribute(String name,Object obj);
   ```

2. 获得转发器

   ```
   RequestDispatcher rd = request.getRequestDispatcher(String path);
   ```

3. 转发

   ```
   rd.forward(request,response);
   ```

### 4、特点

1. 浏览器地址栏不会发生变化，因为转发过程在服务器内部进行
2. 转发的目标必须是同一个应用内部的某个地址
3. 所涉及的web组件共享同一个request对象和response对象
4. 转发后的语句也会执行

### 5、转发和重定向区别

| 转发 | 重定向 |
| --- | --- |
| 服务器转向另一个新地址 | 浏览器获取响应后再发一次新的请求 |
| 一个请求对象，且共享数据 | 两次请求，不共享数据 |
| 地址栏不变 | 地址栏改变为新的地址 |
| 新地址必须是应用内部某个地址 | 可以是任意地址 |



