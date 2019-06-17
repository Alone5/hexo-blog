---
title: JS-JavaScript-case
date: 2019-06-13 19:42:47
tags: github
---
## JS - case
#### 轮播图步骤:
1. 搭建页面，通过css给图片和div设置相同的宽高
2. 修改图片的定位为绝对定位，并且在上级div里添加相对定位
3. 在页面加载完成的时候得到所有的图片数组，遍历数组，让数组里面图片的left值为”i*200“
4. 创建1秒执行100次的定时器 在定时器里面获取所有图片数组，遍历数组，
得到当前遍历图片left值 让其-=2 把新的值再次给到图片
5. 在移动图片时 如果left值<=-200时 让left值=400
6. 让上级div超出范围不显示
7. 让定时器通过一个moveTimer记录 ，在判断left值<=-200时让定时器停止 
8. 把移动一张 图片定时器的所有代码封装到一个方法中，再开启另外一个定时器，每隔2秒调用次方法
9. 给div添加鼠标悬停事件悬停时调用stop方法 方法中停止2秒执行的定时器
10. div添加鼠标移出事件 调用start方法 方法中再次开启2秒执行的定时器。
11. 当页面失去焦点时调用 stop方法 当页面获取焦点时调用 start方法 
13. 在start方法中调用stop方法 为了避免重复开启定时器  

```bash
<!doctype html>
<html>
<head>
<meta charset="utf-8">
<title>无标题文档</title>
    <style type="text/css">
        div,img{
            width: 200px;
            height: 150px;
            position: relative;
        }
        img{
            position: absolute;
        }
        div{
            overflow: hidden;
        }
    </style>
    <script type="text/javascript">
        onload = function(){
            var arr = document.getElementsByTagName("img");
            for(var i =0; i<arr.length;i++){
                arr[i].style.left = i*200+"px";
            }
        }
        var timer = setInterval(change,2000);
        function change(){
            var moveTimer = setInterval(function(){
            var arr = document.getElementsByTagName("img");
            for(var i =0; i<arr.length; i++){
                var oldLeft = parseInt(arr[i].style.left);
                oldLeft -= 2;
                if(oldLeft<=-200){
                   oldLeft = 400;
                    clearInterval(moveTimer);
                   }
                arr[i].style.left = oldLeft + "px";
            }
            },10);
        }
        function stop(){
            clearInterval(timer);
        }
        function start(){
            stop();
            timer = setInterval(change,2000);
        }
        onblur = function(){
            stop();
        }
        onfocus = function(){
            start();
        }
    </script>
</head>
<body>
    <div onmouseover="stop()" onmouseout="start()">
        <img alt="" src="../imgs/1.jpg">
        <img alt="" src="../imgs/a.jpg">
        <img alt="" src="../imgs/k.jpg">
    </div>
</body>
</html>
```