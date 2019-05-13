---
title: API-6
date: 2019-05-13 18:08:29
tags: github
---
## 文本数据IO操作
### Reader与Writer
- **Reader是字符输入流的父类,Writer是字符输出流的父类。**
- **字符流是以字符（char）为单位读写数据的,一次处理一个unicode。**
- **字符流的底层仍然是基本的字节流。**
- **字符流封装了字符的编码解码算法。**

- **Reader的常用方法：**
	- **int read()：**
		- **读取一个字节，返回的int值"低16"位有效。**
	- **int read(char[] chs)：**
		- **读取一个字符数组的length个字符并存入该数组，返回值为实际读取到的字符量。**

- **Writer的常用方法：**
	- **void write(int c)：**
		- **写出一个字符，写出给定int值"低16"位表示的字符。**
	- **void write(char[] chs)：**
		- **将给定字符数组中所有字符写出。**
	- **void write(String str)：**
		- **将给定字符串写出。**
	- **void write(char[] chs, int off, int len)：**
		- **将给定的字符数组中从off处开始连续的冷个字符写出。**

### 转换流
- **InputStreamReader：字符输入流**
	- **该流可以设置字符集，并按照指定的字符集从流中按照该编码将字节数据转换为字符并读取。**
- **OutputStreamWriter：字符输出流**
	- **该流可以设置字符集，并按照指定的字符集将字符转换为对应字节都通过该流写出。**
- **InputStreamReader的构造方法允许我们设置字符集：**
	- **InputStreamReader(InputStream in, String charsetName)：**
		- **基于给定的字节输入流以及字符编码创建ISR。**
	- **InputStreamReader(InputStream in)：**
		- **本构造方法会根据系统默认字符集创建ISR。**
- **OutputStreamWriter的构造方法：**
	- **OutputStreamWrite(OutputStream out, String charsetName)：**
		- **基于给定的字节输出流以及字符编码创建OSW。**
	- **OutputStreamWrite(OutputStream out)：**
		- **本构造方法会根据系统默认字符集创建OSW。**

### PrintWriter
- **创建PW对象**
- **PrintWriter是具有自动行刷新的缓冲字符输出流。**
	- **常用构造方法：**
		- **PrintWriter(File file)**
		- **PrintWriter(String fileName)**
		- **PrintWriter(OutputStream out)**
		- **PrintWriter(OutputStream out, boolean autoFlush)**
		- **PrintWriter(Writer writer)**
		- **PrintWriter(Writer writer,  boolean autoFlush)**
			- **其中参数为OutputSream与Writer的构造方法提供了可以传入boolean值的参数，该参数用于表示PrintWriter是否具有自动行刷新。**

- **print与println方法**
- **PrintWriter提供了丰富的重载方法。其中println方法在输出目标数据后自动输出一个系统支持的换行符。若该流是具有自动行刷新的，那么每当通过println方法写出的内容都会被实际写出，而不是进入缓存。**
- **常用方法：**
	- **void print(int i ):打印整数**
	- **void print(char c):打印字符**
	- **void print(boolean b):打印boolean值**
	- **void print(char[] c):打印字符数组**
	- **void print(double d):打印double值**
	- **void print(int float):打印float值**
	- **void print(long l):打印long值**
	- **void print(String str):打印字符串**

### BufferedReader
- **BufferedReader是缓冲字符输入流，其内部提供了缓冲区，可以提高读取效率。**
- **常用方法：**
	- **BufferedReader(Reader reader)**

- **BufferedReader提供了一个读取一行字符串的方法：**
	- **String readLine()**
	- **读取一行字符串，返回值不含有该行末尾的换行符，如果返回值为null，则表示读取到了流的末尾，若是读取的文件，则表示文件读取到了末尾。**






























