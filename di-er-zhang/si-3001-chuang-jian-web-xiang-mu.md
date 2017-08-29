# 创建Web项目(Eclipse)

## 一、传统的Eclipse创建动态Web项目

1. 在新增项目对话框中选择【Dynamic Web Project】

   ![](http://opzv089nq.bkt.clouddn.com/17-8-27/59672629.jpg)

2. 直接点击【Next】按钮后输入**工程名,选择Tomcat服务器,选择servlet版本**,点击'Next'

   ![](http://opzv089nq.bkt.clouddn.com/17-8-27/92660767.jpg)

3. 点击'Next,**build\classes 存放 java 编译过后的class文件**

   ![](http://opzv089nq.bkt.clouddn.com/17-8-27/73063082.jpg)

4. **Java Web工程的根目录:**WebRoot 可以存放jsp,html,css,js,最后点击'Finish'

   ![](http://opzv089nq.bkt.clouddn.com/17-8-27/68675658.jpg)

5. 工程目录说明

   ![](http://opzv089nq.bkt.clouddn.com/17-8-27/40250067.jpg)

6. 发布工程到WEB 服务器Tomcat

   ![](http://opzv089nq.bkt.clouddn.com/17-8-27/77922628.jpg)

7. 部署完成

   ![](http://opzv089nq.bkt.clouddn.com/17-8-27/66523033.jpg)



## 二、使用Maven来构建Web项目

1. 依次打开Window –> Perferences –> Maven ，展开Maven的配置界面，如下图
   ![](http://opzv089nq.bkt.clouddn.com/17-7-4/52235848.jpg)

2. 然后点击Installations –> add 选择maven安装目录，选择你的Maven安装目录，并点击确定, 之后可以点击Apply,点击OK，即可完成 

   ![](http://opzv089nq.bkt.clouddn.com/17-7-4/79566057.jpg)

3. 在Maven的配置界面，设置User Settings —Global Settings选择maven 安装目录下conf文件夹下的settings.xml，这里我的Maven安装目录为{user}\conf\settings.xml
   ![](http://opzv089nq.bkt.clouddn.com/17-7-4/81066420.jpg)

4. 右键—> new—>project
   ![](http://opzv089nq.bkt.clouddn.com/17-8-29/84469309.jpg)

5. 选maven-archetype-webapp后，next
   ![img](http://images.cnitblog.com/blog/201693/201310/10160452-e2627b10020848e3b385b9e012fdf432.png)

6. 填写相应的信息，Packaged是默认创建一个包，不写也可以

   ![](http://opzv089nq.bkt.clouddn.com/17-8-29/89083070.jpg)

7. 创建好项目后，目录如下：
   ![img](http://images.cnitblog.com/blog/201693/201310/10162503-5c30f5375dc2410d8ff3ac049cc1c145.png)

8. 项目已经创建完毕，下边可是配置。

9. 添加Source Folder
   Maven规定，必须创建以下几个Source Folder
   src/main/resources
   src/main/Java
   src/test/resources
   src/test/java
   添加以上的Source Folder
   ![img](http://images.cnitblog.com/blog/201693/201310/10164131-d6487e13a6f049518e0ead37d9cf3a4b.png)
   ![img](http://images.cnitblog.com/blog/201693/201310/10164259-9e2ac0978eda4b06b5758666b4c322f0.png)

10. 好后的目录如下：

![img](http://images.cnitblog.com/blog/201693/201310/10163838-0146632c1a5a40218959849af2abd575.png)

11. Configure Build Path
   ![img](http://images.cnitblog.com/blog/201693/201310/10164458-e97d972bce1c4798ac4a51d28cd57fab.png)

12. 件夹的输出Output folder，双击修改

    ![img](http://images.cnitblog.com/blog/201693/201310/10164910-d9b3a1caad5248758ccb3407e55aff83.png)

      分别修改输出路径为
      src/main/java　　对应　　target/classes
      src/main/resources　　对应　　target/classes
      src/test/java　　对应　　target/test-classes
      src/test/resources　　对应　　target/test-classes

13. 修改后如下图（新的eclipse可能有所不同）
    ![img](http://images.cnitblog.com/blog/201693/201310/10165443-a52ab62db62046838012e68b8af9363e.png)

14. Libraries

    ![img](http://images.cnitblog.com/blog/201693/201310/10165736-5fdcac5eb76346f8b53bb972aa279773.png)

    ![img](http://images.cnitblog.com/blog/201693/201310/10165628-12ed162df58f4b438afbde3d2d6b4ca8.png)


15. 完Build Path后目录如下：
   ![img](http://images.cnitblog.com/blog/201693/201310/10165905-1c75116b05a1460f846096a624614610.png)

16. 目转换成Dynamic Web Project
    在项目上右键Properties
    在左侧选择 Project Facets,单击右侧的 ”Convert faceted from“
    ![img](http://images.cnitblog.com/blog/201693/201310/15151904-2772ef3fd9ae46aa87492d43018ac35d.png)

17. 修改Java为你当前项目的JDK，并添加Dynamic Web Module ，最后单击”Further Configuration available“ 链接：（此处如果没有链接，可以取消Dynamic选中点击appply然后再次打开此界面选中）
    ![img](http://images.cnitblog.com/blog/201693/201310/15152147-6586dbc7ae9f425c93a6a266bfe132ea.png)

18. Content directory 为 src/main/webapp ,单击OK:

    ![img](http://images.cnitblog.com/blog/201693/201310/15152349-616b3a06e39844fdb44ff923eabbede7.png)



19. 设置完Content directory，ok后再次点击前一界面ok，完成转换成Dynamic Web Project项目

   ![img](http://images.cnitblog.com/blog/201693/201310/15152550-44d0044f9ce34f1083f33c7f994cdd7b.png)

20. 设置部署程序集(Web Deployment Assembly)

    在项目上右键单击，选择Properties，在左侧选择Deployment Assembly
    ![img](http://images.cnitblog.com/blog/201693/201310/15153759-a38429073a1448d39284da65238e0a32.png)

    ​

21. 设置部署时的文件发布路径

    1、我们删除test的两项，因为test是[测试](http://lib.csdn.net/base/softwaretest)使用，并不需要部署。

    2、设置将Maven的jar包发布到lib下。 
    　　　　Add -> [Java ](http://lib.csdn.net/base/java)Build Path Entries -> Maven Dependencies -> Finish　![img](http://images.cnitblog.com/blog/201693/201310/15154111-ddf9bf23bbe74b98af595567773ad142.png)

22. Ok后，web项目就创建完毕了，目录机构如图
    ![img](http://images.cnitblog.com/blog/201693/201310/15154559-13a29cbeb6a0411289044e901197f6b9.png)

## 三、创建Maven Web项目(IntelliJ IDEA)

1. IDEA配置Maven

   ![](http://opzv089nq.bkt.clouddn.com/17-8-27/56070792.jpg)

   ​

2. 第一步

   ![](http://opzv089nq.bkt.clouddn.com/17-8-24/88944034.jpg)

3. 第二步

   ![](http://opzv089nq.bkt.clouddn.com/17-8-27/49631769.jpg)

4. 第三步

   ![](http://opzv089nq.bkt.clouddn.com/17-8-27/38545561.jpg)

5. 第四步

   ![](http://opzv089nq.bkt.clouddn.com/17-8-27/66076017.jpg)

6. 第五步

   ![](http://opzv089nq.bkt.clouddn.com/17-8-27/49607680.jpg)

7. 第六步

   ![](http://opzv089nq.bkt.clouddn.com/17-8-27/56620951.jpg)

8. 部署应用程序

   ![](http://opzv089nq.bkt.clouddn.com/17-8-27/62134852.jpg)

   ![](http://opzv089nq.bkt.clouddn.com/17-8-27/48219670.jpg)

   ![](http://opzv089nq.bkt.clouddn.com/17-8-27/59269765.jpg)

   ![](http://opzv089nq.bkt.clouddn.com/17-8-27/24252815.jpg)

   ![](http://opzv089nq.bkt.clouddn.com/17-8-27/5962114.jpg)

   ![](http://opzv089nq.bkt.clouddn.com/17-8-27/6765812.jpg)

