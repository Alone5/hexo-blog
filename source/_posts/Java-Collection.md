---
title: Java-Collection
date: 2019-05-22 16:24:16
tags: github
---
### Java 集合框架
{% asset_img Collection.gif 集合框架图 %}
- **从上面的集合框架图可以看到，Java 集合框架主要包括两种类型的容器，一种是集合（Collection），存储一个元素集合，另一种是图（Map），存储键/值对映射。Collection 接口又有 3 种子类型，List、Set 和 Queue，再下面是一些抽象类，最后是具体实现类，常用的有 ArrayList、LinkedList、HashSet、LinkedHashSet、HashMap、LinkedHashMap 等等。**
- **集合框架是一个用来代表和操纵集合的统一架构。所有的集合框架都包含如下内容：**
	- **接口：是代表集合的抽象数据类型。例如 Collection、List、Set、Map 等。之所以定义多个接口，是为了以不同的方式操作集合对象**
	- **实现（类）：是集合接口的具体实现。从本质上讲，它们是可重复使用的数据结构，例如：ArrayList、LinkedList、HashSet、HashMap。**
	- **算法：是实现集合接口的对象里的方法执行的一些有用的计算，例如：搜索和排序。这些算法被称为多态，那是因为相同的方法可以在相似的接口上有着不同的实现。**
**集合与数组一样，都是用来保存一组数据的，但是集合提供了操作元素的相关方法，使用更方便，并且集合有多种不同的数据结构实现。**

### Java集合和数组的区别：
- **数组长度在初始化时指定，意味着只能保存定长的数据。而集合可以保存数量不确定的数据。同时可以保存具有映射关系的数据（即关联数组，键值对 key-value）。**
- **数组元素即可以是基本类型的值，也可以是对象。集合里只能保存对象（实际上只是保存对象的引用变量），基本数据类型的变量要转换成对应的包装类才能放入集合类中。**

### 集合接口
- **Collection 接口**
**Collection 是最基本的集合接口，一个 Collection 代表一组 Object，即 Collection 的元素, Java不提供直接继承自Collection的类，只提供继承于的子接口(如List和set)。**
**Collection 接口存储一组不唯一，无序的对象。**

- **List 接口**
**List接口是一个有序的 Collection，使用此接口能够精确的控制每个元素插入的位置，能够通过索引(元素在List中位置，类似于数组的下标)来访问List中的元素，第一个元素的索引为 0，而且允许有相同的元素。**
**List 接口存储一组不唯一，有序（插入顺序）的对象。**

- **Set**
**Set 具有与 Collection 完全一样的接口，只是行为上不同，Set 不保存重复的元素。**
**Set 接口存储一组唯一，无序的对象。**

- **SortedSet** 
**继承于Set保存有序的集合。**

- **Map**
**Map 接口存储一组键值对象，提供key（键）到value（值）的映射。**

- **Map.Entry** 
**描述在一个Map中的一个元素（键/值对）。是一个Map的内部类。**

- **SortedMap**
**继承于 Map，使 Key 保持在升序排列。**

- **Enumeration**
**这是一个传统的接口和定义的方法，通过它可以枚举（一次获得一个）对象集合中的元素。这个传统接口已被迭代器取代。**
- **Set和List的区别：**
	- **Set 接口实例存储的是无序的，不重复的数据。List 接口实例存储的是有序的，可以重复的元素。**
	- **Set检索效率低下，删除和插入效率高，插入和删除不会引起元素位置改变 <实现类有HashSet,TreeSet>。**
	- **List和数组类似，可以动态增长，根据实际存储的数据的长度自动增长List的长度。查找元素效率高，插入删除效率低，因为会引起其他元素位置改变 <实现类有ArrayList,LinkedList,Vector> 。**

### 集合实现类（集合类）
- **Java提供了一套实现了Collection接口的标准集合类。其中一些是具体类，这些类可以直接拿来使用，而另外一些是抽象类，提供了接口的部分实现。**
- **AbstractCollection** 
**实现了大部分的集合接口。**
- **AbstractList** 
**继承于AbstractCollection 并且实现了大部分List接口。**
- **AbstractSequentialList** 
**继承于 AbstractList ，提供了对数据元素的链式访问而不是随机访问。**
- **LinkedList**
**该类实现了List接口，允许有null（空）元素。主要用于创建链表数据结构，该类没有同步方法，如果多个线程同时访问一个List，则必须自己实现访问同步，解决方法就是在创建List时候构造一个同步的List。例如：**
```bash
Listlist=Collections.synchronizedList(newLinkedList(...));
LinkedList 查找效率低。
```
- **ArrayList**
**该类也是实现了List的接口，实现了可变大小的数组，随机访问和遍历元素时，提供更好的性能。该类也是非同步的,在多线程的情况下不要使用。ArrayList 增长当前长度的50%，插入删除效率低。**
- **AbstractSet** 
**继承于AbstractCollection 并且实现了大部分Set接口。**
- **HashSet**
**该类实现了Set接口，不允许出现重复元素，不保证集合中元素的顺序，允许包含值为null的元素，但最多只能一个。**
- **LinkedHashSet**
**具有可预知迭代顺序的 Set 接口的哈希表和链接列表实现。**
- **TreeSet**
**该类实现了Set接口，可以实现排序等功能。**
- **AbstractMap** 
**实现了大部分的Map接口。**
- **HashMap** 
**HashMap 是一个散列表，它存储的内容是键值对(key-value)映射。**
**该类实现了Map接口，根据键的HashCode值存储数据，具有很快的访问速度，最多允许一条记录的键为null，不支持线程同步。**
- **TreeMap** 
**继承了AbstractMap，并且使用一颗树。**
- **WeakHashMap** 
**继承AbstractMap类，使用弱密钥的哈希表。**
- **LinkedHashMap** 
**继承于HashMap，使用元素的自然顺序对元素进行排序.**
- **IdentityHashMap** 
**继承AbstractMap类，比较文档时使用引用相等。**

#### java.util包中定义的类:
- **Vector** 
**该类和ArrayList非常相似，但是该类是同步的，可以用在多线程的情况，该类允许设置默认的增长长度，默认扩容方式为原来的2倍。**
- **Stack** 
**栈是Vector的一个子类，它实现了一个标准的后进先出的栈。**
- **Dictionary** 
**Dictionary 类是一个抽象类，用来存储键/值对，作用和Map类相似。**
- **Hashtable** 
**Hashtable 是 Dictionary(字典) 类的子类，位于 java.util 包中。**
- **Properties** 
**Properties 继承于 Hashtable，表示一个持久的属性集，属性列表中每个键及其对应值都是一个字符串。**
- **BitSet**
**一个Bitset类创建一种特殊类型的数组来保存位值。BitSet中数组大小会随需要增加。**

### 如何使用迭代器
- **通常情况下，你会希望遍历一个集合中的元素。例如，显示集合中的每个元素。
一般遍历数组都是采用for循环或者增强for，这两个方法也可以用在集合框架，但是还有一种方法是采用迭代器遍历集合框架，它是一个对象，实现了Iterator 接口或ListIterator接口。
迭代器，使你能够通过循环来得到或删除集合的元素。ListIterator 继承了Iterator，以允许双向遍历列表和修改元素。**

### 集合的遍历
- **Iterator iterator()**
**该方法可以获取一个用于遍历当前集合的迭代器实现类通过Iterator就可以遍历集合元素。**
- **java.util.Iterator接口**
**该接口规定了所有迭代器实现类遍历和的通用操作想法 而遍历集合遵循的步骤为：问，取，删，其中删除元素不是必要操作。 不同的集合是提供了一个Iterator的实现类，我们无需记住这些实现类的名字，以Iterator接口接收并调用方法遍历即可。**
```bash
public static void main(String[] args) {
		 Collection c = new ArrayList();
		 c.add("one");
		 c.add("#");
		 c.add("two");
		 c.add("#");
		 c.add("three");
		 c.add("#");
		 c.add("four");
		 c.add("#");
		 c.add("five");
		 System.out.println(c);
		 
		 Iterator it = c.iterator();
		 /*
		  * boolean hasNext()
		  * 通过迭代器判断集合是否含有元素可以遍历
		  */
		 while(it.hasNext()) {
			 /*
			  * E next()
			  * 获取集合下一个元素
			  */
			 String s = (String)it.next();
			 System.out.println(s);
			 if("#".equals(s)) {
				 /*
				  * 迭代器遍历集合有一个要求，遍历的过程中不能
				  * 通过集合的方法增删元素，否则会抛出异常
				  */
				 //c.remove(s);
				 /*
				  * 迭代器的remove方法不需要传参，
				  * 删除的就是本次遍历时next()得到的元素。
				  */
				 it.remove();
			 }
		 }
		 System.out.println(c);
	}
```

### 增强for 循环
- **For-Each循环也叫增强型的for循环，或者叫foreach循环。**
- **For-Each循环是JDK5.0的新特性（其他新特性比如泛型、自动装箱等）。**
- **For-Each循环的加入简化了集合的遍历。**
```java
for(type element : array){
		System.out.println(element);
}
```
- **For-Each循环的缺点：丢掉了索引信息。**
	- **当遍历集合或数组时，如果需要访问集合或数组的下标，那么最好使用旧式的方式来实现循环或遍历，而不要使用增强的for循环，因为它丢失了下标信息。**

### Java 泛型
- **Java 泛型是 JDK 5 中引入的一个新特性, 泛型提供了编译时类型安全检测机制，该机制允许程序员在编译时检测到非法的类型。泛型的本质是参数化类型，也就是说所操作的数据类型被指定为一个参数。**
- **Java泛型机制广泛的应用在集合框架中。所有的集合类型都带有泛型参数，这样在创建集合时可以指定放入集合中元素的类型。Java编译器可以根据此类型进行检查，这样可以减少代码在运行时出现错误的可能性。**

### 泛型在集合中的应用
- **ArrayList类的定义中，< E >中的E为泛型参数，在创建对象时可以将类型作为参数传递，此时，类定义所有的E将替换成传入的参数。**
```java
public class ArrayList<E>{
	... ... ...
	public boolean add(E e){...};
	public E get(int index){...};
}
ArrayList<String> list = new ArrayList<String>();
list.add("one");
//list.add(100);  Java编译器类型检查错误，此时add方法应传入的参数类型是String
```