---
title: 练习案例-1
date: 2019-04-26 15:09:17
tags: github
---
#### 编写一个收银柜台收款程序。根据商品单价、购买数量以及收款金额计算并输出应收金额和找零
```bash
package day03;
import java.util.Scanner;
public class Cashier {
	public static void main(String[] args) {
		Scanner scan = new Scanner(System.in);
		System.out.println("请输入商品单价：");
		double price = scan.nextDouble();
		System.out.println(price);

		System.out.println("请输入购买的数量：");
		int number = scan.nextInt();
		System.out.println(number);

		System.out.println("实际支付为：");
		double money = scan.nextDouble();
		System.out.println(money);

		double totalPrice = price * number;
		System.out.println("总价为：" + totalPrice);
		if (totalPrice >= 500) {
			totalPrice *= 0.8;
			System.out.println("打折后：" + totalPrice);
		}
		if (money >= totalPrice) {
			double change = money - totalPrice;
			System.out.println("最后价格为：" + totalPrice + "找零：" + change);
		} else {
			System.out.println("ERROR!您给的钱不够，" + "需要再付款：" + (totalPrice - money));
		}
	}
}
```

