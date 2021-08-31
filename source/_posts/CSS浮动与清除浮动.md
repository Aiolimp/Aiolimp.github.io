---
title: CSS浮动与清除浮动
date: 2021-04-12 16:57:49
top_img: https://static001.geekbang.org/resource/image/bd/47/bdce168820c080adbcbee78b02292f47.jpg
cover: https://static001.geekbang.org/resource/image/bd/47/bdce168820c080adbcbee78b02292f47.jpg
tags:
- CSS
categories:
- CSS
aplayer: true
---

## CSS浮动与清除浮动

## 1.CSS浮动

**浮动的作用：**
1.1 浮动可以解决文字包围图片的问题
1.2 浮动可以解决div之间的间隔问题
1.3 浮动可以向左或者向右进行排版布局

```javascript
img{
float：left
}
```

  ***添加浮动前，文字行基线和图片对齐。***
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824225107930.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70#pic_center)
***添加浮动后，文字包围图片。***

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824225301848.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70#pic_center)
***添加浮动前，图片之间会有莫名其妙的间隔***
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824230258109.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70#pic_center)
***添加浮动后，图片进行向左浮动***
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824230453473.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70#pic_center)
**浮动可以设置元素，脱离正常的文档流，向左或者向右，靠近父元素边缘或者是设置了浮动的其他元素的边缘**

## 2.CSS浮动的高度塌陷问题

   当父元素高度设置为0时，子元素浮动脱离文档流，h1标签的内容和子元素处于同一行。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200824231210828.png#pic_center)
解决方案：
1.设置父元素高度
2.子元素添加div，清除左右浮动

```javasc
        .clear{
			clear: both;//元素左右两侧均不允许浮动
		}

```

3.最佳解决方案，伪元素清除浮动。

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
	</head>
	<style type="text/css">
		.parent{
			width: 400px;
			/* height: 300px; */
			margin: 0 auto;
			background: deeppink;
		}
		.child{
			display: inline-block;
			width: 100px;
			height: 100px;
			background-color: aqua;
			float: left;
		}
		/* .clear{
			clear: both;
		} */
		.clear:after{
			content: "";
			display:block;
			clear:both;//元素左右两侧均不允许浮动
		}
	</style>
	<body>
		<div class="parent clear">
			<div class="child"></div>
			<div class="child"></div>
			<div class="child"></div>
			<div class="clear"></div>
		</div>
		<h1>Skrrrrrrrr</h1>
	</body>
</html>

```