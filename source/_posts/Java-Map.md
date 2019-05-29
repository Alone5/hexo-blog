---
title: Java-Map
date: 2019-05-27 18:52:01
tags: github
---
## Map接口
- **Map接口中键和值一一映射. 可以通过键来获取值。**
	- **给定一个键和一个值，你可以将该值存储在一个Map对象. 之后，你可以通过键来访问对应的值。**
	- **当访问的值不存在的时候，方法就会抛出一个NoSuchElementException异常.**
	- **当对象的类型和Map里元素类型不兼容的时候，就会抛出一个 ClassCastException异常。**
	- **当在不允许使用Null对象的Map中使用Null对象，会抛出一个NullPointerException 异常。**
	- **当尝试修改一个只读的Map时，会抛出一个UnsupportedOperationException异常。**

### 方法
- **void	clear()**
	- **从该地图中删除所有的映射（可选操作）。**
- **default V	compute(K key, BiFunction< ? super K,? super V,? extends V> remappingFunction)**
	- **尝试计算指定键的映射及其当前映射的值（如果没有当前映射， null ）。**
- **default V	computeIfAbsent(K key, Function< ? super K,? extends V> mappingFunction)**
	- **如果指定的键尚未与值相关联（或映射到 null ），则尝试使用给定的映射函数计算其值，并将其输入到此映射中，除非 null 。**
- **default V	computeIfPresent(K key, BiFunction< ? super K,? super V,? extends V> remappingFunction)**
	- **如果指定的密钥的值存在且非空，则尝试计算给定密钥及其当前映射值的新映射。**
- **boolean	containsKey(Object key)**
	- **如果此映射包含指定键的映射，则返回 true 。**
- **boolean	containsValue(Object value)**
	- **如果此地图将一个或多个键映射到指定的值，则返回 true 。**
- **Set<Map.Entry<K,V>>	entrySet()**
	- **返回此地图中包含的映射的Set视图。**
- **boolean	equals(Object o)**
	- **将指定的对象与此映射进行比较以获得相等性。**
- **default void	forEach(BiConsumer< ? super K,? super V> action)**
	- **对此映射中的每个条目执行给定的操作，直到所有条目都被处理或操作引发异常。**
- **V	get(Object key)**
	- **返回到指定键所映射的值，或 null如果此映射包含该键的映射。**
- **default V	getOrDefault(Object key, V defaultValue)**
	- **返回到指定键所映射的值，或 defaultValue如果此映射包含该键的映射。**
- **int	hashCode()**
	- **返回此地图的哈希码值。**
- **boolean	isEmpty()**
	- **如果此地图不包含键值映射，则返回 true 。**
- **Set< K >	keySet()**
	- **返回此地图中包含的键的Set视图。**
- **default V	merge(K key, V value, BiFunction< ? super V,? super V,? extends V> remappingFunction)**
	- **如果指定的键尚未与值相关联或与null相关联，则将其与给定的非空值相关联。**
- **V	put(K key, V value)**
	- **将指定的值与该映射中的指定键相关联（可选操作）。**
- **void	putAll(Map< ? extends K,? extends V> m)**
	- **将指定地图的所有映射复制到此映射（可选操作）。**
- **default V	putIfAbsent(K key, V value)**
	- **如果指定的键尚未与某个值相关联（或映射到 null ）将其与给定值相关联并返回 null ，否则返回当前值。**
- **V	remove(Object key)**
	- **如果存在（从可选的操作），从该地图中删除一个键的映射。**
- **default boolean	remove(Object key, Object value)**
	- **仅当指定的密钥当前映射到指定的值时删除该条目。**
- **default V	replace(K key, V value)**
	- **只有当目标映射到某个值时，才能替换指定键的条目。**
- **default boolean	replace(K key, V oldValue, V newValue)**
	- **仅当当前映射到指定的值时，才能替换指定键的条目。**
- **default void	replaceAll(BiFunction< ? super K,? super V,? extends V> function)**
	- **将每个条目的值替换为对该条目调用给定函数的结果，直到所有条目都被处理或该函数抛出异常。**
- **int	size()**
	- **返回此地图中键值映射的数量。**
- **Collection< V >	values()**
	- **返回此地图中包含的值的Collection视图。**

### HashMap
- **hashMap是一个用于存储key-value键值对的集合，每一个键值对也被叫做Entry。这些个键值对分散存储在一个数组当中，这个数组就是HaspMap的主干。**
- **总体上看，Hashmap分为很多个数组，每一个数组里面存放着一个链表。**

#### hashCode方法
- **key的hashCode()方法的返回值对HashMap存储元素时起着很重要的作用。而hashCode()方法实际上是在Object中定义的。那么应当妥善重写该方法：**
	- **对于重写equals方法的对象，一般要妥善的重写继承自Object类的hashCode方法，Object提供的hashCode方法将返回该对象所在内存地址的整数形式。**
	- **重写hashCode方法时需注意两点：**
		- **1：与equals方法的一致性，即equals比较返回true的两个对象其hashCode方法返回值应该相同。**
		- **2：hashCode返回的数值应符合hash算法的要求，如果有很多对象的hashCode方法返回值都相同，则会大大降低hash表的效率。**
- **Capacity：容量，hash表里bucket(桶)的数量，也就是散列数组大小。**
- **Initial capacity：初始容量，创建hash表示，初始bucket的数量，默认构建容量是16，也可以使用特定容量。**
- **Size：大小，当前散列表中储存数据的数量。**
- **Load factor：加载因子，默认值0.75(75%)，当向散列表增加数据时如果size/capacity的值大于Load factor则发生扩充并且重新散列(rehash)。**
- **性能优化：加载银子较小时，散列查找性能会提高，同时也浪费了散列桶空间容量。0.75是性能和空间相对平衡结果。在创建散列表时指定合理容量，减少rehash提高性能。**






















