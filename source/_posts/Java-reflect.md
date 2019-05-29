---
title: Java-reflect
date: 2019-05-28 18:57:50
tags: github
---
## Java 反射
### 什么是反射？
- **反射 (Reflection) 是 Java 的特征之一，它允许运行中的 Java 程序获取自身的信息，并且可以操作类或对象的内部属性。**
- **简而言之，通过反射，我们可以在运行时获得程序或程序集中每一个类型的成员和成员的信息。程序中一般的对象的类型都是在编译期就确定下来的，而 Java 反射机制可以动态地创建对象并调用其属性，这样的对象的类型在编译期是未知的。所以我们可以通过反射机制直接创建对象，即使这个对象的类型在编译期是未知的。**
- **反射的核心是 JVM 在运行时才动态加载类或调用方法/访问属性，它不需要事先（写代码的时候或编译期）知道运行对象是谁。**
- **Java 反射主要提供以下功能：**
	- **在运行时判断任意一个对象所属的类；**
	- **在运行时构造任意一个类的对象；**
	- **在运行时判断任意一个类所具有的成员变量和方法（通过反射甚至可以调用private方法）；**
	- **在运行时调用任意一个对象的方法**
**重点：是运行时而不是编译时**

- **注意事项：**
- 由于反射会额外消耗一定的系统资源，因此如果不需要动态地创建一个对象，那么就不需要用反射。
- 另外，反射调用方法时可以忽略权限检查，因此可能会破坏封装性而导致安全问题。

### Class类
- 类对象，它的每一个实例用于表示JVM加载的一个类。并且在JVM内部，每个被加载的类有且只有一个类对象。通过类对象可以得知其表示的类的一切信息(类名，有哪些方法，属性，构造器等)，并且可以快速实例化该类的实例。

#### Class.forName() 动态加载类
- 动态加载类到内存方法区：
```bash
Class cls = Class.forName(类名)
```
1. 类名是运行期间动态输入的类名，可以任何类名。
2. 返回值是一个引用，利用这个引用指向的对象可以访问方法区中的类信息。
3. 如果类名是错的将会出现"类没有找到"异常，(ClassNotFoundException)
```java
package reflect;
import java.util.Scanner;
public class ReflectDemo1 {
	public static void main(String[] args) throws ClassNotFoundException {
		Scanner scan = new Scanner(System.in);   
		System.out.println("请输入要加载的类名：");
		String className = scan.nextLine();
//		Class cls = Class.forName(className);
		Class cls = String.class;
		String name = cls.getName();
		System.out.println(name);
		}
}
```

#### newInstance() 动态创建对象
- Class提供了动态创建对象的方法：
```java
Object newInstance()
```
1. newInstance方法将调用类信息中的无参数构造器创建对象，如果没有无参数构造器，将抛出没有方法的异常。
2. 如果需要调用有参数构造器，可以利用Constructor API实现。
3. 返回值引用动态创建的对象，因为可以是任何类型的对象，所以其类型为Object。
```java
package reflect;
import java.util.Scanner;
public class ReflectDemo2 {
	public static void main(String[] args) throws ClassNotFoundException, InstantiationException, IllegalAccessException {
	Scanner scan = new Scanner(System.in);
		System.out.println("请输入要实例化的类名：");
		String className = scan.nextLine();
		Class cls = Class.forName(className);
		Object o = cls.newInstance();
		System.out.println(o);
		}
}
```

### 动态调用方法
#### 动态发现方法
- Class提供了方法可以动态获取类的全部方法信息：
```java
Method[] getMethods()
```
1. Method代表方法信息，可以利用Method API获取方法对详细信息，如：方法名，返回值类型列表等。
2. 这个方法返回对数组代表当前类中对全部方法信息，每个元素代表一个方法信息。
```java
package reflect;
import java.util.Scanner;
import java.lang.reflect.Method;
public class ReflectDemo1 {
	public static void main(String[] args) throws ClassNotFoundException {
		Scanner scan = new Scanner(System.in);   
		System.out.println("请输入要加载的类名：");
		String className = scan.nextLine();
//		Class cls = Class.forName(className);
		Class cls = String.class;
		String name = cls.getName();
		System.out.println(name);
		//Method类的每一个实例用于表示一个具体的方法
		Method[] methods = cls.getMethods();
		for(Method m : methods) {
			System.out.println(m.getName());
		}
	}
}
```

#### 动态执行方法
- Method提供了动态执行一个方法的方法：
```java
Object invoke(obj, args)
```
1. obj代表一个对象，该对象上一定包含当前方法！否则将会出现调用异常，如果obj为null则抛出空指针异常。
2. args代表调用方法时候传递的实际参数，如果没有参数可以不用或者传递null，但是要注意参数的个数和类型必须和要调用的方法匹配，否则会出现参数错误异常。
3. 返回值表示方法执行的结果，因为可能是任何类型，则其类型为Object，调用没有返回值的方法则返回值为null；
4. 当被调用方法执行出现异常时候抛出InvocationTargetException。
```java
package reflect;
import java.lang.reflect.Method;
public class ReflectDemo4 {
	public static void main(String[] args) throws Exception {
		/*
		 * 1:加载类对象
		 * 2：获取对应方法
		 * 3：执行该方法
		 */
		Class cls = Class.forName("reflect.Person");
		Object o = cls.newInstance();
//		Method m = cls.getMethod("sayHello");
//		m.invoke(o);
		
		Method me = cls.getMethod("say", String.class);
		me.invoke(o, "wangtao");
	}
}
```

#### 执行不可访问方法
- 如果利用反射API调用了没有可访问权限时候会抛出异常：IllegalAccessException，表示没有访问权限。
- 但是在Method方法上提供了解除访问限制的方法：
```java
setAccessible(boolean flag)
```
- 在invoke之前使用这个方法可以解除访问限制，实现访问没有权限的方法。
- 注意：这个功能破坏了面向对象原有的封装性，但是利用它却能写出一些特殊功能代码。

### 变长参数
- 在Java5 中提供了变长参数（varargs），也就是在方法定义中可以使用个数不确定的参数，对于同一方法可以使用不同个数的参数调用。
- 演示案例：
```java
package reflect;
public class ReflectDemo5 {
	public static void main(String[] args) {
		dosome();
		dosome("one","two");
		dosome("one","two","three");
		dosome("one","two","three","one","two","three");
	}
	/**
	 * 变长参数在一个方法中只能定义一个，
	 * 并且只能作为方法的最后一个参数使用。
	 * 变长参数本质就是一个数组。
	 * @param s
	 */
	public static void dosome(String... s) {
		System.out.println(s.length);
	}
}
```


































