---
title: DOM简介和DOM操作
date: 2021-07-26 16:57:49
top_img: https://static001.geekbang.org/resource/image/cb/8d/cb4a6ded3cdc7c0eb6a7e3879567f08d.jpg
cover: https://static001.geekbang.org/resource/image/cb/8d/cb4a6ded3cdc7c0eb6a7e3879567f08d.jpg
tags:
- JavaScript
categories:
- JavaScript
aplayer: true
---

# DOM简介和DOM操作

### **JavaScript的组成**

- ECMAScript：JavaScript的语法标准，包括变量、表达式、运算符、函数、if语句、for语句等等。
- **DOM**：文档对象模型(Document object Model)，操作网页上的元素的API，比如让盒子移动、变色、轮播图等。
- **BOM**：浏览器对象模型，操作浏览器部分功能的API，比如让浏览器自动滚动。

### **节点**

节点(Node):构成HTML网页的最基本单元，网页中的每一个部分都可以称为是一个节点，比如：html标签、属性、文本、注释等都是一个节点。常用节点分为四大类：

- 文档节点(文档)：整个HTML文档就是一个文档节点。
- 元素节点(标签):HTML标签。
- 属性节点(属性)：元素的属性。
- 文本节点(文本):HTML标签中的文本内容（包括标签之间的空格、换行）。
  节点的类型不同，属性和方法也都不同，所有的节点都是Object。
  **什么是DOM**
  DOM：Document Object Model，文档对象模型。DOM为文档提供了结构化表示，并定义了如何通过脚本来访问文档结构。目的其实就是为了能让js操作html元素而制定的一个规范。
  DOM由节点组成。
  **解析过程**：HTML加载完毕，渲染引擎会在内存中吧HTML文档，生成一个DOM树，getElementById是获取DOM上的元素节点，然后操作的时候修改的是改元素的属性。
  **DOM树：**(一切都是节点)
  结构如下：
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200831232426959.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70#pic_center)
  **DOM元素可以做什么**
- 找对象(元素节点)
- 设置元素的属性值
- 设置元素的样式
- 动态创建和删除元素
- 事件的触发响应：事件源、事件、事件的驱动程序

**1.元素节点的获取**

```javascript
    //2009ES5之前发布的ECMASCRIPT标准
			var div1 = document.getElementById("box1");//通过id获取元素节点(因为id是唯一的所以只获取一个)
			var arr1 = document.getElementsByClassName("box2")//通过标签名获取元素节点数组
			var arr2 = document.getElementsByTagName("div")//通过类名获取元素节点数组
    //ES5之后的ECMASCRIPT标准
			//document.querySelector 返回最先匹配的第一个对象
			var abc = document.querySelector("#b")//和document.getElementById获取的对象是一样的
			var cba = document.querySelector(".b")
			//document.querySelectorAll 获取所有匹配到的元素对象
			var aaa = document.querySelectorAll(".b")
```

**2.DOM中访问关系的获取**

- 获取父节点：节点.parentNode
- 获取兄弟节点：nextSibling    |  nextElementSibling | previousSibling | previousElementSibling
- 获取子节点：   firstChild | firstElementChild | lastChild | lastElementChild
- 所有子节点： childNodes | children

**3.DOM节点的操作**

- 创建节点：

```javascript
            新的标签 = document.createElement("标签名")；


```

- 插入节点

```javascript
方式一： 
父节点.appendChild（新的子节点）//父节点的最后插入一个新的子节点
            //1.创建元素
			var h1 = document.createElement("h1")
			//2.设置元素
			h1.innerHTML = "你今天好帅！"
			//3.插入元素，首先找到被插入的元素
			var d1 = document.querySelector(".d1")
			//4.通过appendChild()追加子元素
			d1.appendChild(h1)
方式二：
父节点.insertBefore（新的子节点，作为参考的子节点）//在参考节点前插入一个新节点
			//在什么面前插入元素
			//1.创建img元素
			var img  = document.createElement('img')
			//2.设置img的src属性
			img.src = "img/123.jpg"
			//3.找到被插入的父元素，<body>
			var body = document.querySelector('body')
			//4.在body的d3前面插入内容
			var d3 = document.querySelector('.d3')
			body.insertBefore(img,d3)
			d3.parentElement.insertBefore(img,d3) // 通过d3.parentElement直接获取父元素
```

- 删除节点

```javascript
被删除的父元素.removeChild（删除的元素）

			//删除元素
			//1.找到被删除的元素
			var body = document.querySelector('body')
			//2.找到要删除的元素
			var d2 = document.querySelector('.d2')
			//3.使用removeChild()方法删除元素
			body.removeChild(d2)
```

- 复制节点

```javascript
要复制的节点.cloneNode()
要复制的节点.cloneNode(true)
//不带参数或者带参数false：只复制节点本身，不复制子节点
//带参数true：即复制节点本身，也复制所有子节点
```

## 

**节点的获取、设置和删除**
1.获取节点

```javascript
元素节点.getAttribute（属性名）
```

2.设置节点

```javascript
元素节点.setAttribute（属性名，新的属性值）
```

3.删除节点

```javascript
元素节点.removeAttribute (属性名)
```

```javascript
<body>
		<div id="d1">
			helloWorld
		</div>
		<script type="text/javascript">
			var d1 = document.querySelector("#d1")
			//获取属性值
			console.log(d1.id)
			console.log(d1['id'])
			console.log(d1.getAttribute('id'))
			//设置属性
			d1.id = "d2"
			d1['id'] = "d3"
			//setAttribute("属性名称","属性值")
			d1.setAttribute("id","d4")
			
			d1.abc="789"//默认元素中如果没有此属性，那么此属性只会创建在对象中，不会显示在html上
			console.log(d1)
			console.log([d1])
			
			d1.setAttribute("aaa","666")//默认元素中如果没有此属性，那么此属性不会创建在对象中个，会显示在html上
			console.log(d1)
			console.log([d1])
			//删除子元素
			d1.removeAttribute("abc")
		</script>
	</body>
```

**innerHtml和innerText的区别**

- value：标签的value属性

```javascript
	<div id="d1">
			helloword
			<h1>helloword</h1>
			<h1>helloword</h1>
		</div>
		<input type="text" name="username" id="username" value="admin"/>
		<input type="password" name="password" id="password" value="123456"/>
		<script type="text/javascript">
			var usernameInput = document.querySelector('#username')
			var username = usernameInput.value;
			console.log(username)//admin
			var passwordInput = document.querySelector("#password")
			var password = passwordInput.value
			console.log(password)//123456
		</script>
```

- innerHtml：双闭合标签里面的内容（识别标签）
- innerText：双闭合标签里面的内容（不识别标签）

```javascript
<div id="d1">
			helloword
			<h1>helloword</h1>
			<h1>helloword</h1>
		</div>
		<script type="text/javascript">
			var d1 = document.querySelector('#d1')
			//获取的d1内部的html代码
			console.log(d1.innerHTML)
			//获取d1内部的文本内容
			console.log(d1.innerText)
			//获取包含d1的html代码
			console.log(d1.outerHTML)
		</script>
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200901221439591.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70#pic_center)

### **DOM操作设置元素样式**

1.通过style修改属性样式
2.通过className修改属性样式
3.通过增加style标签来修改样式

```javascript
<div id="d1">
			hello
		</div>
		<div id="d2" >
			hello
		</div>
		<div class="d3" >
			hello
		</div>
		<script type="text/javascript">
			//1.通过style修改属性样式
			var abc1 = document.querySelector('#d1')
			abc1.style.width= "200px";
			abc1.style.height= "200px";
			abc1.style.backgroundColor= "red";//设置css属性时，如果css属性是多个单词组成，则采用驼峰命名法
			//注意：通过style属性设置的值，优先级是最高的，因为是直接修改标签的style属性
			//2.通过className修改属性样式
			var abc2 = document.querySelector('#d2')
			abc2.className = "bg bigFont"
			//类名还可以通过classList属性的add/remove/replace进行修改className
			abc2.classList.add("shaow")
			abc2.classList.replace("bigFont","smallFont")
			abc2.classList.remove("bg")
			//3.通过增加style标签来修改样式
			var style=document.createElement("style")
			//反引号``可以包括多行字符串
			style.innerHTML= `.d3{
				width: 800px;
				height: 800px;
				background-color: orange;
			}
			`
			var body = document.querySelector("body")
			body.appendChild(style)
```