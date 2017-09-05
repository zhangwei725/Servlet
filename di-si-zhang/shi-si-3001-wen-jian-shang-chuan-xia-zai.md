# 文件上传与下载

## 一、文件上传

### 1、注意事项

#### 1.1、表单要求

1. 必须使用表单，而不能是超链接
2. 表单的method必须是POST，而不能是GET
3. 表单的enctype必须是multipart/form-data

#### 1.2、Servlet的要求：

1. 不能再使用request.getParameter()来获取表单数据
2. 可以使用request.getInputStream()得到所有的表单数据，而不是一个表单项的数据
3. 这说明不使用fileupload，我们需要自己来对request.getInputStream()的内容进行解析

### 2、使用Servlet的API

#### 2.1、servlet3.0上传文件注意事项：

1. html中 input type="file" 表示文件上传控件；

2. form的 enctype="multipart/form-data"；

3. 在Servlet类前加上 @MultipartConfig

4. request.getPart()获得

   > request.getPart("字段名字");//得到Part实例

5. part对象

   > String getContentType();//获取上传文件的MIME类型
   >
   > //获取表单项的名称，不是文件名称
   >
   > String getName();
   >
   > //获取指定的头的值
   >
   > String getHeader(String header);
   > String getSize();//获取上传文件的大小
   > InputStream getInputStream();//获取文件流内容
   > void write(String filePath);//把上传文件保存到指定的位置

#### 2.2、示例代码

1. HTML代码

   ```
      <html>
      <head>
      <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
      <title>Insert title here</title>
      </head>
      <body>
          <form method="post" enctype="multipart/form-data" action="upload">
              <input type="file" id="file" name="file" /> 
              <input type="text" id="name" name="name" /> 
              <input type="submit" value="提交" />
          </form>
      </body>
      </html>
   ```

2. Java代码

   ```
      @WebServlet(name = "UploadServlet", urlPatterns = {"/upload"})
      @MultipartConfig
      public class UploadServlet extends HttpServlet {
          public void init(ServletConfig config) throws ServletException {
              super.init(config);
          }
       
          public void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        	
          }
      }
   ```

   ```

   ```

### 3、第三方框架 commons-fileupload-1.3.3.jar

#### 3.1、核心类api介绍

##### 1.1、FileItem 表示文件上传表单中 每个数据部分

1. isFormField

   > public boolean isFormField()
   >
   > isFormField方法用于判断FileItem类对象封装的数据是否属于一个普通表单字段，还是属于一个文件表单字段，如果是普通表单字段则返回true，否则返回false。

2. getName方法

   > public String getName()
   >
   > getName方法用于获得文件上传字段中的文件名，对于描述头，getName方法返回的结果为字符串 
   >
   > “C:/bg.gif”。如果FileItem类对象对应的是普通表单字段，getName方法将返回null。即使用户没有通过网页
   >
   > 表单中的文件字段传递任何文件，但只要设置了文件表单字段的name属性，浏览器也会将文件字段的信息传
   >
   > 递给服务器，只是文件名和文件内容部分都为空，但这个表单字段仍然对应一个FileItem对象，此时，
   >
   > getName方法返回结果为空字符串""，读者在调用Apache文件上传组件时要注意考虑这个情况。
   >
   > 注意：如果用户使用Windows系统上传文件，浏览器将传递该文件的完整路径，如果用户使用Linux或者
   >
   > Unix系统上传文件，浏览器将只传递该文件的名称部分。

3. getFieldName方法

   > public String getFieldName()
   >
   > getFieldName方法用于返回表单字段元素的name属性值，也就是返回name属性值，例如“name=img”中的“img”。

4. write方法

   > public void write(File file)
   >
   > write 方法用于将FileItem对象中保存的主体内容保存到某个指定的文件中。如果FileItem对象中的主体内容是保存在某个临时文件中，该方法顺利完成后，临时文件有可能会被清除。该方法也可将普通表单字段内容写入到一个文件中，但它主要用途是将上传的文件内容保存在本地文件系统中。

5. getString方法

   > public java.lang.String getString()
   >
   > getString方法用于将FileItem对象中保存的主体内容作为一个字符串返回，它有两个重载的定义形式：前者使用缺省的字符集编码将主体内容转换成字符串，后者使用参数指定的字符集编码将主体内容转换成字符串。如果在读取普通表单字段元素的内容时出现了中文乱码现象，请调用第二个getString方法，并为之传递正确的字符集编码名称。


1. getContentType方法

   > getContentType 方法用于获得上传文件的类型，对于描述头，getContentType方法返回的结果为字符串“image/gif”，即 “Content-Type”字段的值部分。如果FileItem类对象对应的是普通表单字段，该方法将返回null。

2. isInMemory方法

   > public boolean isInMemory()
   > isInMemory方法用来判断FileItem类对象封装的主体内容是存储在内存中，还是存储在临时文件中，如果存储在内存中则返回true，否则返回false。	

3. delete方法

   > delete方法用来清空FileItem类对象中存放的主体内容，如果主体内容被保存在临时文件中，delete方法将删除该临时文件。尽管 Apache组件使用了多种方式来尽量及时清理临时文件，但系统出现异常时，仍有可能造成有的临时文件被永久保存在了硬盘中。在有些情况下，可以调用这个方法来及时删除临时文件。

##### 1.2、ServletFileUpload 文件上传核心类

> 是Apache文件上传组件处理文件上传的核心高级类（所谓高级就是不需要管底层实现，暴露给用户的简单易用的接口）。使用其 parseRequest(HttpServletRequest) 方法可以将通过表单中每一个HTML标签提交的数据封装成一个FileItem对象，然后以List列表的形式返回。使用该方法处理上传文件简单易用。
> 如果你希望进一步提高性能，你可以采用 getItemIterator 方法，直接获得每一个文件项的数据输入流，对数据做直接处理。
> 在使用ServletFileUpload对象解析请求时需要根据DiskFileItemFactory对象的属性 sizeThreshold（临界值）和repository（临时目录） 来决定将解析得到的数据保存在内存还是临时文件中，如果是临时文件，保存在哪个临时目录中？。所以，我们需要在进行解析工作前构造好DiskFileItemFactory对象，

1. 构造方法

   > 1. public ServletFileUpload()：
   >
   >    构造一个未初始化的实例，需要在解析请求之前先调用setFileItemFactory()方法设置 fileItemFactory属性。
   >
   > 2. public ServletFileUpload(FileItemFactory fileItemFactory)
   >
   >    构造一个实例，并根据参数指定的FileItemFactory 对象，设置 fileItemFactory属性

2. public void setSizeMax(long sizeMax)

   > 方法setSizeMax方法继承自FileUploadBase类，用于设置请求消息实体内容（即所有上传数据）的最大尺寸限制，以防止客户端恶意上传超大文件来浪费服务器端的存储空间。其参数是以字节为单位的long型数字。
   > 在请求解析的过程中，如果请求消息体内容的大小超过了setSizeMax方法的设置值，将会抛出FileUploadBase内部定义的SizeLimitExceededException异常

3. public ServletFileUpload(FileItemFactory fileItemFactory)

   > 构造一个实例，并根据参数指定的FileItemFactory 对象，设置 fileItemFactory属性


1. public void setSizeMax(long sizeMax)

   > setSizeMax方法继承自FileUploadBase类，用于设置请求消息实体内容（即所有上传数据）的最大尺寸限制，以防止客户端恶意上传超大文件来浪费服务器端的存储空间。其参数是以字节为单位的long型数字。
   >
   > 在请求解析的过程中，如果请求消息体内容的大小超过了setSizeMax方法的设置值，将会抛出FileUploadBase内部定义的SizeLimitExceededException异常(FileUploadException的子类)。

2. public void setFileSizeMax(long fileSizeMax)

   > 方法setFileSizeMax方法继承自FileUploadBase类，用于设置单个上传文件的最大尺寸限制，以防止客户端恶意上传超大文件来浪费服务器端的存储空间。其参数是以字节为单位的long型数字。该方法有一个对应的读方法：public long geFileSizeMax()方法。
   >
   > 在请求解析的过程中，如果单个上传文件的大小超过了setFileSizeMax方法的设置值，将会抛出FileUploadBase内部定义的FileSizeLimitExceededException异常(FileUploadException的子类)。


1. public List parseRequest(javax.servlet.http.HttpServletRequest req)

   > parseRequest 方法是ServletFileUpload类的重要方法，它是对HTTP请求消息体内容进行解析的入口方法。它解析出FORM表单中的每个字段的数据，并将它们分别包装成独立的FileItem对象，然后将这些FileItem对象加入进一个List类型的集合对象中返回。
   >
   > 该方法抛出FileUploadException异常来处理诸如文件尺寸过大、请求消息中的实体内容的类型不是“multipart/form-data”、IO异常、请求消息体长度信息丢失等各种异常。每一种异常都是FileUploadException的一个子类型。

2. public FileItemIterator getItemIterator(HttpServletRequest request)

   > getItemIterator方法和parseRequest 方法基本相同。但是getItemIterator方法返回的是一个迭代器，该迭代器中保存的不是FileItem对象，而是FileItemStream 对象，如果你希望进一步提高性能，你可以采用 getItemIterator 方法，直接获得每一个文件项的数据输入流，做底层处理；如果性能不是问题，你希望代码简单，则采用parseRequest方法即可。 


1. public stiatc boolean isMultipartContent(HttpServletRequest req)

   > isMultipartContent方法方法用于判断请求消息中的内容是否是“multipart/form-data”类型，是则返回true，否则返回false。isMultipartContent方法是一个静态方法，不用创建ServletFileUpload类的实例对象即可被调用

2. getFileItemFactory()和setFileItemFactory(FileItemFactory)

   > 两个方法继承自FileUpload类，用于设置和读取fileItemFactory属性。

3. public void setProgressListener(ProgressListener pListener)

   > 设置文件上传进度监听器。该方法有一个对应的读取方法：ProgressListener getProgressListener()。

4. public void setHeaderEncoding()方法

   > 在文件上传请求的消息体中，除了普通表单域的值是文本内容以外，文件上传字段中的文件路径名也是文本，在内存中保存的是它们的某种字符集编码的字节数组，Apache文件上传组件在读取这些内容时，必须知道它们所采用的字符集编码，才能将它们转换成正确的字符文本返回。
   >
   > setHeaderEncoding方法继承自FileUploadBase类，用于设置上面提到的字符编码。如果没有设置，则对应的读方法getHeaderEncoding()方法返回null，将采用HttpServletRequest设置的字符编码，如果HttpServletRequest的字符编码也为null，则采用系统默认字符编码。可以通过一下语句获得系统默认字符编码：System.getProperty("file.encoding"));

##### 1.3、**DiskFileItemFactory 磁盘文件项工厂类**

1. 构造工厂时，指定内存缓冲区大小和临时文件存放位置

   > public DiskFileItemFactory(int sizeThreshold, File repository) 

2. 设置内存缓冲区大小

   > Apache文件上传组件在解析和处理上传数据中的每个字段内容时，需要临时保存解析出的数据。因为Java虚拟机默认可以使用的内存空间是有限的（测试不大于100M），超出限制时将会发生“java.lang.OutOfMemoryError”错误，如果上传的文件很大，例如上传800M的文件，在内存中将无法保存该文件内容，Apache文件上传组件将用临时文件来保存这些数据；但如果上传的文件很小，例如上传600个字节的文件，显然将其直接保存在内存中更加有效。setSizeThreshold方法用于设置是否使用临时文件保存解析出的数据的那个临界值，该方法传入的参数的单位是字节

3. 默认10K

   > public void setSizeThreshold(int sizeThreshold) 

4. 设置临时文件存放位置

   > public void setRepository(File repository) 
   > 如果不设置存放路径，那么临时文件将被储存在"java.io.tmpdir"这个JVM环境属性所指定的目录中，tomcat 将这个属性设置为了'<tomcat安装目录>/temp/'目录

5. 说明

   - 内存缓冲区

     > 上传文件时，上传文件的内容优先保存在内存缓冲区中，当上传文件大小超过缓冲区大小，就会在服务器端产生临时文件

   - 临时文件存放位置

     > 保存超过了内存缓冲区大小上传文件而产生临时文件 ，产生临时文件可以通过 FileItem的delete()方法删除

#### 3.2、示例代码

1. Java部分

   ```java
   public class UploadServlet extends HttpServlet {

       public void doGet(HttpServletRequest request, HttpServletResponse response)
               throws ServletException, IOException {
                   //得到上传文件的保存目录，将上传的文件存放于WEB-INF目录下，不允许外界直接访问，保证上传文件的安全
                   String savePath = this.getServletContext().getRealPath("/WEB-INF/upload");
                   //上传时生成的临时文件保存目录
                   String tempPath = this.getServletContext().getRealPath("/WEB-INF/temp");
                   File tmpFile = new File(tempPath);
                   if (!tmpFile.exists()) {
                       //创建临时目录
                       tmpFile.mkdir();
                   }
                   
                   //消息提示
                   String message = "";
                   try{
                       //使用Apache文件上传组件处理文件上传步骤：
                       //1、创建一个DiskFileItemFactory工厂
                       DiskFileItemFactory factory = new DiskFileItemFactory();
                       //设置工厂的缓冲区的大小，当上传的文件大小超过缓冲区的大小时，就会生成一个临时文件存放到指定的临时目录当中。
                       factory.setSizeThreshold(1024*100);//设置缓冲区的大小为100KB，如果不指定，那么缓冲区的大小默认是10KB
                       //设置上传时生成的临时文件的保存目录
                       factory.setRepository(tmpFile);
                       //2、创建一个文件上传解析器
                       ServletFileUpload upload = new ServletFileUpload(factory);
                       //监听文件上传进度
                       upload.setProgressListener(new ProgressListener(){
                           public void update(long pBytesRead, long pContentLength, int arg2) {
                               System.out.println("文件大小为：" + pContentLength + ",当前已处理：" + pBytesRead);
                               /**
                                * 文件大小为：14608,当前已处理：4096
                                */
                           }
                       });
                        //解决上传文件名的中文乱码
                       upload.setHeaderEncoding("UTF-8"); 
                       //3、判断提交上来的数据是否是上传表单的数据
                       if(!ServletFileUpload.isMultipartContent(request)){
                           //按照传统方式获取数据
                           return;
                       }
                       
                       //设置上传单个文件的大小的最大值，目前是设置为1024*1024字节，也就是1MB
                       upload.setFileSizeMax(1024*1024);
                       //设置上传文件总量的最大值，最大值=同时上传的多个文件的大小的最大值的和，目前设置为10MB
                       upload.setSizeMax(1024*1024*10);
                       //4、使用ServletFileUpload解析器解析上传数据，解析结果返回的是一个List<FileItem>集合，每一个FileItem对应一个Form表单的输入项
                       List<FileItem> list = upload.parseRequest(request);
                       for(FileItem item : list){
                           //如果fileitem中封装的是普通输入项的数据
                           if(item.isFormField()){
                               String name = item.getFieldName();
                               //解决普通输入项的数据的中文乱码问题
                               String value = item.getString("UTF-8");
                               //value = new String(value.getBytes("iso8859-1"),"UTF-8");
                               System.out.println(name + "=" + value);
                           }else{//如果fileitem中封装的是上传文件
                               //得到上传的文件名称，
                               String filename = item.getName();
                               System.out.println(filename);
                               if(filename==null || filename.trim().equals("")){
                                   continue;
                               }
                               //注意：不同的浏览器提交的文件名是不一样的，有些浏览器提交上来的文件名是带有路径的，如：  c:\a\b\1.txt，而有些只是单纯的文件名，如：1.txt
                               //处理获取到的上传文件的文件名的路径部分，只保留文件名部分
                               filename = filename.substring(filename.lastIndexOf("\\")+1);
                               //得到上传文件的扩展名
                               String fileExtName = filename.substring(filename.lastIndexOf(".")+1);
                               //如果需要限制上传的文件类型，那么可以通过文件的扩展名来判断上传的文件类型是否合法
                               System.out.println("上传的文件的扩展名是："+fileExtName);
                               //获取item中的上传文件的输入流
                               InputStream in = item.getInputStream();
                               //得到文件保存的名称
                               String saveFilename = makeFileName(filename);
                               //得到文件的保存目录
                               String realSavePath = makePath(saveFilename, savePath);
                               //创建一个文件输出流
                               FileOutputStream out = new FileOutputStream(realSavePath + "\\" + saveFilename);
                               //创建一个缓冲区
                               byte buffer[] = new byte[1024];
                               //判断输入流中的数据是否已经读完的标识
                               int len = 0;
                               //循环将输入流读入到缓冲区当中，(len=in.read(buffer))>0就表示in里面还有数据
                               while((len=in.read(buffer))>0){
                                   //使用FileOutputStream输出流将缓冲区的数据写入到指定的目录(savePath + "\\" + filename)当中
                                   out.write(buffer, 0, len);
                               }
                               //关闭输入流
                               in.close();
                               //关闭输出流
                               out.close();
                               //删除处理文件上传时生成的临时文件
                               //item.delete();
                               message = "文件上传成功！";
                           }
                       }
                   }catch (FileUploadBase.FileSizeLimitExceededException e) {
                       e.printStackTrace();
                       request.setAttribute("message", "单个文件超出最大值！！！");
                       request.getRequestDispatcher("/message.jsp").forward(request, response);
                       return;
                   }catch (FileUploadBase.SizeLimitExceededException e) {
                       e.printStackTrace();
                       request.setAttribute("message", "上传文件的总的大小超出限制的最大值！！！");
                       request.getRequestDispatcher("/message.jsp").forward(request, response);
                       return;
                   }catch (Exception e) {
                       message= "文件上传失败！";
                       e.printStackTrace();
                   }
                   request.setAttribute("message",message);
                   request.getRequestDispatcher("/message.jsp").forward(request, response);
       }
       
       /**
       * @Method: makeFileName
       * @生成上传文件的文件名，文件名以：uuid+"_"+文件的原始名称
       * @param filename 文件的原始名称
       * @return uuid+"_"+文件的原始名称
       */ 
       private String makeFileName(String filename){  //2.jpg
           //为防止文件覆盖的现象发生，要为上传文件产生一个唯一的文件名
           return UUID.randomUUID().toString() + "_" + filename;
       }
       
       /**
       * @Method: makePath
       * @param filename 文件名，要根据文件名生成存储目录
       * @param savePath 文件存储路径
       * @return 新的存储目录
       */ 
       private String makePath(String filename,String savePath){
           //得到文件名的hashCode的值，得到的就是filename这个字符串对象在内存中的地址
           int hashcode = filename.hashCode();
           int dir1 = hashcode&0xf;  //0--15
           int dir2 = (hashcode&0xf0)>>4;  //0-15
           //构造新的保存目录
           String dir = savePath + "\\" + dir1 + "\\" + dir2;  //upload\2\3  upload\3\5
           //File既可以代表文件也可以代表目录
           File file = new File(dir);
           //如果目录不存在
           if(!file.exists()){
               //创建目录
               file.mkdirs();
           }
           return dir;
       }

       public void doPost(HttpServletRequest request, HttpServletResponse response)
               throws ServletException, IOException {

           doGet(request, response);
       }
   }
   ```

   1. jsp部分,

   ```jsp
   <!--只需要一个表单。注意表单必须是post的，而且enctype必须是mulitpart/form-data的 -->
   <form action="/uploadimg" 
    method="post" enctype="multipart/form-data">

    用户名：<input type="text" name="username"/><br/>
    文件1：<input type="file" name="img"/><br/>
    <input type="submit" value="上传"/>
   </form>
   ```

2. 注意细节

   1、为保证服务器安全，上传文件应该放在外界无法直接访问的目录下，比如放于WEB-INF目录下。
   2、为防止文件覆盖的现象发生，要为上传文件产生一个唯一的文件名。
   3、要限制上传文件的最大值。
   4、要限制上传文件的类型，在收到上传文件名时，判断后缀名是否合法。

## 二、文件下载

### 1、JAVA代码

```
/**
 * 列出Web系统中所有下载文件
 */ 
public class ListFileServlet extends HttpServlet {

    public void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        //获取上传文件的目录
        String uploadFilePath = this.getServletContext().getRealPath("/WEB-INF/upload");
        //存储要下载的文件名
        Map<String,String> fileNameMap = new HashMap<String,String>();
        //递归遍历filepath目录下的所有文件和目录，将文件的文件名存储到map集合中
        listfile(new File(uploadFilePath),fileNameMap);//File既可以代表一个文件也可以代表一个目录
        //将Map集合发送到listfile.jsp页面进行显示
        request.setAttribute("fileNameMap", fileNameMap);
        request.getRequestDispatcher("/listfile.jsp").forward(request, response);
    }
    
    public void listfile(File file,Map<String,String> map){
        //如果file代表的不是一个文件，而是一个目录
        if(!file.isFile()){
            //列出该目录下的所有文件和目录
            File files[] = file.listFiles();
            //遍历files[]数组
            for(File f : files){
                //递归
                listfile(f,map);
            }
        }else{
            /**
             * 处理文件名，上传后的文件是以uuid_文件名的形式去重新命名的，去除文件名的			 *uuid_部分file.getName().indexOf("_")
             *  那么file.getName().substring(file.getName().indexOf("_")+1)
             */
            String realName = file.getName().substring(file.getName().indexOf("_")+1);
            //file.getName()得到的是文件的原始名称，这个名称是唯一的，因此可以作为key，realName是处理过后的名称，有可能会重复
            map.put(file.getName(), realName);
        }
    }
    
    public void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        doGet(request, response);
    }
}
```

### 2、文件下载jsp页面

```
<%@ page language="java" import="java.util.*" pageEncoding="UTF-8"%>
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
<!DOCTYPE HTML>
<html>
  <head>
    <title>下载文件显示页面</title>
  </head>
  
  <body>
      <!-- 遍历Map集合 -->
    <c:forEach var="me" items="${fileNameMap}">
        <c:url value="/servlet/DownLoadServlet" var="downurl">
            <c:param name="filename" value="${me.key}"></c:param>
        </c:url>
        ${me.value}<a href="${downurl}">下载</a>
        <br/>
    </c:forEach>
  </body>
</html>
```

### 3、下载的Servlet

```
public class DownLoadServlet extends HttpServlet {
  
    public void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        //得到要下载的文件名
        String fileName = request.getParameter("filename");  //23239283-92489-阿凡达.avi
        fileName = new String(fileName.getBytes("iso8859-1"),"UTF-8");
        //上传的文件都是保存在/WEB-INF/upload目录下的子目录当中
        String fileSaveRootPath=this.getServletContext().getRealPath("/WEB-INF/upload");
        //通过文件名找出文件的所在目录
        String path = findFileSavePathByFileName(fileName,fileSaveRootPath);
        //得到要下载的文件
        File file = new File(path + "\\" + fileName);
        //如果文件不存在
        if(!file.exists()){
            request.setAttribute("message", "您要下载的资源已被删除！！");
            request.getRequestDispatcher("/message.jsp").forward(request, response);
            return;
        }
        //处理文件名
        String realname = fileName.substring(fileName.indexOf("_")+1);
        //设置响应头，控制浏览器下载该文件
        response.setHeader("content-disposition", "attachment;filename=" + URLEncoder.encode(realname, "UTF-8"));
        //读取要下载的文件，保存到文件输入流
        FileInputStream in = new FileInputStream(path + "\\" + fileName);
        //创建输出流
        OutputStream out = response.getOutputStream();
        //创建缓冲区
        byte buffer[] = new byte[1024];
        int len = 0;
        //循环将输入流中的内容读取到缓冲区当中
        while((len=in.read(buffer))>0){
            //输出缓冲区的内容到浏览器，实现文件下载
            out.write(buffer, 0, len);
        }
        //关闭文件输入流
        in.close();
        //关闭输出流
        out.close();
    }
    
    /**
	 * 通过文件名和存储上传文件根目录找出要下载的文件的所在路径
     * @param filename 要下载的文件名
     * @param saveRootPath 上传文件保存的根目录，也就是/WEB-INF/upload目录
     * @return 要下载的文件的存储目录
     */ 
    public String findFileSavePathByFileName(String filename,String saveRootPath){
        int hashcode = filename.hashCode();
        int dir1 = hashcode&0xf;  //0--15
        int dir2 = (hashcode&0xf0)>>4;  //0-15
        String dir = saveRootPath + "\\" + dir1 + "\\" + dir2;  //upload\2\3  upload\3\5
        File file = new File(dir);
        if(!file.exists()){
            //创建目录
            file.mkdirs();
        }
        return dir;
    }

    public void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        doGet(request, response);
    }
}
```

