---
title: JS-JavaScript-1
date: 2019-06-12 18:38:54
tags: github
---
## JS - 1
1. JavaScript  给页面添加动态效果和动态内容
2. 三种引入方式
	- 内联:在标签的事件属性中添加js代码
	- 内部:在页面的任意位置添加script标签 
	- 外部:在单独的js文件中写js代码 通过script标签的src属性引入
3. 数据类型
- js中只有对象类型
- 常见的对象类型有:
	1. 数值 number
	2. 字符串 string
	3. 布尔值 boolean
	4. 未定义 undefined
	5. 自定义 object
4. 变量声明和赋值
- js属于弱类型语言 
```bash
var x = 10;
	x = "abc";
```
5. 常见数据类型间的隐式转换
	- 字符串: 转数值能转直接转 不能则转成NaN ,转布尔值 ""转false 其它true
	- 数值:转字符串直接转  转布尔值 0和NaN转false 其它true
	- 布尔值:转字符串直接转 转数值  true->1 false->0
	- undefined: 转数值NaN 转布尔值false    
	- null:转数值0    转布尔值false
6. 运算符
	- 和Java大体相同 
	- ==:先统一类型再比较值和===:先比较类型 如果相等再比较值 
	- 除法运算会自动转换整数和小数
	- typeof x 获取变量的类型  typeof 66+6   =  "number6";
7. 语句
	- 和java大体相同
	- if和while括号里面的内容 如果不是布尔值会自动隐式转成布尔值
	- for循环把int i改成var i 不支持新for循环
8. 方法声明
	- function 方法名(参数列表){方法体}
	- var 方法名 = function(参数列表){方法体}
	- var 方法名 = new Function("参数1","参数2","方法体");
9. 和页面相关的方法
- 通过id获取页面标签
	 var d = document.getElementById("id")
- 获取和修改标签的文本内容
	d.innerText = "xxx";
	d.innerText;
- 获取和修改文本框的文本内容
	input.value="xxx";
	input.value
- 获取和修改标签的html内容
	d.innerHTML +="<xx>asdfa</xx>";
	d.innerHTML;
10. 字符串相关
- 创建字符串 
	var s1 = "xxx"/'xxx';
	var s2 = new String();
- 转数值
	parseFloat()
	parseInt()
	Number()
- 获取字符串出现的位置
	indexOf()
	lastIndexOf()
- 转大小写
	toUpperCase()
	toLowerCase()
- 替换
	replace(old,new) 只能替换第一个
- 截取字符串
	substring(start,?end);
- 拆分字符串
	split()  返回值是数组

11. 数学相关
- 随机数  Math.random()
- 四舍五入  num.toFixed(n);   n代表保留几位小数
12. 数组相关
- 创建数组
	var arr1 = ["xxx",23,false];
	var arr2 = new Array();
- 添加元素
	arr.push(xxx);
- 数组长度
	arr.length
- 数组遍历和java一样
- 数组反转 arr.reverse() 
- 数组排序 arr.sort()  默认是通过Unicode编码依次排序
	自定义排序方法 	
		function mysort(a,b){
			return a-b;//升序   b-a降序
		}
13. 日期相关
- 获取当前客户端时间
	var d = new Date();
	var d = new Date("2019/6/11 17:37:00");
- 获取和设置时间戳
	d.getTime();
	d.setTime(1000);
- 获取时间分量
	d.getFullYear/Month/Date/Day/Hours/Minutes/Seconds();
- 获取年月日和时分秒
	d.toLocaleDateString();
	d.toLocaleTimeString();
	
14. 正则
- 创建正则对象
	var reg1 = /规则/模式;
	var reg2 = new RegExp("规则",?"模式");  \需要转义
- 相关方法
	1. 查找 
		reg.exec(str)  
	2. 校验
		reg.test(str)
	3. 字符串查找
		str.match(reg)   返回数组装着所有找到的内容
	4. 替换
		str.replace(reg,new) 

15. 修改元素的class
	元素对象.className="xxx";
	
#### 隐藏页面标签的两种方式
1. display:none 元素隐藏并脱离文档流, 改回原来的值则显示
2. visibility:hidden(隐藏)/visible(显示)  隐藏元素不脱离文档流 

#### 自定义对象(了解)
```bash
第一种方式:
		function Person(name,age){
			//给对象添加属性  
			this.name=name;
			this.age=age;
			//给对象添加方法 
			this.run = function(){
				alert(this.name+":"+this.age);
			}
		} */
		//实例化对象
		//var p = new Person("关羽",42);
		//p.run();
		
		function Person(name,age){
			//给对象添加属性  
			this.name=name;
			this.age=age;
			//给对象添加方法 
			this.run = function(){
				alert(this.name+":"+this.age);
			}
		} */
		//实例化对象
		//var p = new Person("关羽",42);
		//p.run();
第二种:
		var p2 = {
				/* 以键值对的形式添加属性和方法 */
				"name":"刘备",
				"age":43,
				"run":function(){
					alert(this.name+":"+this.age);
				}
		}
		p2.run();
```

#### DHTML
- 简介: Dynamic(动态)HTML=html+css+js
- 对象分类:
1. JavaScript内置对象:包括 string number boolean array
2. BOM:Browser(浏览器) Object Modle,包含和浏览器相关的各种对象
3. DOM:Document(文档) Object Model,包含和页面相关的各种对象

#### BOM
- window对象, 该对象里面属性和方法称为全局属性和全局方法,访问时可以省略window.
- window对象中常见的方法:
	1. parseInt/Flaot()
	2. alert()  //弹出提示框
	3. confirm() //弹出确认框
	4. prompt() //弹出文本框
	5. isNaN()  判断是否是数
- window中常见的属性
	1. location  位置
		获得和修改浏览器的访问地址: 
```bash
		location.href //获取
		location.href="http://tmooc.cn" //修改
		location.reload(); 重新加载(刷新)
```
	2. history  历史  当前窗口的历史页面
		history.length   页面数量
		history.back()  返回上一页面
		history.forward() 前往下一页面
		history.go(n)  n正值则前进 负值后退  0刷新
	3. screen 屏幕  
		screen.width/height  获得屏幕的分辨率
		screen.availWidth/availHeight  获取屏幕可用分辨率 
	4. navigator 导航/帮助
		navigator.userAgent 获取浏览器的版本信息
- window中相关的事件
	1. onclick 窗口点击事件
	2. onload 页面加载完成事件   ****
	3. onfocus 页面获取焦点事件
	4. onblur 页面失去焦点事件

#### eval()方法
- 可以将字符串以js代码的形式执行  
"2+3*5-5"

#### 通过js代码给标签添加css样式

#### 通过标签名获取元素们
```bash
	var arr = document.getElementsByTagName("标签名");
```

#### 定时器
- 开启   var timer = setInterval(方法,时间间隔);
- 停止   clearInterval(timer);

#### 只执行一次的定时器
- setTimeout(方法,时间间隔);

#### DOM Document Object Model文档对象模型
- 和页面相关的内容

#### 查找页面元素的方法
1. 通过id查找
	- document.getElementById()
2. 通过标签名查找
	- document.getElementsByTagName()
3. 通过标签的name属性查找
	- document.getElementsByName()
4. 通过标签的class属性查找 
	- document.getElementsByClassName();
5. 获取页面的body元素
	- document.body

#### 创建新元素
- var d = document.createElement("div");
	- d.innerText="abc";

#### 添加到页面
-  父元素.appendChild(新元素);
	- document.body.appendChild(d);

#### 插入元素
-  父元素.insertBefore(新元素,弟弟元素);

#### 删除元素
- 父元素.removeChild(被删除的元素);