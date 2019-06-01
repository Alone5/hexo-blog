---
title: Java-Date(一)
date: 2019-05-30 13:52:06
tags: github
---
## Java 日期时间
### Java日期涉及几大类
#### 1. java.util.Date
- Date类负责时间的表示，在计算机中，时间的表示是一个较大的概念，现有的系统基本都是利用从1970.1.1 00:00:00 到当前时间的毫秒数进行计时，这个时间称为epoch。在后文中如果没有明确说明，毫秒数就是指从1970年到对应时间的毫秒数。在Java 的Date类内部其实也是一个毫秒数，对外表现为一个Date对象。
##### Date类方法
- **Date()**
	- 分配一个 Date对象，并初始化它，以便它代表它被分配的时间，测量到最近的毫秒。
- **Date(long date)**
	- 分配一个 Date对象，并将其初始化为表示自称为“时代”的标准基准时间以后的指定毫秒数，即1970年1月1日00:00:00 GMT。
- **void  setTime(long date)**
	- 使用给定的毫秒时间值设置现有的 Date对象。
- **void  setTime(long time)**
	- 设置此 Date对象以表示1970年1月1日00:00:00 GMT后的 time毫秒的时间点。
- **Instant  toInstant()**
	- 此方法总是引发UnsupportedOperationException，因为SQL Date值没有时间组件，所以不应该使用。
- **LocalDate  toLocalDate()**
	- 将此 Date对象转换为 LocalDate
- **String  toString()**
	- 格式化日期转义格式yyyy-mm-dd。
- **static Date  valueOf(LocalDate date)**
	- 从一个LocalDate对象获取一个Date的实例，具有与给定的LocalDate相同的年，月和日的月值。
- **static Date  valueOf(String s)**
	- 将JDBC日期转义格式的字符串转换为 Date值。
- **boolean	after(Date when)**
	- 测试此日期是否在指定日期之后。
- **boolean	before(Date when)**
	- 测试此日期是否在指定日期之前。
- **Object	clone()**
	- 返回此对象的副本。
- **int	compareTo(Date anotherDate)**
	- 比较两个日期进行订购。
- **boolean	equals(Object obj)**
	- 比较两个日期来平等。
- **static Date	from(Instant instant)**
	- 从 Instant对象获取一个 Date的实例。
- **long	getTime()**
	- 返回自1970年1月1日以来，由此 Date对象表示的00:00:00 GMT的毫秒 数 。
- **int	hashCode()**
	- 返回此对象的哈希码值。

```java
	//常用几个方法使用
	public static void main(String[] args) {
		//当前系统时间
		Date date = new Date();
		System.out.println(date);
		/*
		 * 获取Date内部维护的毫秒
		 */
		long time = date.getTime();
		System.out.println(time);
		/*
		 * 设置一个毫秒值，使得Date表示该日期
		 */
		date.setTime(10);
		System.out.println(date);
		/*
		 * Date大部分方法被声明为过时的，新开发项目不要再使用这些方法。
		 */
		date.getYear();
	}
```

#### 2.java.util.Calendar（抽象类）
- Calendar类说是一个工具类，其实它比工具类还是更深一步的，它更像是一个加强版的Date类。一般的工具类会提供一堆static方法，工具类本身并不存储对象。但是Calendar类不是像一般的工具类只提供一堆static方法，而是在其内部本身就有一个毫秒数，所以它不是对外部Date对象操作，而是对内部的毫秒数进行转化，所以说Calendar本身就包含了日期时间信息，像一个装饰者模式。
- Calendar本身是一个抽象类，不能直接实例化，但是Calendar类提供一个工厂方法，即getInstance来创建一个Calendar实例，通过setTime()设定一个Calendar内部的毫秒数，之后就可以对这个毫秒数进行分析，进而得到它的年月日信息。同时，我们也可以对Calendar直接设定年月日属性，从而获取对应的Date对象。
- GregorianCalendar 是 Calendar 的一个具体子类，提供了世界上大多数国家使用的标准日历系统。结合Calendar抽象类使用。

##### Calendar类对象字段类型

| 常量                  | 描述                           |
| :-------------------- | :----------------------------- |
| Calendar.YEAR         | 年份                           |
| Calendar.MONTH        | 月份                           |
| Calendar.DATE         | 日期                           |
| Calendar.DAY_OF_MONTH | 日期，和上面的字段意义完全相同 |
| Calendar.HOUR         | 12小时制的小时                 |
| Calendar.HOUR_OF_DAY  | 24小时制的小时                 |
| Calendar.MINUTE       | 分钟                           |
| Calendar.SECOND       | 秒                             |
| Calendar.DAY_OF_WEEK  | 星期几                         |

- **Calendar中需要注意的：**
1. Calendar的星期是从周日开始的，常量值为0。
2. Calendar的月份是从一月开始的，常量值为0。
3. Calendar的每个月的第一天值为1。
- **常用方法示例:**
```java
	public static void main(String[] args) {
		/*
		 * static Calendar getInstance()
		 * 根据当前系统所在地区获取一个适用的实现类
		 * 通常返回的就是GregorianCalendar
		 */
		Calendar calendar = Calendar.getInstance();
		System.out.println(calendar);
		/*
		 * Date getTime()
		 * 将当前Calendar的表示的时间一个Date实例形式返回。
		 */
		Date date = calendar.getTime();
		System.out.println(date);
		/*
		 * void setTime()
		 * 调整当前calendar的时间为给定的Date所表示的时间。
		 */
		calendar.setTime(date);
		
	}
```
```java
package date;
import java.util.Calendar;
/**
 * int get(int field)
 * 该方法可以获取指定的时间分量所对应的值。
 * 时间分量为一个int值，无需记住每个值得含义，
 * Calendar提供了大量的常量与之对应。
 */
public class CalendarDemo2 {
	public static void main(String[] args) {
		Calendar c = Calendar.getInstance();
		/*
		 * 1：获取年
		 * 2：获取月
		 * 3：获取日
		 * 与"天"相关的时间分量
		 * DAY_OF_MONTH：月中的天(DATE)
		 * DAY_OF_WEEK：周中的天,星期几
		 * DAY_OF_YEAR：年中的天
		 * HOUR	12小时制的小时
		 * HOUR_OF_DAY	24小时制的小时
		 * MINUTE	分钟
		 * SECOND	秒
		 */
		int year = c.get(Calendar.YEAR);
		int month = c.get(Calendar.MONTH);
		int day = c.get(Calendar.DAY_OF_MONTH);
		int h = c.get(Calendar.HOUR_OF_DAY);
		int m = c.get(Calendar.MINUTE);
		int s = c.get(Calendar.SECOND);
		System.out.println(year + "-" + month + "-" + day);
		System.out.println(h + ":" + m + ":" + s);

		int days = c.get(Calendar.DAY_OF_YEAR);
		System.out.println("今年的第" + days + "天");
		days = c.get(Calendar.DAY_OF_WEEK);
		String[] data = { "日", "一", "二", "三", "四", "五", "六" };
		System.out.println("今天是周" + data[days - 1]);
		// 获取指定时间分量所允许的最大值
		days = c.getActualMaximum(Calendar.DAY_OF_YEAR);
		System.out.println("今年一共" + days + "天");
		days = c.getActualMaximum(Calendar.DAY_OF_MONTH);
		System.out.println("当月一共" + days + "天");
	}
}
```
```java
package date;
import java.util.Calendar;
/**
 * void set(int field , int value)
 * 调整指定的时间分量为指定的值
 */
public class CalendarDemo3 {
	public static void main(String[] args) {
		Calendar c = Calendar.getInstance();
		System.out.println(c.getTime());

		// 调整年
		c.set(Calendar.YEAR, 2000);
		System.out.println(c.getTime());
		// 调整月
		c.set(Calendar.MONTH, Calendar.DECEMBER);
		System.out.println(c.getTime());

		c.set(Calendar.HOUR_OF_DAY, 20);
		c.set(Calendar.MINUTE, 20);
		c.set(Calendar.SECOND, 20);
		System.out.println(c.getTime());

		c.set(Calendar.DAY_OF_WEEK, 4);
		System.out.println(c.getTime());
	}
}
```
``` java
package date;
import java.util.Calendar;
/**
 * void add(int field,int amount) 
 * 对指定的时间分量加上给定的值，若给定的值为负数则是减去。
 */
public class CalendarDemo4 {
	public static void main(String[] args) {
		Calendar c = Calendar.getInstance();
		/*
		 * 查看三年二月零25天以后所在周的周三是哪天？
		 */
		//加年
		c.add(Calendar.YEAR, 3);
		//加月
		c.add(Calendar.MONTH, 2);
		//加天
		c.add(Calendar.DAY_OF_YEAR, 25);
		System.out.println(c.getTime());
		c.set(Calendar.DAY_OF_WEEK, 4);
		System.out.println(c.getTime());
	}
}

```
