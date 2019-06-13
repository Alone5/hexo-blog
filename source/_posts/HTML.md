---
title: HTML
date: 2019-06-10 18:39:51
tags: github
---
## **HTML**
1. **内容标题标签  h1-h6    独占一行  有上下外边距 字体加粗  align=left/right/center**
2. **段落标签 p     独占一行  有上下外边距** 
3. **水平分割线  hr**
4. **换行 br**
5. **列表标签**
	- **无序列表  ul : type    li**
	- **有序列表  ol : type start  reversed   li**
	- **定义列表  dl  dt  dd**
	- **列表嵌套  有序和无序可以任意无限嵌套**
6. **分区元素**
	- **div：独占一行 和 span：共占一行** 
	- **html5 新增  header article footer**
7. **元素分类**
	- **块级元素： 独占一行   包括：div h1-h6 hr  p** 
	- **行内元素： 共占一行   包括：span strong和b em和i del和s  u**
8. **实体引用**
	- **空格  &nbsp;**
	- **小于号   &lt;**
	- **大于号   &gt;**
9. **图片标签**
	- **img: src路径 相对 访问站内资源和绝对 访问站外资源  alt  title  width、height：1.像素 2.百分比** 
10. **图片地图**
	- **map：name**
	- **子元素 area:shape形状 coords坐标  href：资源路径**
11.  **超链接a**
	- **如果不写href属性则就是纯文本**
	- **文本超链接和图片超链接**
	- **target="_blank"  从新的窗口显示页面** 
	- **页面内部跳转   如果目的地有标签直接给标签添加id  如果没标签添加猫(空的a标签)**  
12. **表格table**
	- **子标签：tr  td  th  caption  thead  tbody  tfoot**
	- **属性： table:border  cellspacing  cellpadding   td:rowspan colspan**
13. **表单form**
	- **文本框  type=text  name value  readonly placeholder maxlength** 
	- **密码框 type=password name value  readonly placeholder maxlength** 
	- **单选 type=radio  name value id checked     lable标签**
	- **多选 type=checkbox name value id checked     lable标签**
	- **日期 type=date  name** 
	- **文件 type=file name** 
	- **隐藏域 type=hidden  name value**
	- **下拉选  select:name**
	- **子标签  option:value selected**
	- **文本域  textarea:name rows  cols  placeholder**
	- **提交按钮 type=submit  value**
	- **重置按钮 type=reset   value**
	- **自定义按钮  type=button value**  