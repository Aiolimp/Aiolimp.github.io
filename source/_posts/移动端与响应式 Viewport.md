---
title: 移动端与响应式 Viewport
date: 2021-03-15 16:57:49
top_img: https://static001.geekbang.org/resource/image/73/e4/730ea9c393def7975deceb48b3eb6fe4.jpg
cover: https://static001.geekbang.org/resource/image/73/e4/730ea9c393def7975deceb48b3eb6fe4.jpg
tags:
- 移动端
categories:
- 移动端
aplayer: true
---
## 移动端与响应式 Viewport

**1.什么是Viewport？**
Viewport是用户网页的可视区域。
手机浏览器是把页面放在一个虚拟的窗口(Viewport)中，通常这个窗口比屏幕宽，这样就不用把每个网页挤到很小的窗口中,用户可以通过平移或者缩放来看网页的不同部分。
**2.设置Viewport**
常用的Viewport meta标签大致如下：(**此标签仅适用于移动端,pc端无效**)

```javascript
<meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no" />
```

其中：

 -  width=device-width,//宽度等于设备宽度,浏览器宽度分辨率等于系统分辨率
-  initial-scale=1,//初始化的比例是1
-  minimum-scale=1,//允许最小缩放比例为1
-  maximum-scale=1,//允许最大放大比例为1
-  user-scalable=no"//用户不允许缩放

**3.手机屏幕分辨率：物理分辨率和系统分辨率**
物理分辨率：硬件所支持的，手机显示屏实际存在的分辨率。
系统分辨率：软件可以达到的，我们肉眼感知的实际尺寸
某些手机机型物理分辨率和系统分辨率参照表：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200826100357846.png#pic_center)
**4.媒体查询**

- 并不是所有页面都需要移动端和pc端响应，页面比较复杂时，代码就会比较凌乱和繁多，而且不能更加针对性的PC和移动端页面。
- 使用媒体查询必须加上 Viewport meta标签！

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<meta name="viewport" content="width=750px,user-scalable=no" />
		<title></title>
		<style type="text/css">
			.d1{
				width: 20%;
				height: 200px;
				background: orange;
			}
			/* 
			 媒体查询
			 设定像素小于600像素后的样式
			 */
			@media only screen and (max-width:600px) {
				.d1{
					width: 100%;
					background: seagreen;
				}
			}
			/* 
			 PC端设定
			 设定大于1200像素的样式
			 */
			@media only screen and (min-width:1200px) {
				.d1{
					width: 33%;
					background: skyblue;
				}
			}
			/* 
			 平板设定
			 设定600px-1200px之间的样式
			 */
			@media only screen and (min-width:600px) and  (max-width:1200px){
				.d1{
					width: 50%;
					background: pink;
				}
			}
			
		</style>
	</head>
	<body>
		<!-- 
		 大屏幕pc端的时候，d1占据50%的宽度
		 小屏幕移动端的时候，d1占据100%的宽度
		 -->
		<div class="d1">
		</div>
	</body>
</html>

```
