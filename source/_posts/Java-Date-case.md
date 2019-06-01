---
title: Java-Date-case
date: 2019-06-01 11:17:39
tags: github
---
## 两个日期常见案例
- **程序启动要求用户输入自己的生日，格式：yyyy-MM-dd HH:mm:ss**
- **然后经过程序计算输出到今天位置一共活了多少天**
- **再经过计算输出其出生10000天的纪念日为哪天格式同上。**
```java
package date;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;
import java.util.Scanner;

public class Test {
	public static void main(String[] args) throws ParseException {
		System.out.println("请输入你的生日：（如0000-00-00）");
		Scanner scan  = new Scanner(System.in);
		String line = scan.nextLine();
		
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
		Date birth = sdf.parse(line);
		
		Date now = new Date();
		long time = now.getTime()-birth.getTime();
		time = time/1000/60/60/24;
		System.out.println("到今天为止一共活了"+time+"天");
		
//		//1:第一种方法
//		time = 10000-time;
//		System.out.println("距离活够10000天还有"+time+"天");
//		time = time*1000*60*60*24;
//		now.setTime(now.getTime()+time);
//		System.out.println("生存10000天的纪念日为："+sdf.format(now));
		
		//2:第二种方法
		time = birth.getTime()+1000L*60*60*24*10000;
		Date date = new Date(time);
		String str = sdf.format(date);
		System.out.println("10000天的纪念日为："+str);
	}
}
```

- **编写程序计算商品促销日期**
```java
package date;
import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Calendar;
import java.util.Date;
import java.util.Scanner;
/**
 * 程序启动要求输入商品的生产日期：yyyy-MM-dd
 * 然后再输入保质期的天数。
 * 经过程序计算输出该商品促销日期，格式同上。
 * 
 * 促销日期计算规则：商品过期日前2周的周3
 */
public class Test2 {
	public static void main(String[] args) throws ParseException {
		Scanner scan = new Scanner(System.in);
		System.out.println("请输入商品生产日期：（如2019-12-01）");
		String data = scan.nextLine();
		System.out.println("请输入商品保质期：单位是(天)");
		int day = scan.nextInt();
		
		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
		Date date = sdf.parse(data);
		Calendar c = Calendar.getInstance();
		c.setTime(date);
		c.add(Calendar.DAY_OF_YEAR, day);
		c.add(Calendar.DAY_OF_YEAR, -14);
		c.set(Calendar.DAY_OF_WEEK, Calendar.WEDNESDAY);
		date = c.getTime();
		String input = sdf.format(date);
		System.out.println("促销日期为:" + input);
	}
}
```