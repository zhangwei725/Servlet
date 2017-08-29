# WebServlet 3.0

### 一、@WebServlet

@WebServlet 用于将一个类声明为 Servlet，该注解将会在部署时被容器处理，容器将根据具体的属性配置将相应的类部署为 Servlet。该注解具有下表给出的一些常用属性（以下所有属性均为可选属性，但是 vlaue 或者 urlPatterns 通常是必需的，且二者不能共存，如果同时指定，通常是忽略 value 的取值）：

##### 表 1. @WebServlet 主要属性列表

| **属性名**        | **类型**         | **描述**                                   |
| -------------- | -------------- | ---------------------------------------- |
| name           | String         | 指定 Servlet 的 name 属性，等价于 <servlet-name>。如果没有显式指定，则该 Servlet 的取值即为类的全限定名。 |
| value          | String[]       | 该属性等价于 urlPatterns 属性。两个属性不能同时使用。        |
| urlPatterns    | String[]       | 指定一组 Servlet 的 URL 匹配模式。等价于 <url-pattern> 标签。 |
| loadOnStartup  | int            | 指定 Servlet 的加载顺序，等价于 <load-on-startup> 标签。 |
| initParams     | WebInitParam[] | 指定一组 Servlet 初始化参数，等价于 <init-param> 标签。  |
| asyncSupported | boolean        | 声明 Servlet 是否支持异步操作模式，等价于 <async-supported> 标签。 |
| description    | String         | 该 Servlet 的描述信息，等价于 <description> 标签。    |
| displayName    | String         | 该 Servlet 的显示名，通常配合工具使用，等价于 <display-name> 标签。 |

下面是一个简单的示例：

如此配置之后，就可以不必在 web.xml 中配置相应的 <servlet> 和 <servlet-mapping> 元素了，容器会在部署时根据指定的属性将该类发布为 Servlet。它的等价的 web.xml 配置形式如下：

### 二、WebInitParam

该注解通常不单独使用，而是配合 @WebServlet 或者 @WebFilter 使用。它的作用是为 Servlet 或者过滤器指定初始化参数，这等价于 web.xml 中 <servlet> 和 <filter> 的 <init-param> 子标签。@WebInitParam 具有下表给出的一些常用属性：

##### 表 2. @WebInitParam 的常用属性

| **属性名**     | **类型** | **是否可选** | **描述**                     |
| ----------- | ------ | -------- | -------------------------- |
| name        | String | 否        | 指定参数的名字，等价于 <param-name>。  |
| value       | String | 否        | 指定参数的值，等价于 <param-value>。  |
| description | String | 是        | 关于参数的描述，等价于 <description>。 |

### 三、@WebFilter

@WebFilter 用于将一个类声明为过滤器，该注解将会在部署时被容器处理，容器将根据具体的属性配置将相应的类部署为过滤器。该注解具有下表给出的一些常用属性 ( 以下所有属性均为可选属性，但是 value、urlPatterns、servletNames 三者必需至少包含一个，且 value 和 urlPatterns 不能共存，如果同时指定，通常忽略 value 的取值 )：

##### 表 3. @WebFilter 的常用属性

| **属性名**         | **类型**         | **描述**                                   |
| --------------- | -------------- | ---------------------------------------- |
| filterName      | String         | 指定过滤器的 name 属性，等价于 <filter-name>         |
| value           | String[]       | 该属性等价于 urlPatterns 属性。但是两者不应该同时使用。       |
| urlPatterns     | String[]       | 指定一组过滤器的 URL 匹配模式。等价于 <url-pattern> 标签。  |
| servletNames    | String[]       | 指定过滤器将应用于哪些 Servlet。取值是 @WebServlet 中的 name 属性的取值，或者是 web.xml 中 <servlet-name> 的取值。 |
| dispatcherTypes | DispatcherType | 指定过滤器的转发模式。具体取值包括：ASYNC、ERROR、FORWARD、INCLUDE、REQUEST。 |
| initParams      | WebInitParam[] | 指定一组过滤器初始化参数，等价于 <init-param> 标签。        |
| asyncSupported  | boolean        | 声明过滤器是否支持异步操作模式，等价于 <async-supported> 标签。 |
| description     | String         | 该过滤器的描述信息，等价于 <description> 标签。          |
| displayName     | String         | 该过滤器的显示名，通常配合工具使用，等价于 <display-name> 标签。 |

下面是一个简单的示例：

如此配置之后，就可以不必在 web.xml 中配置相应的 <filter> 和 <filter-mapping> 元素了，容器会在部署时根据指定的属性将该类发布为过滤器。它等价的 web.xml 中的配置形式为：

### 四、@WebListener

该注解用于将类声明为监听器，被 @WebListener 标注的类必须实现以下至少一个接口：

1. ServletContextListener
2. ServletContextAttributeListener
3. ServletRequestListener
4. ServletRequestAttributeListener
5. HttpSessionListener
6. HttpSessionAttributeListener

该注解使用非常简单，其属性如下：

##### 表 4. @WebListener 的常用属性

| **属性名** | **类型** | **是否可选** | **描述**     |
| ------- | ------ | -------- | ---------- |
| value   | String | 是        | 该监听器的描述信息。 |

一个简单示例如下：

如此，则不需要在 web.xml 中配置 <listener> 标签了。它等价的 web.xml 中的配置形式如下：

### 五、@MultipartConfig

该注解主要是为了辅助 Servlet 3.0 中 HttpServletRequest 提供的对上传文件的支持。该注解标注在 Servlet 上面，以表示该 Servlet 希望处理的请求的 MIME 类型是 multipart/form-data。另外，它还提供了若干属性用于简化对上传文件的处理。具体如下：

##### @MultipartConfig 的常用属性

| 属性名               | 类型     | 是否可选 | 描述                                       |
| ----------------- | ------ | ---- | ---------------------------------------- |
| fileSizeThreshold | int    | 是    | 当数据量大于该值时，内容将被写入文件。                      |
| location          | String | 是    | 存放生成的文件地址。                               |
| maxFileSize       | long   | 是    | 允许上传的文件最大值。默认值为 -1，表示没有限制。               |
| maxRequestSize    | long   | 是    | 针对该 multipart/form-data 请求的最大数量，默认值为 -1，表示没有限制。 |