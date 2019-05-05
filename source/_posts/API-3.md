---
title: API-3
date: 2019-05-05 19:46:40
tags: github
---
### Object
**Object是java提供的一个类，是所有类的父类。**
- **1) toString():**
		- **返回该对象的字符串表示** 
		- **类名@散列码（整数）** 
		- **当输出一个对象的引用时，会自动去调用所属类的toString()**
		- **java中建议：子类对toString()最好都进行重写，使其更具有实际意义**
- **2) equals()**
		- **在Object类中作用等同于 == 的作用**
		- **java中对此方法给出建议：建议在子类中重写此方法，使其更具有实际意义**

### 包装类
#### 什么是包装类
**1）java中对8种基本类型都提供了对应的包装类，用于将数据包装成对象**
**2）java是面向对象的语言，基本数据类型不是对象，所以提供了包装类用于
将基本类型数据包装成对象使用**
**8种基本类型对应的包装类：**
```bash
	byte  --> Byte
	short --> Short
	int  --> Integer
	long  --> Long
	float --> Float
	double --> Double
	char   --> Character
	boolean --> Boolean
```
- **了解：**
	**基本类型和包装类型，若用于计算，基本类型计算的比包装类型计算更快**

**8种包装类中实现的功能都类似，列举其一进行讲解：**
### Integer
- **valueOf(int):Integer**
	- **得到int对应的包装类型对象**
- **intValue():**
	- **从包装类型-->基本类型**
- **parseInt(String):int**
	- **String-->int**
	- **前提：String表示的值一定是可以转换成int类型才可以**
### 自动拆装箱
1. **装箱：将基本类型数据包装成包装类型**
	1. **int-->Integer**
	2. **通过valueOf(int)完成的**
2. **拆箱：将包装类型数据拆成基本类型**
	1. **Integer-->int**
	2. **通过intValue()实现的**
#### 自动装箱，自动拆箱
- **自动装箱，自动拆箱是java从JDK1.5开始提供的功能，是编译器实现的**
	**如何实现：编译过程中，编译器将Integer i = 3;替换成了**
		Integer i = Integer.valueOf(3);

### 文件操作-File
- **File：实现对文件的操作**
- **通过File对象可以实现对文件/目录的以下操作**
	1. **创建，删除文件/目录**
	2. **获取文件/目录的属性信息**
	3. **获取指定目录下的所有子项目**
- **构造方法：**
	- **File(String path)**
	- **File(File parent,String child)**
	- **File(String parent,String child)**
- **方法：**
	1. **创建文件**
		1. **creatNewFile()**
	2. **创建目录**
		1. **mkdir():创建一个目录**
		2. **mkdirs()：创建多级目录**
	3. **delete()：**
		1. **删除文件/目录，注意，若删除的目录下还有子文件/目录，删除失败**
	4. **getName():String**
		1. **获取文件名称**
	5. **length():long**
		1. **获取文件内容的大小（字节）**
		2. **对文件进行读写操作的基本单位是字节**
	6. **exists():boolean**
		1. **判断文件是否存在**
		2. **通常和其他方法一起使用**
			1. **eg:创建文件**
				1. **如果不存在，创建**
				2. **否则不创建，给出提示**
	7. **isFile():boolean**
		1. **判断File对象指向的是否是文件** 
	8. **isDirectory():boolean**
		1. **判断File对象指向的是否是目录**
