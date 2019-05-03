---
title: 练习案例-2
date: 2019-05-03 18:26:21
tags: github
---
### 1.输出两个int数中的最大值
**用户从控制台接收两个整数，通过程序找出两个数中的最大值。**
```bash
public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.println("请输入依次输入两个整数a，b 用空格符 空开");
		int a = scan.nextInt();
		int b = scan.nextInt();
		int max = a>b?a:b;
		System.out.println(max);
		if(a>b) {
			System.out.println("最大值为："+a);
		}else {
			System.out.println("最大值为："+b);
		}

	}
```
### 2.编写三个数值的排序程序
**使用程序为用户所录入的 3 个数值进行升序排列，并将排序后的结果输出到控制台。**
```bash
public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.println("请输入依次输入三个整数a，b，c 用空格符 空开");
		int a = scan.nextInt();
		int b = scan.nextInt();
		int c = scan.nextInt();
		System.out.println("您输入的是：");
		System.out.println("a="+a+",b="+b+",c="+c);
		if(a>b) {
			int d=a;
			a=b;
			b=d;
		}
		if(a>c) {
			int d=a;
			a=c;
			c=d;
		}
		if(b>c) {
			int d=b;
			b=c;
			c=d;
		}
		System.out.println("升序排序后结果为：");
		System.out.println("a="+a);
		System.out.println("b="+b);
		System.out.println("c="+c);
}
```
### 3.编写程序判断某一个年份是否为闰年（使用if-else）
**本案例需要使用交互的方式判断某年是否为闰年：用户从控制台输入需要判断的年份值，由程序使用if-else判断该年是否为闰年，并将判断结果输出到控制台。**
```bash
public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);
		System.out.println("请输入年份：");
		int year = scan.nextInt();
		if(year%4==0 && year%100!=0 || year%400==0) {
			System.out.println(year+"是闰年");
		}else {
			System.out.println(year+"是平年");
		}
}
```
### 4.输出三个int数中的最大值
**用户从控制台接收三个整数，通过程序找出三个数中的最大值。**
```bash
public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.println("请输入依次输入三个整数a，b，c 用空格符 空开");
		int a = scan.nextInt();
		int b = scan.nextInt();
		int c = scan.nextInt();
		if(a>b && a>c) {
			System.out.println("最大值为a="+a);
		}else if(b>a && b>c) {
			System.out.println("最大值为b="+b);
		}else {
			System.out.println("最大值为c="+c);
		}

}
```