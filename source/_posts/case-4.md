---
title: 练习案例-4
date: 2019-05-08 19:59:45
tags: github
---
## JAVA 数列求和
**1. 有数列为：9，99，999，...，9999999999。要求使用程序计算此数列的和，并在控制台输出结果。**
```java
public static void main(String[] args) {
		long num = 9;
		long result = num;
		for (int i = 2; i <= 10; i++) {
			num = num * 10 + 9;
			result = result+num;
		}
		System.out.println("num=" + result);
	}
```
**2. 另有数列：1+1/2+1/3…+1/n（n>=2）。要求使用交互的方式计算此数列的和：用户在控制台录入需要计算的整数 n 的值，程序计算此数列的和，并在控制台输出结果。**
```java
public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.println("请输入一个整数：");
		int n = scan.nextInt();
		if (n >= 2) {
			double result1 = 0;
			for (int i = 1; i <= n; i++) {
				result1 += 1.0 / i ;
			}
			System.out.println("result=" + result1);
		}
	}
```

## 数组案例
#### 1. 查询数组最小值，并将其放在第一位
**创建程序，实现查询数组中最小值的功能，并将最小值放入数组的第一位。需求为：创建一个长度为 10 的数组，数组内放置 10 个 0 到 99 之间（包含0，包含99）的随机整数作为数组元素，要求查询出数组中的最小值，并打印显示在界面上。然后，将查询到的数组最小值记载为数组的第一个元素，并打印赋值后的数组内容。**
```java
public static void main(String[] args) {	
		int[] arr = new int[10];
		for (int i = 0; i < arr.length; i++) {
			arr[i] = (int) (Math.random() * 100);
			System.out.println(arr[i]);
		}
		int min = arr[0];
		for (int i = 1; i < arr.length; i++) {
			if (arr[i] < min) {
				min = arr[i];
			}
		}
		System.out.println("最小值为:" + min);
		int[] as = new int[arr.length + 1];
		System.arraycopy(arr, 0, as, 1, arr.length);
		as[0] = min;
		for (int i = 0; i < as.length; i++) {
			System.out.println(as[i]);
		}
}
```
#### 2. 随机生成数组
**封装一个方法generateArray，该方法实现生成指定长度的int数组，该数组的元素为0到指定范围内的随机数，并将该数组返回。**
```java
public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.println("请输入指定长度：");
		int m = scan.nextInt();
		System.out.println("请输入指定范围：");
		int n = scan.nextInt();
		int[] arr = generateArray(m,n);
		for(int i=0;i<arr.length;i++) {
			System.out.print(arr[i]+" ");
		}
	}
public static int[] generateArray(int len,int max){
		int[] arr = new int[len];
		for(int i=0;i<arr.length;i++) {
			arr[i] = (int)(Math.random()*max);
		}
		return arr;
	}
```



