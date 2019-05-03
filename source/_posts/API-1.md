---
title: API-1
date: 2019-04-26 08:40:44
tags: github
---
### 1.String： String是不可改变对象
**java中规定String一旦创建就不可以改变了。**
```bash
		public static void main(String[] args) {
			String str = "abc";
			str += "def";
			System.out.println(str);  //abcdef
		}
String对象创建后是不可变的，本案例是str引用指向发生了改变，
在内存中新创建了一个String对象“abcdef”,str重新指向了此对象;
原对象“abc”是没有发生改变的
```
{% asset_img 1.png  %}

### 2.String的本质：底层封装了一个字符数组。
#### String的创建:两种方式
**1.String str = new String("abc");**
**通过new创建String对象，常用的构造方法有：**
```bash
	1. new  String(String)
	2. new  String(char[])
```
**2.String str = "hello";**
**字符串字面量，是new String（""）的简写   优点：字符串字面量是被复用的**
#### 注意：
**1. java中字符串字面量之间的拼接，编译器在编译期会进行优化，在.class文件中保存的是拼接后的结果**
**2. 字符串变量之间的拼接，编译器不会优化，底层采用new的方式创建了一个新的String对象**

### 3.String方法操作
**length():获取字符串的长度**
**indexOf系列：**
```bash
indexOf(String str):int //获取指定子字符串在本字符串中第一次出现的索引
indexOf(String str,int fromIndex):int //找出字符串从指定位置开始第一次出现的下标
lastIndexOf(String str):int //获取指定字符串在当前字符串中最后一次出现的下标
```
**substring系列：**
```bash
substring(int fromIndex):String //从指定位置开始截取，直到字符串的末尾
substring(int start,int end):String //截取从两个坐标之间的字符  规则： 包头不包尾  最后坐标加1
```
**trim():String //去掉字符串开头和结尾部分的空白**

**charAt() 方法用于返回指定索引处的字符。索引范围为从 0 到 length() - 1。**
```bash
 判断字符串是否是回文
		String str = "上海自来水来自海上";
		boolean flag = false;
		for(int i=0;i<str.length()/2;i++) {
			char c1 = str.charAt(i);
			char c2 = str.charAt(str.length()-1-i);
			if(c1!=c2) {
				System.out.println("不是回文");
				flag = true;
				break;
			}
		}
		if(!flag) { // !flag 等同于flag为false
			System.out.println("是回文");
		}
```