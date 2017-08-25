# XML详解

## 一、标记语言发展史

标记语言，用一系列约定好的标记来对电子文档进行标记，以实现对电子文档的语义、结构、及格式的定义。这些标记必须很容易的和内容区分，并且易于识别。

1、为了促进数据交换和操作，在20世纪60年代，通过IBM公司研究人员的杰出工作，得出了重要的结论：要提高系统的移植性，必须采用一种通用的文档格式，这种文档的格式必须遵守特定的规则。这也就是创建通用标记语言的指导原则，从人们所产生的将文件结构化为标准的格式的动机出发，IBM创建了通用标记语言。

2、在标记语言的概念达成共识的基础上，IBM公司的研究人员Charles Goldfarb带领的开发团队完善着通用标记语言，将其称为标准通用标记语言，标准通用标记语言成为了IBM内部格式化和维护合法化文件的手段。后来被拓展和修改，作为一种全面的信息标准以适应工业范围的广泛应用，1986年，标准通用标记语言被国际标准化组织（ISO）所采纳。

他的功能非常强大，但是非常复杂，需要许多昂贵的软件配合运行，因此在很长一段时间内没有被推广。

3、1989年，欧洲粒子物理实验室（CERT）的研究员Tim Berners-Lee和Anders Berglund共同创建了一种基于标记的语言HTML，他可看做标准通用标记语言的简单应用，开始时仅仅提供一种对静态文本的信息显示的方法，后来越来越多的标签产生，两大浏览器厂商微软和网景格式，甚至创建了自己的产品的兼容标签，使HTML变得臃肿不堪，兼容性不好。

4、1996年人们开始致力于描述一个新的标记语言，它是一种在WEB中应用标准通用标记语言的灵活性和强大功能的方法，万维网联盟--领导万维网，制定其公共的协议，促进万维网的发展并确保其互操作性的国际组织）专门成立了专家小组以从事这项工作。1998.2，w3c批准了XML1.0规范。可扩展标记语言具备标准通用标记语言的核心特性，但简洁，他的内容甚至不到标准通用标记语言的十分之一

![](http://opzv089nq.bkt.clouddn.com/17-8-25/35293271.jpg)

## 二	XML​

### 1、定义

​	可扩展标记语言（EXtensible Markup Language），是一种标记语言，很类似 HTML。XML 的设计宗旨是传输数据，而非显示数据。XML 标签没有被预定义，需要自行定义标签。XML 是 W3C 的推荐标准。XML先驱是SGML（SGML，通用标识语言标准（Standard Generalized Markup Language)），

### 2、XML和HTML的差异

1. XML不是HTML的替代，他们是为不同的目标的设计的。
2. XML 的设计是为了传输和存储数据，其焦点是数据的内容。
3. HTML 的设计是为了用来显示数据，其焦点是数据的外观。
4. HTML 旨在显示信息，而 XML 旨在传输信息。

### 3、语法规则

1. XML 文档必须有根元素而且只能有一个顶级元素，
2. 必须有关闭标签，
3. 标签对大小写敏感，
4. 元素必须被正确的嵌套，
5. 属性的值必须加引号	

### 4、示例代码

```
<?xml version="1.0" encoding="utf-8"?>	   
<!--XML声明,注意默认编码的选择-->
<person>	                                   
<!--根元素（结束标签）-->
	<id>1</id>        <!--元素-->
	<name></name>     <!--元素-->                  
</person>		
```

### 5、XML 命名规则

1. 名称可以含字母、数字以及其他的字符
2. 名称不能以数字或者标点符号开始 
3. 名称不能以字符 “xml”（或者 XML、Xml）开始 
4. 名称不能包含空格 
5. 可使用任何名称，没有保留的字词

## 三、DTD

1. 概念

```
DTD（文档类型定义）的作用是定义 XML 文档的合法构建模块。它使用一系列的合法元素来定义文档结构。通过 DTD 验证的 XML 是“合法”的 XML
```

1. 优点

```
1.通过 DTD，每一个 XML 文件均可携带一个有关其自身格式的描述。
2.通过 DTD，独立的团体可一致地使用某个标准的 DTD 来交换数据。
3.应用程序也可使用某个标准的 DTD 来验证从外部接收到的数据。
4.还可以使用 DTD 来验证您自身的数据。
```

1. 使用

   1. DTD可被成行的声明在XML文档中，也可作为一个外部引用

   2. DTD包含在XML源文件中，它应当通过下面的语法（<!DOCTYPE 根元素 [元素声明]>）包装在一个 DOCTYPE 声明中

      ```
      <!DOCTYPE person [
        <!ELEMENT person (id,name)>
        <!ELEMENT id      (#PCDATA)>
        <!ELEMENT name    (#PCDATA)>
      ]>

      <person>
      <id>1</id>
      <name>小明</name>
      </person>
      ```

   3. 外部文档声明，DTD在XML文档的外部这里关键是通过<!DOCTYPE person

   4. SYSTEM "xx.dtd">引用“.dtd"文件

      1. person.dtd文件

         ```
         <!DOCTYPE person [
           <!ELEMENT person (id,name)>
           <!ELEMENT id      (#PCDATA)>
           <!ELEMENT name    (#PCDATA)>
           <!ELEMENT age    (#PCDATA)>
           <!ELEMENT phone    (#PCDATA)>
         ]>
         ```

      2. .xml文件

         ```
         <!DOCTYPE person SYSTEM "person.dtd">
         <person>
             <id></id>
             <username></username>
             <age></age>
             <phone></phone>
         </person>
         ```

   ​

## 四 、XML Schema

### 1、概念    

​	 XML Schema(XML Schema Definition，XSD) 语言也称作 XML Schema 定义，作用是定义 XML 文档的合法构建模块，类似 DTD，它是DTE的继承者，它比DTD更强大，而且很有可能在网络应用程序中取代DTD。

### 2、与DTD相比优势

1. XML Schema 可针对未来的需求进行扩展
2. 更完善，功能更强大
3. 基于 XML 编写
4. 支持数据类型
5. 支持命名空间 

### 3、作用

​	XML 文档的合法构建模块，类似 DTD。定义可出现在文档中的元素+属性（及数据类型、默认值、固定值、文本）、子元素（次序、数目）

## 五、xml解析

#### 1、说明:

​	所谓的解析就是获取xml获取节点下的值

#### 2、常用的有四种解析方式

1. DOM解析
2. DOM4J解析 http://dom4j.sourceforge.NET/ 
3. SAX解析
4. JDOM解析 http://www.jdom.org

## 六、DOM解析详解

### 1、说明

​	DOM是用与平台和语言无关的方式表示XML文档的官方W3C标准。DOM是以层次结构组织的节点或信息片断的集合。这个层次结构允许开发人员在树中寻找特定信息。分析该结构通常需要加载整个文档和构造层次结构，然后才能做任何工作。由于它是基于信息层次的，因而DOM被认为是基于树或基于对象的。DOM以及广义的基于树的处理具有几个优点。首先，由于树在内存中是持久的，因此可以修改它以便应用程序能对数据和结构作出更改。它还可以在任何时候在树中上下导航，而不是像SAX那样是一次性的处理。DOM使用起来也要简单得多

### 2、基本使用

#### 1、概要

​	dom操作XML会比较简单，就是将XML看做是一颗树，DOM就是对这颗树的一个数据结构的描述，但对大型XML文件效果可能会不理想

#### 2、操作步骤

1. ##### 实例化工厂类

   ```
   DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
   ```

2. 实例化解析器

   ```
   DocumentBuilder builder = dbf.newDocumentBuilder();
   ```

3. 实例化Document对象

   ```java
   /**
   * 第一：前面有 “/”代表了工程的根目录，例如工程名叫做Day45，“/”代表了Day45
   *
   * 第二：前面没有 “   / ” 代表当前类的目录
   */
   Document document = builder.parse(TestMain.class.getResourceAsStream("/person.xml"));
   ```

4. 实例化根元素节点

   ```java
   Element root = document.getDocumentElement();
   //获取节点的值
   //String nodeValue = root.getNodeValue();
   //获取节点的类型
   //short nodeType = root.getNodeType();
   //获取节点的名字
   //String nodeName = root.getNodeName();
   ```

   ​

5. 获取根元素节点下所有的节点

   ```java
   /**
    *注意 包含此节点的所有子节点的 NodeList。如果不存在子节点，则这是不包含节点的 NodeList  
    * 常见的比如 元素节点 文本节点 ,注释节点
    */
   NodeList childNodes = root.getChildNodes();
   //获得所有节点元素的长度
   //childNodes.getLength()
   // 返回集合中的第 index 个项
   Node child = childNodes.item(index);
   ```

6. 遍历所有元素节点

   ```java
   /**
   *  1>通过childNodes.item[index]得到所有的子节点
   *  注意  注释跟换行也会作为节点
   *  2>判断当前节点是不是元素节点
   *  3>如果是元素节点可以获取里面的属性节点和文本内容,
   *  3.1> 如果元素节点里又包含了节点继续遍历
   *  4>获取里面的值有两种方式
   *  4.1 直接获取通过节点的
   *
   */
   for (int i = 0; i < childNodes.getLength(); i++) {
                   Node item = childNodes.item(i);
                   if (item.getNodeType() == Node.ELEMENT_NODE && item.getNodeName().equals("pid")) {
   		     String pid = item.getFirstChild().getNodeValue();
                     ...
                   }
   ```

## 七、DOM4J解析详解

### 1、说明

1. 最初它是JDOM的一种智能分支。它合并了许多超出基本XML文档表示的功能，包括集成的XPath支持、XML Schema支持以及用于大文档或流化文档的基于事件的处理。它还提供了构建文档表示的选项，它通过DOM4J API和标准DOM接口具有并行访问功能。从2000下半年开始，它就一直处于开发之中。
2. 为支持所有这些功能，DOM4J使用接口和抽象基本类方法。DOM4J大量使用了API中的Collections类，但是在许多情况下，它还提供一些替代方法以允许更好的性能或更直接的编码方法。直接好处是，虽然DOM4J付出了更复杂的API的代价，但是它提供了比JDOM大得多的灵活性。
3. 在添加灵活性、XPath集成和对大文档处理的目标时，DOM4J的目标与JDOM是一样的：针对Java开发者的易用性和直观操作。它还致力于成为比JDOM更完整的解决方案，实现在本质上处理所有Java/XML问题的目标。在完成该目标时，它比JDOM更少强调防止不正确的应用程序行为。
4. DOM4J是一个非常非常优秀的Java XML API，具有性能优异、功能强大和极端易用使用的特点，同时它也是一个开放源代码的软件。如今你可以看到越来越多的Java软件都在使用DOM4J来读写XML，特别值得一提的是连Sun的JAXM也在用DOM4J

### 2、基本使用

#### 2.1、接口的继承关系

```
interface org.dom4j.Node (	
为dom4j中所有的XML节点定义了多态行为)

	|interface org.dom4j.Attribute 	(定义了 XML 的属性)

	|interface org.dom4j.Branch(指能够包含子节点的节点。如XML元素(Element)和文档(Docuemnts)定义了一个公共的行为)

		| interface org.dom4j.Document (	
定义了XML 文档)
		|interface org.dom4j.Element (定义XML 元素)
	
	| interface org.dom4j.CharacterData (是一个标识接口，标识基于字符的节点。如CDATA，Comment, Text)
		|interface org.dom4j.CDATA (	
定义了 XML CDATA 区域)
		|interface org.dom4j.Comment (定义了 XML 注释的行为)
 		|interface org.dom4j.Text (	
定义 XML 文本节点)
```

#### 2.2、类常用API

```
1> class org.dom4j.io.SAXReader
	read  提供多种读取xml文件的方式，返回一个Domcument对象
2> interface org.dom4j.Document
	iterator  使用此法获取node
	getRootElement  获取根节点
3> interface org.dom4j.Node
    getName  获取node名字，例如获取根节点名称为
    getNodeType  获取node类型常量值，
    getNodeTypeName  获取node类型名称
4> interface org.dom4j.Element
    attributes  返回该元素的属性列表
    attributeValue  根据传入的属性名获取属性值
    elementIterator  返回包含子元素的迭代器
    elements  返回包含子元素的列表
5> interface org.dom4j.Attribute
    getName  获取属性名
    getValue  获取属性值

6> interface org.dom4j.Text
	getText  获取Text节点值
7> interface org.dom4j.CDATA
	getText  获取CDATA Section值
8> interface org.dom4j.Comment
	getText  获取注释 
```

#### 2.3、Document对象相关

```
1.读取XML文件,获得document对象.
  SAXReader reader = new SAXReader();
  Document  document = reader.read(new File("input.xml"));
2.解析XML形式的文本,得到document对象(一般在服务器响应的XML格式的字符串,客服端解析)
  String text = "<person></person>";
  Document document = DocumentHelper.parseText(text);
  String text = "<person></person>";
  Document document = DocumentHelper.parseText(text);
```

#### 2.4、节点相关

```
1.获取文档的根节点.
Element rootElm = document.getRootElement();
2.取得某节点的单个子节点.
Element idEle =root.element("pid");// "member"是节点名
3.取得节点的文字
String text=idEle.getText();
或者
String text=root.elementText("pid");这个是取得根节点下的name字节点的文字.
4.取得某节点下名为"address"的所有字节点并进行遍历
例1 List<Element> elements = root.elements();
    for (Element element : elements) {
    }
例2 List<Element> elements = root.elements("address");
    for (Element addElements : elements) {		
    }
```

#### 2.5、属性相关

```
1.取得某节点下的某属性
Element root=document.getRootElement();    
Attribute idAttr=root.attribute("id");// 属性名name

2.取得属性的值
 2.1通过属性获取属性值
 String text=idAttr.getValue();
 2.2通过元素获取属性值
 String text = root.attributeValue("id")
 String text = root.attributeValue("id","默认值");
3. 遍历属性
  List<Attribute> attributes = root.attributes();
  for (Attribute attribute : attributes) {
  }
```

#### 2.6、Element类

| getQName()           | 元素的QName对象                               |
| -------------------- | ---------------------------------------- |
| getNamespace()       | 元素所属的Namespace对象                         |
| getNamespacePrefix() | 元素所属的Namespace对象的prefix                  |
| getNamespaceURI()    | 元素所属的Namespace对象的URI                     |
| getName()            | 元素的local name                            |
| getQualifiedName()   | 元素的qualified name                        |
| getText()            | 元素所含有的text内容，如果内容为空则返回一个空字符串而不是null      |
| getTextTrim()        | 元素所含有的text内容，其中连续的空格被转化为单个空格，该方法不会返回null |
| attributeIterator()  | 元素属性的iterator，其中每个元素都是Attribute对象        |
| attributeValue()     | 元素的某个指定属性所含的值                            |
| elementIterator()    | 元素的子元素的iterator，其中每个元素都是Element对象        |
| element()            | 元素的某个指定（qualified name或者local name）的子元素  |
| elementText()        | 元素的某个指定（qualified name或者local name）的子元素中的text信息 |
| getParent            | 元素的父元素                                   |
| getPath()            | 元素的XPath表达式，其中父元素的qualified name和子元素的qualified name之间使用"/"分隔 |
| isTextOnly()         | 是否该元素只含有text或是空元素                        |
| isRootElement()      | 是否该元素是XML树的根节点                           |

### 3、案例见代码

1. 解析简单数据
2. 解析集合数据

## 八、总结

1. DOM4J性能最好，连Sun的JAXM也在用DOM4J。目前许多开源项目中大量采用DOM4J，例如大名鼎鼎的hibernate也用DOM4J来读取XML配置文件。如果不考虑可移植性，那就采用DOM4J.
2. JDOM和DOM在性能测试时表现不佳，在测试10M文档时内存溢出。在小文档情况下还值得考虑使用DOM和JDOM。虽然JDOM的开发者已经说明他们期望在正式发行版前专注性能问题，但是从性能观点来看，它确实没有值得推荐之处。另外，DOM仍是一个非常好的选择。DOM实现广泛应用于多种编程语言。它还是许多其它与XML相关的标准的基础，因为它正式获得W3C推荐(与基于非标准的Java模型相对)，所以在某些类型的项目中可能也需要它(如在JavaScript中使用DOM)。
3. SAX表现较好，这要依赖于它特定的解析方式－事件驱动。一个SAX检测即将到来的XML流，但并没有载入到内存(当然当XML流被读入时，会有部分文档暂时隐藏在内存中)。