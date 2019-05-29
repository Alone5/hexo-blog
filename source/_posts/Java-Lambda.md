---
title: Java-Lambda
date: 2019-05-29 19:22:42
tags: github
---
## Lambda表达式
- Java 8的一个大亮点是引入Lambda表达式，使用它设计的代码会更加简洁。当开发者在编写Lambda表达式时，也会随之被编译成一个函数式接口。
- 使用Lambda表达式不仅让代码变的简单、而且可读、最重要的是代码量也随之减少很多。
- **语法：**
```java
(参数列表)->{
方法体
}
```

- **lambda表达式的重要特征:**
	- 可选类型声明：不需要声明参数类型，编译器可以统一识别参数值。
	- 可选的参数圆括号：一个参数无需定义圆括号，但多个参数需要定义圆括号。
	- 可选的大括号：如果主体包含了一个语句，就不需要使用大括号。
	- 可选的返回关键字：如果主体只有一个表达式返回值则编译器会自动返回值，大括号需要指定明表达式返回了一个数值。

- 没有使用Lambda的老方法：
```java
Runnable runnable1=new Runnable(){
	public void run(){
	    System.out.println("Hello!");
	}
};
```

- 使用Lambda:
```java
Runnable r2 = () -> {
	System.out.println("Hi!");
	};
// 只有一句代码 {}可以省略
Runnable r3 = () -> System.out.println("Hi!");
```

- 方法中含有参数:
```java
public static void main(String[] args) {
		List<String> list = new ArrayList<>();
		list.add("one");
		list.add("three");
		list.add("four");
		
		Collections.sort(list,new Comparator<String>(){
			public int compare(String o1, String o2) {
				return o2.length()-o1.length();
			}
		});
		System.out.println(list);
		/*
		 * 参数类型无需指定，编译器会根据程序长下文分析出参数类型，
		 * 若不能确定时需要指定(否则编译不通过)
		 */
		Collections.sort(list,(o1,o2)->{
			return o1.length()-o2.length();
		});
		System.out.println(list);
		/*
		 * 如果可以忽略{} 那么方法中的return关键字也要省略
		 */
		Collections.sort(list,(o1,o2)->o1.length()-o2.length());
	}
```


