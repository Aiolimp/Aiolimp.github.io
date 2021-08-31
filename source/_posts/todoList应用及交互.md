---
title: todoList应用及交互
date: 2021-03-12 16:57:49
top_img: https://static001.geekbang.org/resource/image/4b/8b/4b2db13a5c87e4438146baa0cf0c688b.jpg
cover: https://static001.geekbang.org/resource/image/4b/8b/4b2db13a5c87e4438146baa0cf0c688b.jpg
tags:
- 个人demo
categories:
- 个人demo
aplayer: true
---
# todoList应用及交互

效果图：(主要以移动端为基础)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200904092754946.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70#pic_center)

```javascript
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8" />
		<meta name="viewport" content="width=device-width,initial-scale=1,minimum-scale=1,maximum-scale=1,user-scalable=no" />
		<link rel="stylesheet" type="text/css" href="css/style.css" />
		<title></title>
	</head>
	<body>
		<div class="main">
			<div class="header">
				<div class="logo">ByTodo</div>
				<input type="text" id="input" placeholder="请输入代办事项:" />
			</div>
			<div class="doing todo">
				<h3><span class="title">正在进行</span><span id="num"></span></h3>
				<div class="list">
					<!-- <div class="todoItem">
						<input type="checkbox" />
						<div class="content">正在吃饭</div>
						<div class="del">删除</div>
					</div> -->
				</div>
			</div>
			<div class="done todo">
				<h3><span class="title">已经完成</span><span id="num2"></span></h3>
				<div class="list">
					<!-- <div class="todoItem">
						<input type="checkbox" checked="checked"/>
						<div class="content">正在吃饭</div>
						<div class="del">删除</div>
					</div> -->
				</div>
			</div>

		</div>
		<script type="text/javascript">
			//定义一个全局变量，将待办事项保存到这个变量
			if (localStorage.todoList == undefined) {
				var todoList = []
			} else {
				var todoList = JSON.parse(localStorage.todoList)
			}
			//获取输入框对象
			var inputDom = document.querySelector("#input");

			var doingListDiv = document.querySelector(".doing .list")

			var doneListDiv = document.querySelector(".done .list")

			var mainDiv = document.querySelector(".main")
			//监听输入按键事件为Enter
			inputDom.onkeydown = function(event) {
				if (event.key == "Enter") {
					var value = inputDom.value;
					var objItem = {
						content: value,
						isDone: false
					}
					todoList.push(objItem);
					console.log(todoList)
					reader(todoList);
				}
			}
			var reader = function(todoList) {
				//将对象转换为JSON格式字符串，本地存储
				localStorage.todoList = JSON.stringify(todoList)
				//每一次渲染前清空对象
				doingListDiv.innerHTML = '';
				doneListDiv.innerHTML = '';
				todoList.forEach((item, index) => {
					//新建className为totoItml的div
					var todoItem = document.createElement("div")
					todoItem.className = "todoItem"
					//获取正在进行/已经完成事项次数
					var num = document.querySelector("#num")
					var num2 = document.querySelector("#num2")
					if (item.isDone) {
						var todoList1 = todoList.filter(item => {
							return item.isDone == true
						})
						num2.innerHTML = todoList1.length
						todoItem.innerHTML =
							`
							<input type="checkbox" data-index=${index}  checked="checked">
							<div class="content">` + item.content +
							`</div>
							<div class="del" data-index=${index}>删除</div>
						`
						doneListDiv.appendChild(todoItem)
					} else {
						var todoList2 = todoList.filter(item => {
							return item.isDone == false
						})
						num.innerHTML = todoList2.length
						todoItem.innerHTML = `
							<input type="checkbox" data-index=${index} >
							<div class="content">` +
							item.content + `</div>
							<div class="del" data-index=${index} >删除</div>
						`
						doingListDiv.appendChild(todoItem)
					}
				})
			}
			reader(todoList);
			//正在进行复选框点击事件
			doingListDiv.onchange = function(e) {
				//通过事件对象找到dom对象，获取索引值
				var index = parseInt(e.target.dataset.index);
				todoList[index].isDone = true;
				reader(todoList);
			}
			//已经完成复选框 点击事件
			doneListDiv.onchange = function(e) {
				//通过事件对象找到dom对象，获取索引值
				var index = parseInt(e.target.dataset.index);
				todoList[index].isDone = false;
				reader(todoList);
			}
			//删除点击事件
			mainDiv.onclick = function(e) {
				console.log(e)
				if (e.target.className == "del") {
					var index = parseInt(e.target.dataset.index);
					todoList.splice(index, 1)
					reader(todoList)
				}
			}
			//rem
			function setRem() {
				var screenWidth = window.innerWidth; //获取屏幕宽度
				var unit = screenWidth / 3.75; //屏幕的宽度/设计稿占满全屏的宽度为多少rem
				var html = document.querySelector("html")
				html.style.fontSize = unit + "px";
			}
			setRem();
			//窗口大小发生改变时
			window.onresize = function() {
				setRem();
			}
		</script>
	</body>
</html>


```

CSS：

```javascript
*{
	margin: 0;
	padding: 0;
	box-sizing: border-box;
}
body{
	background-color: #CDCDCD;
	font-size: 16px;
}
.main{
	width: 3.75rem;
	/* height: 10vh; */
}
.header{
	width: 3.75rem;
	height: 0.5rem;
	display: flex;
	justify-content: space-between;
	align-items: center;
	background-color: rgba(47,47,47,0.98);
	color: aliceblue;
}
.header .logo{
	height: 0.5rem;
	width: 1.2rem;
	text-align: center;
	line-height: 0.5rem;
	font-size: 0.25rem;
	font-weight: 900;
}
.header>input{
	height: 0.3rem;
	margin: 0 0.2rem;
	width: 2.2rem;
	border-radius: 0.05rem;
	/* 边框 */
	border: none;
	/* 点击后边框 */
	outline: none;
	padding: 0 0.1rem;
}
.todo h3{
	height: 0.6rem;
	line-height: 0.6rem;
	display: flex;
	justify-content: space-between;
	align-items: center;
	padding: 0 0.15rem;
}
.todo .list{
	padding: 0 0.15rem;
}
.todo .todoItem{
	display: flex;
	height: 0.32rem;
	height: 0.38rem;
	line-height: 0.32rem;
	align-items: center;
	background-color: aliceblue;
	border-radius: 0.05rem;
	/* 避免溢出 */
	overflow: hidden;
	margin-bottom: 0.3rem;
}
/* 设置伪元素 */
.todo .todoItem::before{
	width: 0.04rem;
	height: 0.38rem;
	background-color: aqua;
	content: "";
}
.todo .todoItem>input{
	width: 0.2rem;
	height: 0.2rem;
	margin: 0 0.1rem;
}
.todo .todoItem .content{
	width: 2.65rem;
}
.todo .todoItem .del{
	background-color: orangered;
	width: 0.5rem;
	height: 0.2rem;
	font-size: 0.12rem;
	text-align: center;
	line-height: 0.2rem;
	border-radius: 0.05rem;
	margin: 0 0.1rem;
}
.done.todo .todoItem{
	/* 透明度 */
	opacity: 0.3;
	/* -webkit-filter: grayscale(1); */
	
}
.pc{
	font-size: 100px !important;
}
.pc .main{
	width: 600px;
	margin: 0 auto;
}

```