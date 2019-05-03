---
title: API-2
date: 2019-04-29 22:20:46
tags: github
---
### String 方法：
**startsWith(String str):boolean  //判断字符串是否以给定的前缀开头**
**endsWith(String str):boolean   //判断字符串是否以给定的后缀结尾**
**toUpperCase():                             //将字符串中所有的英文转换为大写**
**toLowerCase()：                          //将字符串中所有的英文转换为小写**
**valueOf(otherType):String       //将其他类型的数据转换为String类型**

```bash
    public static void main(String[] args) {
		String str = "a";
		for(int i=0;i<10000000;i++) {
			str += "a";
		}
		System.out.println("执行结束");
	}
	本程序存在的问题：会创建大量的对象，严重降低内存的性能
	针对于大量的字符串拼接，java中提供了StringBuilder类来完成字符串的修改操作
```
### StringBuilder
**是专门设计用于做字符串修改操作的，提供了编辑字符串对应的增，删，改，插操作**
```bash
构造方法有：
StringBuilder()
StringBuilder(String str)
方法：
append（String）//拼接字符串
delete(int start,int end) //删除从start开始到end结束的字符串
insert(int index,String str) // 将给定的字符串插入到index位置
replace(int start,int end,String str)// 使用给定的字符串替换start-end之间的字符串
reverse()// 判断回文
```
**如果只有两个String进行拼接，"+"可以**
**但是如果是大量的字符串进行拼接，就应该使用StringBuilder对象**
**StringBuffer和StringBuilder功能相同，api的使用也是相同的，**
**区别在于StringBuilder是非线程安全的，StringBuffer是线程安全的**

### 正则表达式
**什么是正则：**
**正则是一个字符串，是用于规定字符出现次序的规则，正则是独立的一套体系，很多语言都提供了对其的支持。**
**java提供了一个简单的方式用于验证用户的输入内容：通过正则表达式验证**

#### 语法：
	- 字符集
	1. [] :匹配一位字符
	2. 预定义字符集
			. 任意字符
			\d 任意数字
			\w 单词字符
			\s 空白字符
	3. 数量词：
			X？ 0次或1次
			X+  出现1次以上
			X*  出现任意次
			X{n} 出现n次
			X{n,} 出现至少n次
			X{n,m} 至少出现n次，最多m次  
** 分组(): 经常和|配合使用**
	- 将（）内的内容看成一个整体

**注意点：**
1. **java中的\和正则中的\都是转义字符，冲突，应使用\\**
2. **若要使用正则中的一些特殊字符的本意，需要进行转义**
3. **通过String中的matches(regex)对字符串进行正则校验**

**正则表达式中若想使用一些特殊字符的本意，需要转义  \\**
```bash
特殊字符：
       .
       +
       ？
       *
      （
       {
```
**边界匹配符：**
1. **^开头  $结尾**
2. **java中的matches(regex)自带边界匹配功能，进行的完全匹配**

### String类中的正则api
	- matches(String regex)
	- split(String regex)
	- replaceAll(String regex,String replacement):
		              - 实际工作中，常用于做过滤敏感字符