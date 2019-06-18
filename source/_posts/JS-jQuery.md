---
title: JS-jQuery
date: 2019-06-17 18:37:00
tags: github
---
## jQuery
- jQuery框架:是一个通过js语言写的框架,对原生js语言进行封装,可以上程序员写的更少但实现的更多
- 优点:
1. 可以向css的选择器一样获取页面中的元素
```bash
	js: var d = document.getElementById("id");    
	jq:	$("#id")
```
2. 可以批量给元素添加样式
```bash
	js:var arr = document.getElementsByTagName("div");
		for(var i=0;i<arr.length;i++){
			arr[i].style.color="red";
		}
	jq: $("div").css("color","red");
```
3. 可以解决部分浏览器的兼容性问题

#### 引入jQuery
- 和引入一个普通的js文件一样

#### JS对象和JQ对象之间的互相转换
1. js转jq
	- var jq = $(js);
2. jq转js
	- var js = jq[0];

#### 选择器
1. 基础选择器
	- 和css的用法一样
- 标签名选择器    $("div")
- id选择器   $("#id")
- class选择器  $(".class")
- 分组选择器   $("div,#id,.class")
- 任意元素选择器 $(" * ")
2. 层级选择器
	- $("div span") 匹配div里面所有的span
	- $("div>span") 匹配div里面所有的span子元素
	- $("div+span") 匹配div的弟弟元素span
	- $("div~span") 匹配div的弟弟们span 
- 层级方法:
	1. $("#abc").siblings()  获取id为abc元素的所有兄弟元素
		$("#abc").siblings("div") 获取id为abc元素的所有div兄弟元素
	2. .prev() 匹配元素的哥哥元素
	3. .prevAll() 匹配元素的哥哥们元素
	4. .next() 匹配元素的弟弟元素
	5. .nextAll() 匹配元素的弟弟们元素
3. 过滤选择器
	- $("div:first") 匹配所有div中的第一个
	- $("div:last") 匹配所有div中的最后一个
	- $("div:even") 匹配所有div中下标为偶数的元素
	- $("div:odd") 匹配所有div中下标为奇数的元素
	- $("div:lt(n)") 匹配所有div中下标小于n的元素 
	- $("div:gt(n)") 匹配所有div中下标大于n的元素 
	- $("div:eq(n)") 匹配所有div中下标等于n的元素 
	- $("div:not(.abc)") 匹配所有div中class值不等于abc的元素
4. 内容选择器
	- $("div:has(p)") 匹配所有包含p子元素的div
	- $("div:empty")  匹配所有空的div
	- $("div:parent") 匹配非空的div
	- $("div:contains('xxx')")  匹配包含xxx文本的div
5. 可见选择器
	- $("div:hidden") 匹配所有隐藏的div
	- $("div:visible") 匹配所有显示的div
	- **和隐藏显示相关的方法**
	1. $("#abc").show(); 显示
	2. $("#abc").hide(); 隐藏
	3. $("#abc").toggle(); 隐藏显示切换
6. 属性选择器
	- $("div[id]") 匹配有id属性的所有div
	- $("div[id='xxx']") 匹配id值为xxx的所有div
	- $("div[id!='xxx']") 匹配id值不为xxx的所有div
7. 子元素选择器
	- $("div:first-child") 是第一个子元素并且是div
	- $("div:last-child") 是最后一个子元素并且是div
	- $("div:nth-child(n)") 是第n个子元素并且是div
8. 表单选择器
	- $(":input") 匹配表单中的每一个控件
	- $(":password") 匹配表单中的所有密码框
	- $(":radio") 匹配所有单选 
	- $(":checkbox") 匹配所有的多选
	- $(":checked") 匹配所有选中的单选/多选和下拉选 
	- $("input:checked") 匹配所有选中的单选和多选
	- $(":selected") 匹配所有选中的下拉选 

#### 创建和添加元素
1. 创建元素
```bash
	js:var d = document.createElement("div");
	jq: var d = $("<div id='xxx'>xxxxx</div>");
```
2. 添加元素
	- js: 父元素.appendChild(新元素)
	- jq: 父元素.append(新元素); //添加到最后面
		父元素.prepend(新元素); //添加到最前面
3. 插入元素
	- js: 父元素.insertBefore(新元素,弟弟);
	- jq: 弟弟.before(新元素);
		哥哥.after(新元素);
4. 删除元素
	- js: 父元素.removeChild(被删除的元素);
	- jq: 被删除的元素.remove();

#### 获取和修改元素的文本内容
- 元素对象.text()  //获取
- 元素对象.text("xxx")  //修改

#### 获取和修改元素的html内容
- 元素对象.html()  //获取
- 元素对象.html(" < h1 >xxx< /h1 > ") //修改

#### 获取和修改元素的样式
- 元素对象.css("color"); //获取样式的值
- 元素对象.css("color","red"); //赋值
- 元素对象.css({"xxx":"xx","xxx":"xx"}) //批量赋值

#### 获取和修改元素的属性
- 元素对象.attr("属性名"); //获取
- 元素对象.attr("属性名","值"); //赋值

#### 获取元素的子元素们
- 元素对象.children() 

#### 获取元素的父元素
- 元素对象.parent();

#### jQuery事件相关
1. 常见事件
	- 鼠标事件: 点击事件 click 鼠标移入 mouseover 鼠标移出mouseout 鼠标按下mousedown 鼠标抬起mouseup 鼠标移动mousemove
	- 键盘事件: 键盘按下 keydown  键盘抬起 keyup
	- 状态改变事件: 页面加载完成 $(function(){xxxx}) 值改变change 表单提交submit  获取焦点focus  失去焦点blur 窗口尺寸改变resize
2. 事件模拟 
	- 元素.trigger("事件名称");
3. 鼠标移入移出事件合并
	- 元素.hover(function(){鼠标移入时执行},function(){鼠标移出执行});