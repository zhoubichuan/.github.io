---
title: 3d旋转色子
date: 2014-12-23 21:13:58
categories:
- 前端开发
- 标准
- W3C
- CSS
- css3
tags:
- css3
- 3d旋转色子
---

## 代码
```
<!doctype html>
<html lang="en">
<head>
	<meta charset="UTF-8" />
	<title>Document</title>
	<style>
		li{list-style: none; width: 100px;height: 100px;line-height: 100px;text-align: center;font-size:80px ;color: white;position: absolute;}
		body{display: -webkit-flex;justify-content: center;}
		#box0{width: 200px;height: 200px;transform-origin:50% 50% 50px ;position: relative;
		transform-style:preserve-3d ;animation:rote  5s ease-in infinite normal ;}
		#box1{top:-50px ;left:50px ;transform-origin:bottom; transform: rotateX(-90deg);}
		#box2{top:50px ;left:150px ;transform-origin:left;transform: rotateY(-90deg);}
		#box3{top:50px ;left:50px ;}
		#box4{top:50px ;left:50px ;transform-origin:right ;transform: translateZ(100px);}
		#box5{left: -50px;top: 50px;transform-origin:right ;transform: rotateY(90deg);}
		#box6{top:150px ;left:50px ;transform-origin:top;transform: rotateX(90deg);}
		@-webkit-keyframes rote{
			0%{transform: rotateX(0deg) rotateY(0deg);}
			100%{transform: rotateX(360deg) rotateY(360deg);}
		}
	</style>
</head>
<body>
	<ul id="box0">
	<li id="box1">1</li>
	<li id="box2">2</li>
	<li id="box3">3</li>
	<li id="box4">4</li>
	<li id="box5">5</li>
	<li id="box6">6</li>	
	</ul>
</body>
<script>
	var li=document.getElementsByTagName('li');
	var a=['red','blue','yellow','orange','pink','black']
		for(var i=0;i<li.length;i++){
			li[i].style.background=a[i];
		}
</script>
</html>
```