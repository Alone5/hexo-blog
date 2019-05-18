---
title: Exception
date: 2019-05-15 21:44:48
tags: github
---
## 异常处理
### JAVA异常概念
- **异常是程序中的一些错误，但并不是所有的错误都是异常，并且错误有时候是可以避免的。**
- **比如说，你的代码少了一个分号，那么运行出来结果是提示是错误 java.lang.Error**

#### 异常发生的原因有很多，通常包含以下几大类：
- **用户输入了非法数据。**
- **要打开的文件不存在。**
- **网络通信时连接中断，或者JVM内存溢出。**
**这些异常有的是因为用户错误引起，有的是程序错误引起的，还有其它一些是因为物理错误引起的。**

#### 要理解Java异常处理是如何工作的，你需要掌握以下三种类型的异常：
- **检查性异常：**
	- **最具代表的检查性异常是用户错误或问题引起的异常，这是程序员无法预见的。例如要打开一个不存在文件时，一个异常就发生了，这些异常在编译时不能被简单地忽略。**
- **运行时异常：**
	-  **运行时异常是可能被程序员避免的异常。与检查性异常相反，运行时异常可以在编译时被忽略。**
- **错误：**
	- **错误不是异常，而是脱离程序员控制的问题。错误在代码中通常被忽略。例如，当栈溢出时，一个错误就发生了，它们在编译也检查不到的。**

### Exception 类的层次
- **所有的异常类是从 java.lang.Exception 类继承的子类。**
- **Exception 类是 Throwable 类的子类。除了Exception类外，Throwable还有一个子类Error 。**

### Java 内置异常类
- **Java 语言定义了一些异常类在 java.lang 标准包中。**
- **标准运行时异常类的子类是最常见的异常类。由于 java.lang 包是默认加载到所有的 Java 程序的，所以大部分从运行时异常类继承而来的异常都可以直接使用。**

- **Java 的非检查性异常:**
	- **ArithmeticException： 当出现异常的运算条件时，抛出此异常。例如，一个整数"除以零"时，抛出此类的一个实例。** 
	- **ArrayIndexOutOfBoundsException： 用非法索引访问数组时抛出的异常。如果索引为负或大于等于数组大小，则该索引为非法索引。** 
	- **ArrayStoreException： 试图将错误类型的对象存储到一个对象数组时抛出的异常。** 
	- **ClassCastException： 当试图将对象强制转换为不是实例的子类时，抛出该异常。** 
	- **IllegalArgumentException： 抛出的异常表明向方法传递了一个不合法或不正确的参数。** 
	- **IllegalMonitorStateException： 抛出的异常表明某一线程已经试图等待对象的监视器，或者试图通知其他正在等待对象的监视器而本身没有指定监视器的线程。** 
	- **IllegalStateException： 在非法或不适当的时间调用方法时产生的信号。换句话说，即 Java 环境或 Java 应用程序没有处于请求操作所要求的适当状态下。** 
	- **IllegalThreadStateException： 线程没有处于请求操作所要求的适当状态时抛出的异常。** 
	- **IndexOutOfBoundsException： 指示某排序索引（例如对数组、字符串或向量的排序）超出范围时抛出。** 
	- **NegativeArraySizeException： 如果应用程序试图创建大小为负的数组，则抛出该异常。** 
	- **NullPointerException： 当应用程序试图在需要对象的地方使用 `null` 时，抛出该异常** 
	- **NumberFormatException： 当应用程序试图将字符串转换成一种数值类型，但该字符串不能转换为适当格式时，抛出该异常。** 
	- **SecurityException： 由安全管理器抛出的异常，指示存在安全侵犯。** 
	- **StringIndexOutOfBoundsException： 此异常由 `String` 方法抛出，指示索引或者为负，或者超出字符串的大小。** 
	- **UnsupportedOperationException： 当不支持请求的操作时，抛出该异常。** 

- **Java 定义在 java.lang 包中的检查性异常类：**
	- **ClassNotFoundException： 应用程序试图加载类时，找不到相应的类，抛出该异常。** 
	- **CloneNotSupportedException： 当调用 `Object` 类中的 `clone` 方法克隆对象，但该对象的类无法实现 `Cloneable` 接口时，抛出该异常。**
	- **IllegalAccessException： 拒绝访问一个类的时候，抛出该异常。**
	- **InstantiationException： 当试图使用 `Class` 类中的 `newInstance` 方法创建一个类的实例，而指定的类对象因为是一个接口或是一个抽象类而无法实例化时，抛出该异常。** 
	- **InterruptedException： 一个线程被另一个线程中断，抛出该异常。** 
	- **NoSuchFieldException： 请求的变量不存在** 
	- **NoSuchMethodException： 请求的方法不存在**

### 捕获异常
- **使用 try 和 catch 关键字可以捕获异常。try/catch 代码块放在异常可能发生的地方。
try/catch代码块中的代码称为保护代码，使用 try/catch 的语法如下：**
```bash
try{
代码块
}catch(XXXExecption e){
处理try中出现的XXXExecption的代码片段
}
```
- **当出现了异常时，JVM会实例化该异常的实例并将其抛出**

### 多重捕获块
- **catch是可以定义多个的，当针对不同的异常,我们有不同的处理手段时，我们可以分别捕获这些异常并处理。但应当有一个好的习惯，在最后一个catch中捕获Exception，Exception,可以防止因为一个未处理的异常导致程序中断。**
```bash
try{
   // 程序代码
}catch(异常类型1 异常的变量名1){
  // 程序代码
}catch(异常类型2 异常的变量名2){
  // 程序代码
}catch(异常类型2 异常的变量名2){
  // 程序代码
}
```

### throws/throw 关键字：
- **如果一个方法没有捕获到一个检查性异常，那么该方法必须使用 throws 关键字来声明。throws 关键字放在方法签名的尾部。**
- **一个方法可以声明抛出多个异常，多个异常之间用逗号隔开。**
- **使用throw关键字可以将一个异常抛出。通常如下情况我们会对外主动抛出异常:**
	- **1.程序执行中发现一个满足语法要求，但是不满足业务逻辑要求的情况时,可以对外抛出异常告知不应当这样做。**
	- **2.程序确实出现了异常，但是该异常不应当在当前代码片段中被解决时,可以对外抛出给调用者解决(责任制问题)**
- **当我们调用一个含有throws声明异常抛出的方法时，编译器要求我们必须处理这个异常，否则编译不通过。**
	- **两种处理方式：**
		- **1.使用try-catch捕获并处理这个异常**
		- **2.在当前方法上继续使用throws将这个异常抛出,具体选择哪种解决方式取决于处理异常的责任问题**

### finally关键字
- **finally块是异常处理机制的最后一块，可以直接跟在try块之后或者最后一个catch之后。**
- **finally块可以确保只要代码执行到try当中，无论try块 当中的代码是否抛出异常，finally块中的代码都必定执行，通常我们将释放的资源这样的操作放在finally中确保执行， 比如IO中关闭流的操作。**

#### 注意：
- **catch 不能独立于 try 存在。**
- **在 try/catch 后面添加 finally 块并非强制性要求的。**
- **try 代码后不能既没 catch 块也没 finally 块。**
- **try, catch, finally 块之间不能添加任何代码。**

### 声明自定义异常
**在 Java 中你可以自定义异常。编写自己的异常类时需要记住下面的几点。**
- **所有异常都必须是 Throwable 的子类。**
- **如果希望写一个检查性异常类，则需要继承 Exception 类。**
- **如果你想写一个运行时异常类，那么需要继承 RuntimeException 类。**

### 通用异常
**在Java中定义了两种类型的异常和错误。**
- **JVM(Java虚拟机)异常：由 JVM 抛出的异常或错误。例如：NullPointerException 类，ArrayIndexOutOfBoundsException 类，ClassCastException 类。**
- **程序级异常：由程序或者API程序抛出的异常。例如 IllegalArgumentException 类，IllegalStateException 类。**