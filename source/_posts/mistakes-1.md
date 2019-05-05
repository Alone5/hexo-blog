---
title: 第一次月考
date: 2019-05-04 14:15:54
tags: github
---
**1.(单选题)Outer类中定义了一个成员内部类Inner，需要在main()方法中创建**
**Inner类实例对象，以下四种方式正确的是（ ）。**
**A.Inner in = new Inner();** 
**B.Inner in = new Outer.Inner();** 
**C.Outer.Inner in = new Outer.Inner();** 
**D.Outer.Inner in = new Outer().new Inner();** 
**【正确答案】D**
**【答案解析】本题考查静态方法创建实例内部类对象 内部类的最重要的一个特点就是它可以直接访问它外部类的成员变量。成员变量是 对象身上的。对象创建完成了,才会为成员变量分配空间。能调用成员变量, 意味着一定有了实例对象. main方法是静态的，它执行的时候可以不用创建那个对象。这就矛盾了。 main方法运行的时候没有任何外部类的实例对象。我们的内部类的实例对象创建了 它又可以访问外部类的成员变量，外部类的实例对象在哪儿了？ 所以这个原因，要想创建内部类的实例对象，必须创建外部类的实例对象 选项D正确。** 

**2.(单选题)下列运算结果正确的是（ ）。**
**A.int x = 4590;**
**x = x / 1000 *1000;**
**x的结果是4590** 
**B.int x = 3;**
**x = x++;**
**x的结果是4** 
**C.short x = 3;**
**x = x+4;**
**x的结果是7** 
**D.int x = 2;**
**boolean b = x>3 && x<6;**
**b的结果是false** 
**【正确答案】D**
**【答案解析】本题考查运算符的使用 选项A，java中整数的直接量都是int类型，两个整型相除，忽略余数，结果还是int类型，4590/1000结果是4,41000=4000，结果为4000，错误 选项B，程序的执行顺序先运算赋值运算，变量再自增，所以x的结果是3 选项C，编译错误** 

**3.(单选题)下列程序输出结果是（ ）。**
**class Super {**
	**public static void m1() {**
		**System.out.println("m1 in Super");	}**
	**public void m2() {**
		**System.out.println("m2 in Super");	}}**
**class Sub extends Super {**
	**public static void m1() {**
		**System.out.println("m1 in Sub");	}**
	**public void m2() {**
		**System.out.println("m2 in Sub");	}}**
**public class TestMain {**
	**public static void main(String args[]) {**
		**Super sup = new Sub();**
		**sup.m1();**
		**sup.m2();**
		**Sub sub = (Sub) sup;**
		**sub.m1();**
		**sub.m2();	}}**
**A.m1 in Sub**
   **m2 in Sub**
   **m1 in Sub**
   **m2 in Sub** 
**B.m1 in Super**
   **m2 in Super**
   **m1 in Sub**
   **m2 in Sub** 
**C.m1 in Sub**
   **m2 in Sub**
   **m1 in Super**
   **m2 in Super** 
**D.m1 in Super**
   **m2 in Sub**
   **m1 in Sub**
   **m2 in Sub** 
**【正确答案】D**
**【答案解析】静态方法可以被继承和隐藏  而不能够被重写，因此不能实现多态，不能实现父类的引用可以指向不同子类的对象进行多态调用。 选项D正确** 

**4.(多选题)请选出属于JDK的组成部分（ ）。**
**A.支持JAVA程序运行的一组类库** 
**B.JVM** 
**C.支持JAVA程序开发的一组开发工具** 
**D.一组软件测试工具** 
**【正确答案】A,B,C**
**【答案解析】JDK：JAVA开发工具集，包含JAVA程序的运行环境和辅助JAVA开发的一组开发工具。运行环境又包括一组类库和JVM** 

**5.(多选题)下面关于数组的说法中，正确的是（ ）。**
**A.在类中声明一个整数数组作为成员变量，如果没有给它赋值，数值元素值为空** 
**B.数组可以在内存空间连续存储任意一组数据** 
**C.数组必须先声明，然后才能使用** 
**D.数组本身是一个对象** 
**【正确答案】C,D**
**【答案解析】本题考查数组的基本使用 数组中元素如果是数值元素初值为0值，数组中元素类型必须一致，数组必须先声明 再使用，数组在内存中以对象的形式存在。选项CD正确。** 

**6.(多选题)下列关于JDK的说法正确的是（ ）**
**A.开发一个Java程序，需要完整的JDK** 
**B.运行一个Java程序，需要完整的JDK** 
**C.运行一个Java程序，只需要JDK中的JRE** 
**D.以上说法都不对** 
**【正确答案】A,C**
**【答案解析】本题考查JDK的组成和每个组成部分的功能 JDK是Java语言的软件开发工具包，其中包括JRE和一组开发工具 选项A，开发一个JAVA程序，需要开发工具，所以需要完整的JDK，正确 选项B，运行一个Java程序只需要JRE 错误 选项C，运行一个Java程序，只需要JDK中的JRE 正确 选项D，A C 选项是正确的，所以该选项是错误的** 

**7.(多选题)给定java代码如下所示，在1处新增下列（  ）方法，是对show方法的重载**
**public class Test{ 
	public void show(int x, int y, int z) {    }** 
	**//1**
**}**
**A.public int show(int x,int y,float z){return 0;}** 
**B.public int show(int x,int y,int z){return 0;}** 
**C.public void show(int x,int z){}** 
**D.public viod show(int z,int y,int x){}** 
**【正确答案】A,C**
**【答案解析】本题考查方法的重载规则 方法的重载必须满足方法名相同和参数不同（类型或个数） 选项AC满足重载的条件正确 选项BD参数都与定义的方法参数相同，不是重载。** 

**8.(多选题)给出以下代码，请问该程序的运行结果是什么？** 
**class A{** 
	**private int x=0;**
	**static int y=1;** 
	**protected int z=2;** 
**}** 
**class B extends A{** 
	**void method(){** 
		**System.out.println(x);** 
		**System.out.println(y);** 
		**System.out.println(z);** 
	**}** 
**}** 
**下列说法正确的是（ ）。**
**A.程序编译错误，因为类B不能访问变量x。** 
**B.程序编译成功，打印输出012。** 
**C.程序编译错误，如果移走System.out.println(x)这条语句，**
**可使编译程序通过。** 
**D.程序编译错误,因为变量x未在类B中定义。** 
**【正确答案】A,C,D**
**【答案解析】本题考查java中几种访问修饰符 public：公共的，可以在任何类中访问 protected：受保护的，只能在同一个包中，或子类中访问 default：只能在同一个包中的类中访问 private:私有的，只能在本类中访问的 本题代码中，变量x是私有的，不能在类外部访问，ACD正确。**

**9.(多选题)关于类、对象、引用以下描述正确的是（ ）。**
**A.Person person = new Person("张三");**
**person是一个对象，是内存中的一块区域，是Person类的一个实例** 
**B.Person person = new Person("张三");**
**person并不是真正的对象，而是指向所创建的对象的引用** 
**C.Person person;**
**person = new Person("张三");**
**person = new Person("李四");**
**person是引用类型的变量，指向了“张三”和“李四”两个对象** 
**D.Person person;**
**person = new Person("张三");**
**person = new Person("李四");**
**person是引用类型的变量，最后指向了“李四”这个对象** 
**【正确答案】B,D**
**【答案解析】本题考查引用类型变量的含义 引用类型的变量可以存放该类对象的地址信息，通常称为“指向该类对象”； 选项A和B中，person并不是所创建的对象，是一个引用，是一个可以指向Person类的对象的引用。真正创建对象的语句是右边的new Person("张三"); A错误，B正确 选项C和D，这里让person先指向了“张三”这个对象，然后又指向了“李四”这个对象。也就是说，Person person，这句话只是声明了一个Person类的引用，它可以指向任何Person类的实例 A错误，D正确** 

