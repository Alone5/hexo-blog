---
title: Java-XML
date: 2019-05-25 10:44:37
tags: github
---
### XML
- **可扩展标记语言（英语：Extensible Markup Language，简称：XML），是一种标记语言。标记指计算机所能理解的信息符号，通过此种标记，计算机之间可以处理包含各种信息的文章等。如何定义这些标记，既可以选择国际通用的标记语言，比如HTML，也可以使用像XML这样由相关人士自由决定的标记语言，这就是语言的可扩展性。XML是从标准通用标记语言（SGML）中简化修改出来的。它主要用到的有可扩展标记语言、可扩展样式语言（XSL）、XBRL和XPath等。**

### 用途
**XML设计用来传送及携带数据信息，不用来表现或展示数据，HTML则用来表现数据，所以XML用途的焦点是它说明数据是什么，以及携带数据信息。**
- **丰富文件（Rich Documents）- 自定文件描述并使其更丰富**
	- **属于文件为主的XML技术应用**
	- **标记是用来定义一份资料应该如何呈现**
- **元数据（Metadata）- 描述其它文件或网络资讯**
	- **属于资料为主的XML技术应用**
	- **标记是用来说明一份资料的意义**
- **配置文档（Configuration Files）- 描述软件设置的参数**

### 处理节点
xml 推荐在第一行，必须写在第一行，可以省略。
```
<?xml version="1.0" encoding="UTF-8"?>
```

### 标记（标签） tag
语法：
```
开始标记 <标记名>  
结束标记    </标记名>
```
标记规则：
1. 中文英文都可以作为标记名，建议使用英文
2. 区分大小写
3. 开始标记和结束标记要成对使用
4. 不能交叉嵌套
5. 一个XML中只有唯一的根标记
标记名可以扩展 标记的嵌套关系可以扩展

### 注释 Comment
语法：
```
<!-- 注释 -->
```
注释不能嵌套使用！！

### 内容 Content
语法： 开始标记和结束标记之间的信息称为内容。
```
<name>檀香刑</name>
<book><name>檀香刑</name></book>
<book>
    <name>檀香刑</name>
    <author>莫言</author>
</book>
```
内容：
1. 文本内容
2. 标记
3. 多个标记和文本的混合
4. 内容是可扩展的！

### 元素 Element
语法：
```
标记 + 内容 = 元素
```
1. XML 只有一个根元素
2. 元素中的元素称为子元素，外层元素称为父元素

### 属性
语法：
```
<book id="b1" lang="cn">
<book lang="cn" id="b1">
```
1. 在开始标记上定义元素的属性
2. 可以定义多个属性
3. 多个属性不分先后次序
4. 属性之间需要有空格
5. 属性名="属性值" 属性和值之间等号前后不用写空格，属性值必须用 引号

### 实体 Entity
实体： 类似于 Java 的字符串中的转义字符，一些用于替换表示XML语法的字符。 用来解决XML文字解析错误
常用实体：
```
< &lt;
> &gt;
& &amp;
```

### CDATA
使用CDATA包裹的文本，可以写任何字符，无需进行实体替换
```
<![CDATA[ 文本内容 ]]>
```

### XML可扩展性
1. 元素（标记）名可以扩展
2. 元素的嵌套关系可以扩展
3. 元素的属性可以扩展
4. 属性名可以扩展
5. 内容可以扩展

### 在Java中使用XML
Java 业界有API可以读取XML文件。读取以后可以按照元素、属性进行结构分解。
Dom4J 读作 Dom for J
Dom4J 的底层
```
IO 流 -> W3C Dom -> Dom4J 
```

### 利用Maven导入dom4j API
得到Dom4j 的maven组件坐标
```
<dependency>
    <groupId>org.dom4j</groupId>
    <artifactId>dom4j</artifactId>
    <version>2.1.1</version>
</dependency>
```
将组件坐标添加到 pom.xml 文件中
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>cn.tedu</groupId>
  <artifactId>XML01</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <dependencies>
    <dependency>
        <groupId>org.dom4j</groupId>
        <artifactId>dom4j</artifactId>
        <version>2.1.1</version>
    </dependency>
  </dependencies>
</project>
```

## Dom4j API 的使用

### 读取XML
Dom4j提供了API将XML文件读取为Dom对象（Document）
```
    /*
     * 使用dom4j读取xml文件
     */
    SAXReader reader = new SAXReader();

    //doc=reader.read(文件、文件流)
    Document doc=
      reader.read(new File("books.xml"));

    System.out.println(doc.asXML());
```

### 利用Dom API（Document）可以访问Dom树中的数据
1. 访问根元素
   ```
   //读取根元素, Root 根， Element元素
   Element root = doc.getRootElement();
   //输出根元素root中的内容
   System.out.println("根元素：");
   System.out.println(root.asXML()); 
   ```
2. 可以获取元素中全部的子元素
   ```
   List<Element> list=root.elements();
   ```
3. 可以获取一批名字一样的子元素
   ```
   List<Element> list= root.elements("book");
   ```
4. 获取一个指定名字的子元素, 适合元素的子元素名字都不同，获取其中的一个子元素时候使用。
   ```
   Element e = book.element("name");
   ```
5. 获取元素中文字内容
   ```
   String s1 = name.getText()
   String s2 = name.getTextTrim() //去除前后空白，常用！
   ```
6. 获取元素的属性值
   ```
   <book id="b1" >
   String s = book.attributeValue("id"); 
   ```
7. 直接获取子元素的文本内容
   ```
   String name = book.elementTextTrim("name");
   ```