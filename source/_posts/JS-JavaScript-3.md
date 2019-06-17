---
title: JS-JavaScript-3
date: 2019-06-14 18:02:06
tags: github
---
## JS - 3
#### 事件相关
- onclick  
- onmouseover  
- onmouseout  
- onload  
- onfocus  
- onblur  
- onsubmit  
- onchange  
- onresize
- 事件:事件就是一些特定的时间点 ,事件分为鼠标事件 键盘事件 状态改变事件

#### 事件分类
- 鼠标事件: 
	- 点击onclick 
	- 鼠标移入onmouseover 
	- 鼠标移出onmouseout 
	- 鼠标按下onmousedown 
	- 鼠标抬起onmouseup 
	- 鼠标移动onmousemove
- 键盘事件: 
	- 键盘按下事件onkeydown  
	- 键盘抬起事件onkeyup
- 状态改变事件: 
	- 页面加载完成onload  
	- 表单提交onsubmit 
	- 获取焦点onfocus 
	- 失去焦点onblur 
	- 窗口尺寸改变onresize 
	- 值改变onchange

#### event事件对象
- 该对象保存着事件相关的信息
1. 鼠标事件中保存着鼠标的坐标 
```bash
	event.clientX/clientY
```
2. 键盘事件中保存着按键的编码
```bash
	event.keyCode  String.fromCharCode(65)
```
3. 在任何事件中都可以获取事件源对象 
```bash
	var obj = event.target || event.srcElement;
```

#### 事件的绑定
1. 在标签内部添加事件属性
	- this代表window对象, 没啥用
2. 通过js代码给标签对象添加事件(动态绑定)
	- this代表事件源 

#### 事件的传递(事件冒泡儿)
- 如果在某一个范围有多个事件需要响应,响应顺序从底层元素往上级元素传递(类似于气泡从下往上,所以称为事件冒泡儿)