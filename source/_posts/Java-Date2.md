---
title: Java-Date2
date: 2019-05-31 10:59:37
tags: github
---
### java.text.SimpleDateFormat（DateFormat的直接子类）
- 这个SimpleDateFormat类是我们日常转换字符串和Date对象最常用的类。
- 我们如果用DateFormat进行转换格式，只能用它规定的几种格式进行转换，非常的不直观，从字面我们看不出来从Date转出来的日期是怎样的格式。
- 但是如果用SimpleDateFormat就不一样了，在构造SimpleDateFormat我们可以直接传一个pattern，只要pattern符合要求且是合理的，就可以对Date和字符串进行转换了。
- 日期和时间格式由日期和时间模式 字符串指定。在日期和时间模式字符串中，未加引号的字母 'A' 到 'Z' 和 'a' 到 'z' 被解释为模式字母，用来表示日期或时间字符串元素。文本可以使用单引号 (') 引起来，以免进行解释。'' 表示单引号。所有其他字符均不解释；只是在格式化时将它们简单复制到输出字符串，或者在分析时与输入字符串进行匹配。

- 定义了以下模式字母（所有其他字符 'A' 到 'Z' 和 'a' 到 'z' 都被保留）：
	- G 年 代标志符
	- y 年
	- M 月
	- d 日
	- h 时 在上午或下午 (1~12)
	- H 时 在一天中 (0~23)
	- m 分
	- s 秒
	- S 毫秒
	- E 星期
	- D 一年中的第几天
	- F 一月中第几个星期几
	- w 一年中第几个星期
	- W 一月中第几个星期
	- a 上午 / 下午 标记符 
	- k 时 在一天中 (1~24)
	- K 时 在上午或下午 (0~11)
	- z 时区

- SimpleDateFormat类方法使用示例：
```java
package date;
import java.text.SimpleDateFormat;
import java.util.Date;
/**
 * java.text.SimpleDateFormat
 * 可以按照指定的日期格式在Date与String之间相互转换
 */
public class SimpleDateFormatDemo {
	public static void main(String[] args) {
		Date now = new Date();
		System.out.println(now);
		/*
		 * 2019-05-29 10:26:24
		 * yyyy-MM-dd HH:mm:ss
		 */
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		/*
		 * String format(Date date)
		 * 将给定的日期按照SDF指定的日期格式转换为
		 * 一个字符串并返回。 
		 */
		String str = sdf.format(now);
		System.out.println(str);
	}
}
```
```java
package date;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
/**
 * 将一个字符串解析为Date
 */
public class SimpleDateFormatDemo2 {
	public static void main(String[] args) throws ParseException {
		String line = "2008-12-07 00:00:00";
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
		/*
		 * Date parse(String str)
		 * 将给定的字符串按照SDF指定的日期格式解析为一个Date对象
		 */
		Date date = sdf.parse(line);
		System.out.println(date);
	}
}
```
