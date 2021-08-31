---
title: JavaScript中数组常用方法&数组的遍历
date: 2021-07-08 16:57:49
top_img: https://static001.geekbang.org/resource/image/13/6b/13daccec5e12c89ea63e8c51dd20cb6b.jpg
cover: https://static001.geekbang.org/resource/image/13/6b/13daccec5e12c89ea63e8c51dd20cb6b.jpg
tags:
- JavaScript
categories:
- JavaScript
aplayer: true
---

## JavaScript中数组常用方法&数组的遍历

## **一.数组常用方法**

1.**push()**  向数组**最后面**插入一个或多个元素，返回结果为**该数组的长度**。(会改变原数组)
2.**pop()** 删除数组中的**最后一个元素**，返回结果为**被删除的元素**。(会改变原数组)
3.**unshift()** 在数组**最前面**插入一个或多个元素,返回结果为**该数组新的长度**。(会改变原数组)
4.**shift()** 删除数组中的**第一个元素**,返回结果为**被删除的元素**。(会改变原数组)

```javascript
            var arr = ["a","b","c","d","e"]
			var arrPush = arr.push("ok")
			console.log(arrPush); //6
			var arrPop = arr.pop("ok");
			console.log(arrPop);//ok
			var arrUnshift = arr.unshift("skr","ok")
			console.log(arrUnshift);//7
			var arrShift = arr.shift();
			console.log(arrShift);  //skr
```

5.**slice()** 从数组中**提取**指定的一个或者多个元素，返回结果为**新的数组**。(不会改变原数组)


```javascript
  新数组 = 原数组.slice(开始索引位置，结束索引位置)； 前闭后开
```

```javascript
			var arr = ["a","b","c","d","e"]
			
			var result1 = arr.slice(2);//从第二个值开始提取
			var result2 = arr.slice(-2);//提取最后两个元素
			var result3 = arr.slice(1,3);//提取索引为第一个到第四个的值（但不包括第四个）
			var result4 = arr.slice(4,0);//空
			console.log(result1);// ["c", "d", "e"]
			console.log(result2);//["d", "e"]
			console.log(result3);//["b", "c"]
			console.log(result4);//[]
```

slice()可以将伪数组转换为真数组：

```javascript
array = Array.prototye.slice.call(arrayLike)
或者
array = [].call(arrayLike)
或者
array = Array.from(arrayLike)//ES6中的新API
```

6.**splice()** 从数组中**删除**指定的一个或者多个元素，返回结果为**新的数组**。(会改变原数组)

```javascript
  新数组 = 原数组.splice(开始索引index，需要删除个数，添加的内容)； 可以删除元素也可以添加元素
```

```javascript
			var arr = ["a","b","c","d","e"]
			
			var result1 = arr.splice(1,0,"boy");//删除0个内容，从索引值为1处添加元素
			var result2 = arr.splice(1,1);//删除一个元素，返回删除的元素值
			var result3 = arr.splice(1,2,"skr");//删除两个元素，并且添加新元素
			
			console.log(arr);//["a", "boy", "b", "c", "d", "e"]
			console.log(result1);//[]
			console.log(result2);//["boy"]
			console.log(result3);//["b", "c"]
			console.log(arr);//["a", "skr", "d", "e"]
```

7.**concat()** 连接两个或者多个数组，返回结果为**新的数组**。(不会改变原数组)

```javascript
新数组 = 原数组.concat(数组一，数组二，...)
```

```javascript
            var arr1 = [1,2,3];
			var arr2 = ["a","b","c"];
			var arr3 = ["one","two","three"]
			
			var newArr1 = arr1.concat(arr2);
			var newArr2 = arr1.concat(arr2,arr3);
			
			console.log(newArr1);//[1, 2, 3, "a", "b", "c"]
			console.log(newArr2);// [1, 2, 3, "a", "b", "c", "one", "two", "three"]
```

8.**join()** 将数组转换为字符串，返回结果为**转换后的字符串**。(不会改变原数组)

```javascript
新的字符串 = 原数组.join(参数)；//参数选填
```

```javascript
			var arr = ["A","B","C","D"];
			
			console.log(arr.toString());//A,B,C,D
			var str1 = arr.join();//不传参数时用法和toSting()一样
			var str2 =arr.join("-");//传入的参数可以作为字符串的分割符号
			var str3 = arr.join("");//传入空则不需要分割符号
			
			console.log(str1);//A,B,C,D
			console.log(str2);//A-B-C-D
			console.log(str3);//ABCD
```

9.**reverse()**反转数组,返回结果为**反转后的数组**。(会改变原数组)

```javascript
反转后的数组 = 数组.reverse();
```

```javascript
            var arr = ["A","B","C","D"];
			
			var arrReverse = arr.reverse();
			
			console.log(arrReverse);//["D", "C", "B", "A"]
```

10.**sort()** 对数组的元素，默认按照**Unicode编码**，从小到大进行排序。 (会改变原数组)

```javascript
使用 sort()不带参数时，则默认按照Unicode编码，从小到大进行排序
```

```javascript
            var arr1 = ["2","50","100","8","44"];//当数组为字符串时
			var newArr = arr1.sort();//按照字符串的第一个值Unicode编码进行比较
			console.log(newArr);//["100", "2", "44", "50", "8"]
			
			var arr2 = [2,56,400,12,30,6];//当数组为字符串时
			var newArr = arr2.sort();//也是按照字符串的第一个值Unicode编码进行比较
			console.log(newArr);//[12, 2, 30, 400, 56, 6]
```

```javascript
如果在 sort()方法中带参，我们可以自定义排序规则，添加一个回调函数，回调函数中定义两个参数，
浏览器分别会使用数组中的元素作为实参去调用回调函数。根据返回值来决定排序：
1.如果返回一个大于0的值，则元素会交替位置
2.如果返回一个小于0的值，则元素位置不变
3.如果返回0，则两个元素相等，则不交换位置
```

```javascript
			var arr3 = [45,88,11,66,100];
			var newArr1 = arr3.sort()//[100, 11, 45, 66, 88]
			console.log(newArr1);
			var fn = function(a,b){
				if(a>b){
					return 1
				}else{
					return -1
				}
			}
			var newArr2 = arr3.sort(fn)
			console.log(newArr2);//[11, 45, 66, 88, 100]
```

## 二.数组的遍历

1.**forEach()** 没有返回值，和for循环类似

```javascript
            var hotModel = yidianzu.data.hotModel;
			console.log(hotModel);
			for(var i=0;i<hotModel.length;i++){
				console.log(i);
				console.log(hotModel[i])
			}
			var fn = function(item,index,arr){//item:hotModel[i]  index:i arr:hotModel
				console.log(item);
				console.log(index);
			}
			hotModel.forEach(fn);
```

**函数原理：**

```javascript
            var fn = function(item,index,arr){//item:hotModel[i]  index:i arr:hotModel
				console.log(item);
				console.log(index);
			}
			hotModel.forEach(fn);
			//forEach函数原理
			hotModel.__proto__.forEvery = function(fn){
				for(var i=0;i<this.length;i++){  //this指的是hotModel
					fn(this[i],i,this)
				}
			}
			hotModel.forEvery(fn)
```

2.**map()** 对数组的每一项进行加工，将数组**组成一个新的数组**(不会改变原数组）

```javascript
    	       var arr = [1,2,3,4,5]
				var fn = function(item,index){
					return item + 10;
				}
				var arr2 = arr.map(fn);
				console.log(arr2);//[10,11,12,13,14,15] 返回一个新数组，即为map修改后的数组
```

**函数原理**

```javascript
				var arr = [1,2,3,4,5]
				var fn = function(item,index){
					return item + 10;
				}
				var arr2 = arr.map(fn);
				console.log(arr2);//[10,11,12,13,14,15] 返回一个新数组，即为map修改后的数组
				//map函数原理
				arr.__proto__.forMap = function(fn){
					var newArr = [];
					for(var i=0;i<this.length;i++){
						//i:索引值
						//this[i] = arr[i]
						//this:arr
						newArr.push(fn(this[i],i,arr))
					}
					return newArr;//map有返回值
				}
				var arr3 = arr.forMap(fn);
				console.log(arr3);//[10,11,12,13,14,15]
```

3.**filter()** 对数组中每一项运行回调函数，改函数返回结果是true的项，将**组成新的数组**，返回结果为新的数组，起到过滤的作用。(不会改变原数组)。

```javascript
            var arr = [1,2,3,4,5,6]
			var arr2=arr.filter(function(item,index){
				if(item>3){
					return true;
				}else{
					return false;
				}
			})
			console.log(arr2);//[4,5,6]
```

**函数原理：**

```javascript
               var arr = [1,2,3,4,5]
				var fn = function(item,index,arr){
					if(item>2){
						return true
					}else{
						return false
					}
				}
				var arr2 = arr.filter(fn);
				//返回一个新的数组，即为过滤后的数组
				console.log(arr2);//[3,4,5]
				//map函数原理
				arr.__proto__.forMap = function(callBack){
					var newArr = [];
					for(var i=0;i<this.length;i++){//this指的是当前数组arr-->arr.forMap(fn)
					     //i:索引值
						 //this[i] = arr[i]
						 //this:arr
						var isSava = callBack(this[i],i,this)
						//isSave 为boolean值
						if(isSava){
							newArr.push(this[i]);
						}
					}
					return newArr
				}
				var arr3 = arr.forMap(fn);
				console.log(arr3);//[3,4,5]
```

4.**every()** 如果有一项返回false，则停止遍历，此方法返回false。

```javascript
            //every用于判断数组是否全部满足条件
            var arr = [80,50,100,200,300];
			var isArr =arr.every(function(item,index,arr){
				if(item>100){
					return true;
				}else{
					return false
				}
			})
			if(isArr){
				console.log("全部数字大于100")
			}else{
				console.log("有数字小于100")  //有数字小于100
			}
```

5.**some()** 如果有一项返回ture，则停止遍历，此方法返回true。

```javascript
            var arr = [80,50,100,200,300];
			var isArr =arr.some(function(item,index,arr){
				if(item>100){
					return true;
				}else{
					return false
				}
			})
			if(isArr){
				console.log("至少有一个数字大于100")//至少有一个数字大于100
			}else{
				console.log("有数字小于100")  
			}
```

6.**reduce()** 为数组中的每一个元素执行回调函数。
reduce()需要对内容进行遍历，并且最终输出一个结果的时候可以使用reduce()

```javascript
           var arr = [80,50,100,200,300];
			var sum =arr.reduce(function(value,item,index,arr){
				//value为初始值 当前初始值为0
				return value+item;
			},0)
			console.log(sum);//730
```

## **三.数组的其他方法**

**1.indexof()和lastIndexof()**

```javascript
indexof(value)：从前往后索引，获取元素第一次出现的下标
索引值 = 数组.indexof(value);
lastIndexof(value)：从前往后索引，获取元素最后一次出现的下标
索引值 = 数组.lastIndexof(value);
利用这两个哥方法可以判断元素中是否存在谋值，如果不存在返回-1。

```

```javascript
           var arr = ["冷血","无情","追命","铁手"];
			var result1 = arr.indexOf("无情");
			var result2 = arr.lastIndexOf("铁手");
			var result3 = arr.indexOf("吴亦凡");
			console.log(result1);//1
			console.log(result2);//3
			if(result3 == -1){
				console.log("数据不存在")//数据不存在
			}else{
				console.log("数据存在")
			}
```

2.**find()** 找出第一个满足[指定条件返回true]的元素，一旦找到符合条件的第一个元素，将不再继续往下遍历。

```javascript
               find(function(item,index,arr){
                     return true
               })
```

```javascript
            var arr = [12,52,62,32,12];
			var result = arr.find(function(item,index){
				return item>30
			})
			console.log(result);//52
```

3.**findIndex()** 找出满足条件的索引值

```javascript
var arr = [12,52,62,32,12];
			var result = arr.findIndex(function(item,index){
				return item>30
			})
			console.log(result);//1
```

4.**Array.from()**  将伪数组遍历成真数组
伪数组原型链中没有Array.prototype,而真数组中含有Array.prototype。所以伪数组中没有数组相关的方法。

```javascript
           var fn = function(){
				console.log(arguments)
				var arr= Array.from(arguments);
				console.log(arr);//[1,2,3]
			}
			fn(1,2,3);
```

5.**Array.of()**  将一系列值转换为数组

```javascript
var arr = Array.of(value1,value2,.....)

            var Ary = Array.of(1,"one",["Two","three"])
			console.log(Ary)//[1, "one", Array(2)]
```