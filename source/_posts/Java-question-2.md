---
title: Java-question-2
date: 2019-07-18 19:11:51
tags: github
---
## 晨考题

### OOP
1. **什么是方法的重载？构造方法可以重载吗？**
	
- 重载(Overload):发生在一个类中，方法名相同，参数列表不同，方法体不同，与返回值类型无关，重载遵循"编译期"绑定，构造方法可以重载。
	
2. **什么是重写？**
	
	- 重写(Override):
	1. 发生在父子类中，方法名称相同，参数列表相同，方法体不同
2. 遵循"运行期绑定"，看对象的类型来调用方法
	
3. **this关键字的作用**
	
	- this表示当前对象：
		- this.属性   区分成员变量和局部变量
		- this.() 调用本类的某个方法
		- this()表示调用本类构造方法，只能用在构造方法的第一行语句。
	- this关键字只能出现在非static修饰的代码中
	
4. **抽象类与接口的区别**
	
	- 接口是抽象类的变体，接口中所有的方法都是抽象的。而抽象类是声明方法的存在而不去实现它的类。
	- 接口可以多继承，抽象类不行
	- 接口定义方法，不能实现，而抽象类可以实现部分方法。
- 接口中基本数据类型为static 而抽类象不是的。
	
5. **&&和&的区别？可举例说明。**
	
	- &和&&都可以执行关系判断，
	- &: 按位与,当&操作符两边的表达式不是boolean类型时，a&b是把a和b都转换成二进制数然后再进行与的运算，不管前面的条件是否正确，后面都执行.
		- 4&7=100&111=100
- &&：逻辑与,前面条件正确时，才执行后面，不正确时，就不执行，就效率而言，这个更好
	
6. **final关键字的用法。(具体说明)**
	
	1. 修饰变量:变量不可被改变
	2. 修饰方法:方法不可被重写
	3. 修饰类:类不可被继承
- final关键字修饰成员变量，意为初始化后不可改变。该成员变量必需在初始化时赋值，对象一旦创建即不可改变。可以在声明时初始化或在构造方法中进行初始化。

7. **分别写出break、continue、return的作用是什么？**
	- break: 常用在嵌套循环和判断语句中，作用是跳出当前循环(写在哪一层，就跳出哪一层)
	- continue: 跳过当前循环中的剩余语句而开始下一次的循环
	- return: 有返回值时，把方法的返回值返回给调用方，无返回值时，结束方法

8. **static 关键字的用法？**
	1. 用来修饰成员变量，将其变为类的成员，从而实现所有对象对于该成员的共享。
	2. 用来修饰成员方法，将其变为类方法，可以直接使用“类名.方法名”的方式调用，常用于工具类，静态方法可以访问静态成员变量，不能访问非静态成员变量。
	3. 静态块用法，将多个类成员放在一起初始化，使得程序更加规整，其中理解对象的初始化过程非常关键。
	4. static final用来修饰成员变量和成员方法，可简单理解为“全局常量”。

9. **请写出JAVA中的8大基本类型和其在内存中所占用的字节数**
```
    byte	1字节
    short	2字节
    int		4字节
    long	8字节
    float	4字节
    double	8字节
    char	2字节
    boolean	1字节
```

10. **面向对象三大特征:**
    1. 封装:
        1. 类:封装的是对象的属性和行为
        2. 方法:封装的是具体的业务逻辑功能
        3. 访问控制修饰符:封装的是访问的权限
    2. 继承:
        1. 作用:代码的复用
        2. 父类:所有子类共有的属性和行为
           子类:子类所特有的属性和行为
        3. 子继承父后，子具有:父类+子类
        4. 单一继承、多接口实现，传递性
    3. 多态:
        1. 意义:行为的多态、对象的多态
        2. 向上造型、强制类型转换、instanceof
        3. 多态的表现形式:
            - 重写:主要是依据对象
            - 重载:主要是依据参数

### SE
1. **已知如下定义，String s = ”stary”，下面那个表达式是合法的?**
    A. s += ”books”;
    B. char c = s[1];
    C. int len = s.length;
    D. String t = s.toLowerCase();

  **答案：AD**
  解析：
  选项B s为字符串对象，char c=s.toCharArray()[1];
  选项C int len=s.length();

2. **对传入的字符串参数进行对比，如果是以 "db _ " 开头的，则返回"匹配检验成功"，否则则返回"没找到" 。（写出主要代码即可）**

   答案：

    ```java
        public String check(String str){
           if(str != null && str.startsWith("db_") ){
              return "匹配检验成功";
           }else{
              retrun "没找到";
           }
        }
    ```

3. **已知表达式 int[] m= {0，1，2，3，4，5，6}； 下面哪个表达式的值与数组下标量总数相等？**
	A .m.length()		 B.m.length		C.m.length()+1		D.m.length+1

    **答案：B**
    解析：
    数组下标是从零开始的，但是数据下标的总量和数据长度相同。

4. **如何将数值型字符转换为数字？**
**参考答案：**
调用数值类型相应包装类中的方法parse"..." (String) 或 valueOf(String)  即可返回相应基本类型或包装类型数值
    - 例如:
    1. 如何将字串 String 转换成整数 int? 
    ```
        int i = Integer.valueOf(str).intValue();
        或int i=Integer.parseInt(str);
    ```
    2. 如何将字串 String 转换成Integer ?
	```
		Integer integer=Integer.valueOf(str);
	```

5. **写一个线程，每隔10秒钟输出到控制台一个"hello world"。打印10次以后退出**
    
    ```java
        public class Test extends Thread{
            public void run(){
                for(int i=0;i<10;i++){
                    System.out.println("hello world");
                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
            }
    
            }
            public static void main(String[] args) {
                Test t=new Test();
                t.start();
            }
        }
```
    
6. **斐波那契数列，它指的是这样一个数列：1,1,2,3,5,8,13......用递归算法求前10项的系数。**
	
	```java
		public static void main(String[]args){
			System.out.println("系数前20项是：");
			for(int j=1;j<=10;j++){
				System.out.print(getFibo(j)+"\t");
			}
		}
		public  static int getFibo(int i){
			if(i<=0){
				return 0;
			}else if(i==1||i==2){
				return 1;
			}else{
				return getFibo(i-1)+getFibo(i-2);
			}
		}
	```