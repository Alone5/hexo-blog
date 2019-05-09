---
title: 练习案例-3
date: 2019-05-07 20:33:40
tags: github
---
### **1.编写个人所得税计算程序**
**个人所得税是国家对本国公民、居住在本国境内的个人的所得和境外个人来源于本国的所得征收的一种所得税。目前，北京地区的个人所得税的计算公式为：应纳税额＝（工资薪金所得－扣除数）×适用税率－速算扣除数。其中，扣除数为3500元，适用税率以及速算扣除数如下表所示。**

|        全月应纳税所得额      |    税率|	 速算扣除数(元)|
|----------------------------|-------| --------|
|       全月应纳税额不超过1500元|	  3%|	       0|
|  全月应纳税额超过1500元至4500元|	  10%|	    105|
|  全月应纳税额超过4500元至9000元|   20%|	    555|
| 全月应纳税额超过9000元至35000元|	  25%|	   1005|
|全月应纳税额超过35000元至55000元|	  30%|	   2755|
|全月应纳税额超过55000元至80000元|	  35%|	   5505|
|        全月应纳税额超过80000元|    45%|	13505|
**上表中的全月应纳税所得额=工资薪金所得－扣除数。**
**本案例要求计算个人所得税的缴纳额度：用户从控制台输入税前工资的金额，程序计算所需要交纳的个人所得税的金额，并将计算结果输出到控制台。**
```bash
public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.println("请输入您的工资税前金额(￥):");
		double wage = scan.nextDouble();
		wage -= 3500;
		double num = 0;
		if (wage <= 0) {
			num = 0;// 不纳税
		} else if (wage <= 1500) {
			num = wage * 0.03;
		} else if (wage <= 4500) {
			num = wage * 0.1 - 105;
		} else if (wage <= 9000) {
			num = wage * 0.2 - 555;
		} else if (wage <= 35000) {
			num = wage * 0.25 - 1005;
		} else if (wage <= 55000) {
			num = wage * 0.3 - 2755;
		} else if (wage <= 80000) {
			num = wage * 0.35 - 5505;
		} else if (wage >= 80000) {
			num = wage * 0.45 - 13505;
		}
		System.out.println("您应该缴纳的个人所得税是(￥):" + num);

	}
```
### 2.输入年份和月份，输出该月的天数（使用switch-case）
**一年有 12 个月，而每个月的天数是不一样的。其中，有7个月为 31 天，称为大月，分别为1、3、5、7、8、10、12月；有 4个月为 30 天，称为小月，分别为4、6、9、11月；还有二月比较特殊，平年的二月只有28天，而闰年的二月有 29 天。**
**本案例需要使用交互的方式计算某年某月的天数：由用户在控制台输入年份和月份值，程序计算该年该月的天数，并将结果输出在控制台。**
```bash
public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.println("请输入年份：");
		int year = scan.nextInt();
		System.out.println("请输入月份：");
		int month = scan.nextInt();
		int day = 0;
		switch (month) {
		case 1:
		case 3:
		case 5:
		case 7:
		case 8:
		case 10:
		case 12:
			day = 31;
			break;
		case 4:
		case 6:
		case 9:
		case 11:
			day = 30;
			break;
		case 2:
			if (year % 4 == 0 && year % 100 != 0 || year % 400 == 0) {
				System.out.println(year + "是闰年");
				day = 29;
			} else {
				System.out.println(year + "是平年");
				day = 28;
			}
			break;
		}
		System.out.println(year + "年" + month + "月" + "有" + day + "天");

	}
```


