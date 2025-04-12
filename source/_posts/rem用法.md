---
title: rem用法
date: 2021-02-26 16:57:49
top_img: https://static001.geekbang.org/resource/image/4b/8b/4b2db13a5c87e4438146baa0cf0c688b.jpg
cover: https://static001.geekbang.org/resource/image/4b/8b/4b2db13a5c87e4438146baa0cf0c688b.jpg
tags:
- 移动端
categories:
- 移动端
aplayer: true
---
## rem用法

#### **什么是rem**

rem是CSS3中新增的单位值 **r:root em:(font size of element)相对单位**，先后对于html的字体大小单位，可以用于任何设定长度的单位。

#### **rem用法**

css中的body中先全局声明font-size=62.5%，这里的%的算法和rem一样。
因为100%=16px，1px=6.25%，所以10px=62.5%，
这是的1rem=10px，所以12px=1.2rem。px与rem的转换通过10就可以得来。
注意，rem是只相对于根元素htm的font-size，即只需要设置根元素的font-size，其它元素使用rem单位设置成相应的百分比即可。

#### **例子**

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<style type="text/css">
			*{
				margin: 0;
				padding: 0;
			}
			#d1{
				width: 5rem;
				height: 5rem;
				background-color: #00FFFF;
			}
		</style>
	</head>
	<body>
		<div id="d1">
			hello
		</div>
		<!-- 
		 1.设计师网站的设计稿：1000px
		 2.每个人打开网页的时候因为设备不同，或者浏览器设定的分辨率不同，使得需要在不同分辨率下打开
		 3.屏幕大小，1000px，1rem == 100px(设计稿的宽度)，10rem就会刚好占满整个屏幕的宽度
		 4.屏幕大小，500px,1rem == 50px,10rem就刚好占满屏幕宽度
		 -->
		 <script type="text/javascript">
		 	
			
			function setRem(){
				var  screenWidth = window.innerWidth;//获取屏幕宽度
				var unit = screenWidth/10;//屏幕的宽度/设计稿占满全屏的宽度为多少rem
				var html = document.querySelector("html")
				html.style.fontSize= unit + "px";
			}
			setRem();
			window.onresize = function(){
				setRem();
			}
		 </script>
	</body>
</html>

```