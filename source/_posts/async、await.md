---
title: async、await
date: 2021-02-01 16:57:49
top_img: https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cb5b42b73a624a38b10e8e48e00ecfa5~tplv-k3u1fbpfcp-watermark.image
cover: https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cb5b42b73a624a38b10e8e48e00ecfa5~tplv-k3u1fbpfcp-watermark.image
tags:
- JavaScript
categories:
- JavaScript
---

# async、await

## async

### 什么是async？

`async` 函数是 `Generator` 函数的语法糖。使用 关键字 `async` 来表示，在函数内部使用 `await` 来表示异步。相较于 `Generator`，`async` 函数的改进在于下面四点：

- **内置执行器**。`Generator` 函数的执行必须依靠执行器，而 `async` 函数自带执行器，调用方式跟普通函数的调用一样
- **更好的语义**。`async` 和 `await` 相较于 `*` 和 `yield` 更加语义化
- **更广的适用性**。`co` 模块约定，`yield` 命令后面只能是 Thunk 函数或 Promise对象。而 `async` 函数的 `await` 命令后面则可以是 Promise 或者 原始类型的值（Number，string，boolean，但这时等同于同步操作）
- **返回值是 Promise**。`async` 函数返回值是 Promise 对象，比 Generator 函数返回的 Iterator 对象方便，可以直接使用 `then()` 方法进行调用

`async`是ES7新出的特性，表明当前函数是异步函数，不会阻塞线程导致后续代码停止运行。

### 怎么用

申明之后就可以进行调用了

```js
async function asyncFn() {
　　return 'hello world';
}
asyncFn();
```

这样就表示这是异步函数，返回的结果

![在这里插入图片描述](https://img-blog.csdnimg.cn/bd1bf882248e4c748d2db0f4d99cc513.png)

> async 表示函数里有异步操作
>
> await 表示紧跟在后面的表达式需要等待结果。

返回的是一个`promise`对象，状态为`resolved`，参数是`return`的值。那再看下面这个函数

```js
async function asyncFn() {
    return '我后执行'
}
asyncFn().then(result => {
    console.log(result);
})
console.log('我先执行');
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/d6e26fa50cef4b978f6fa3cb085c501d.png)

上面的执行结果是先打印出`'我先执行'`，虽然是上面`asyncFn()`先执行，但是已经被定义异步函数了，不会影响后续函数的执行。

现在理解了`async`基本的使用，那还有什么特性呢？

`async`定义的函数内部会默认返回一个`promise`对象，如果函数内部抛出异常或者是返回`reject`，都会使函数的`promise`状态为失败`reject`。

```js
async function e() {    
    throw new Error('has Error');
}
e().then(success => console.log('成功', success))   
   .catch(error => console.log('失败', error));![img](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2018/6/13/163f53f3b4a1f90d~tplv-t2oaga2asx-zoom-in-crop-mark:652:0:0:0.awebp)
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/a877e2810a3445598a7f4e9fcfb2c9a8.png)

我们看到函数内部抛出了一个`异常`，返回`reject`，`async`函数接收到之后，判定执行失败进入`catch`，该返回的错误打印了出来。

```js
async function throwStatus() {    
    return '可以返回所有类型的值'
}
throwStatus().then(success => console.log('成功', success))             
             .catch(error => console.log('失败', error));

//打印结果

成功 可以返回所有类型的值
```

async`函数接收到返回的值，发现不是`异常`或者`reject`，则判定成功，这里可以`return`各种数据类型的值，`false`,`NaN`,`undefined`...总之，都是`resolve

但是返回如下结果会使`async`函数判定失败`reject`

1. 内部含有直接使用并且未声明的变量或者函数。
2. 内部抛出一个错误`throw new Error`或者返回`reject`状态`return Promise.reject('执行失败')`
3. 函数方法执行出错（🌰：Object使用push()）等等...

还有一点，在`async`里，必须要将结果`return`回来，不然的话不管是执行`reject`还是`resolved`的值都为`undefine`，建议使用箭头函数。

其余返回结果都是判定`resolved`成功执行。

```js
//正确reject方法。必须将reject状态return出去。
async function PromiseError() {    
   return Promise.reject('has Promise Error');
}

//这是错误的做法，并且判定resolve，返回值为undefined,并且Uncaught报错
async function PromiseError() {
  Promise.reject('这是错误的做法');
}

PromiseError().then(success => console.log('成功', success))              
              .catch(error => console.log('失败', error));
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/101c19d5dff04c7f8e3b88233c78ed33.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70)

我们看到第二行多了个`Promise`对象打印，不用在意，这个是在`Chrome`控制台的默认行为，我们平常在控制台进行赋值也是同样的效果。如果最后`执行语句`或者`表达式`没有`return`返回值，默认`undefined`，做个小实验。

```js
var a = 1;
//undefined
------------------------------------------------------------
console.log(a);
//1
//undefined
------------------------------------------------------------
function a(){ console.log(1) }
a();
//1
//undefined
------------------------------------------------------------
function b(){ return console.log(1) }
b();
//1
//undefined
------------------------------------------------------------
function c(){ return 1}
c();
//1
------------------------------------------------------------
async function d(){
    '这个值接收不到'
}
d().then(success => console.log('成功',success));
//成功  undefined
//Promise { <resolved>: undefined }
-----------------------------------------------------------
async function e(){
    return '接收到了'
}
e().then(success => console.log('成功',success));
//成功  接收到了
//Promise { <resolved>: undefined }
```

最后一行`Promise { <resolved> : undefined }` 是因为返回的是`console.log`执行语句，没有返回值。

```js
d().then(success => console.log('成功',success)}
等同于
d().then(function(success){ 
            return console.log('成功',success);
        });
```

js本身是单线程的，通过v8我们可以拥有"异步"的能力

认识完了async，来讲讲await。

## await是什么？

`await`意思是async wait(异步等待)。这个关键字只能在使用`async`定义的函数里面使用。任何`async`函数都会默认返回`promise`，并且这个`promise`解析的值都将会是这个函数的返回值，而`async`函数必须等到内部所有的 `await` 命令的 `Promise` 对象执行完，才会发生状态改变。

**打个比方，await是学生，async是校车，必须等人齐了再开车。**

就是说，必须等所有`await` 函数执行完毕后，才会告诉`promise`我成功了还是失败了，执行`then`或者`catch`

```js
async function awaitReturn() {     
    return await 1
};
awaitReturn().then(success => console.log('成功', success))
             .catch(error => console.log('失败',error))
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/f9a4f6da4c824e9f924c2d48b24eb1ef.png)

在这个函数里，有一个`await`函数，async会等到`await 1` 这一步执行完了才会返回`promise`状态，毫无疑问，判定`resolved`。

**很多人以为`await`会一直等待之后的表达式执行完之后才会继续执行后面的代码，\**\*\*实际上`await`是一个让出线程的标志\*\**\*。`await`后面的函数会先执行一遍(比如await Fn()的Fn ,并非是下一行代码)，然后就会跳出整个`async`函数来执行后面js栈的代码。等本轮事件循环执行完了之后又会跳回到`async`函数中等待await\**\**后面表达式的返回值，如果返回值为非`promise`则继续执行`async`函数后面的代码，否则将返回的`promise`放入`Promise`队列（Promise的Job Queue）**

来看个简单点的例子

```js
const timeoutFn = function(timeout){ 
	return new Promise(function(resolve){
		return setTimeout(resolve, timeout);
               });
}

async function fn(){
    await timeoutFn(1000);
    await timeoutFn(2000);
    return '完成';
}

fn().then(success => console.log(success));
```

这里本可以用箭头函数写方便点，但是为了便于阅读本质，还是换成了ES5写法，上面执行函数内所有的await函数才会返回状态，结果是执行完毕3秒后才会弹出'`完成`'。

**正常情况下，await 命令后面跟着的是 Promise ，如果不是的话，也会被转换成一个 立即 resolve 的 Promise。**

也可以这么写

```js
function timeout(time){
    return new Promise(function(resolve){
        return setTimeout(function(){ 
                    return resolve(time + 200)
               },time);
    })
}

function first(time){
    console.log('第一次延迟了' + time );
    return timeout(time);
}
function second(time){
    console.log('第二次延迟了' + time );
    return timeout(time);
}
function third(time){
    console.log('第三次延迟了' + time );
    return timeout(time);
}

function start(){
    console.log('START');
    const time1 = 500;
    first(time1).then(time2 => second(time2) )
                .then(time3 => third(time3)  )
                .then(res => {
                              console.log('最后一次延迟' + res );
                              console.timeEnd('END');
                             })
};
start();
```

这样用then链式回调的方式执行resolve

```js
//打印结果

START
第一次延迟了500
第二次延迟了700
第三次延迟了900
最后一次延迟1100
END
```

用async/await呢？

```js
async function start() {
    console.log('START');
    const time1 = 500;
    const time2 = await first(time1);
    const time3 = await second(time2);
    const res = await third(time3);
    console.log(`最后一次延迟${res}`);
    console.log('END');
}
start();
```

达到了相同的效果。但是这样遇到一个问题，如果`await`执行遇到报错呢

```js
async function start() {
    console.log('START');
    const time1 = 500;
    const time2 = await first(time1);
    const time3 = await Promise.reject(time2);
    const res = await third(time3);
    console.log(`最后一次延迟${res}`);
    console.log('END');
}
start();
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/91056032f6264960979d37cd1a70bb47.png)

返回reject后，后面的代码都没有执行了，以此迁出一个例子:

```js
let last;
async function throwError() {  
    await Promise.reject('error');    
    last = await '没有执行'; 
}
throwError().then(success => console.log('成功', last))
            .catch(error => console.log('失败',last))
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/515f365c6a6b42a8ba28f09ee5ef9e78.png)

其实`async`函数不难，难在错处理上。

上面函数，执行的到`await`排除一个错误后，就停止往下执行，导致`last`没有赋值报错。

`async`里如果有多个await函数的时候，如果其中任一一个抛出异常或者报错了，都会导致函数停止执行，直接`reject`;

怎么处理呢，可以用`try/catch`，遇到函数的时候，可以将错误抛出，并且继续往下执行。

```js
let last;
async function throwError() {  
    try{  
       await Promise.reject('error');    
       last = await '没有执行'; 
    }catch(error){
        console.log('has Error stop');
    }
}
throwError().then(success => console.log('成功', last))
            .catch(error => console.log('失败',last))
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/8970ccb9b4104f8a83231bf1e92f881a.png)

这样的话，就可以继续往下执行了。

来个🌰练习下

```js
function testSometing() {
    console.log("testSomething");
    return "return testSomething";
}

async function testAsync() {
    console.log("testAsync");
    return Promise.resolve("hello async");
}

async function test() {
    console.log("test start...");

    const testFn1 = await testSometing();
    console.log(testFn1);

    const testFn2 = await testAsync();
    console.log(testFn2);

    console.log('test end...');
}

test();

var promiseFn = new Promise((resolve)=> { 
                    console.log("promise START...");
                    resolve("promise RESOLVE");
                });
promiseFn.then((val)=> console.log(val));

console.log("===END===")
```

执行结果

![在这里插入图片描述](https://img-blog.csdnimg.cn/a26527124b37419b93418c2a779f2086.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70)

我们一步步来解析

首先`test()`打印出`test start...`

然后 `testFn1 = await testSomething();` 的时候，会先执行`testSometing()`这个函数打印出“`testSometing`”的字符串。

`testAsync()`执行完毕返回`resolve`，之后`await`会让出线程就会去执行后面的，触发`promiseFn`打印出“`promise START...`”。

接下来会把返回的Promise`resolve("promise RESOLVE")`放入Promise队列（Promise的Job Queue），继续执行打印“`===END===`”。

等本轮事件循环执行结束后，又会跳回到`async`函数中（`test()`函数），等待之前`await` 后面表达式的返回值，因为`testSometing()` 不是`async`函数，所以返回的是一个字符串“`return``testSometing`”。

`test()`函数继续执行，执行到`testFn2()`，再次跳出`test()`函数，打印出“`testAsync`”，此时事件循环就到了Promise的队列，执行`promiseFn.then((val)=> console.log(val));`打印出“`promise RESOLVE`”。

之后和前面一样 又跳回到test函数继续执行`console.log(testFn2)`的返回值，打印出“`hello async`”。

最后打印“`test end...`”。

加点料，让`testSomething()`变成`async`

```js
async function testSometing() {
    console.log("testSomething");
    return "return testSomething";
}

async function testAsync() {
    console.log("testAsync");
    return Promise.resolve("hello async");
}

async function test() {
    console.log("test start...");

    const testFn1 = await testSometing();
    console.log(testFn1);

    const testFn2 = await testAsync();
    console.log(testFn2);

    console.log('test end...');
}

test();

var promiseFn = new Promise((resolve)=> { 
                    console.log("promise START...");
                    resolve("promise RESOLVE");
                });
promiseFn.then((val)=> console.log(val));

console.log("===END===")
```

执行结果

![在这里插入图片描述](https://img-blog.csdnimg.cn/db992cafdc8841f8a445ed5309d81f84.png)

和上一个例子比较发现`promiseFn.then((val)=> console.log(val)); `先于`console.log(testFn1)` 执行。



原因是因为现在的版本async函数会被await resolve，

```js
function testSometing() {
    console.log("testSomething");
    return "return testSomething";
}
console.log(Object.prototype.toString.call(testSometing)) // [object Function]
console.log(Object.prototype.toString.call(testSometing())) // [object String]

async function testSometing() {
    console.log("testSomething");
    return "return testSomething";
}
console.log(Object.prototype.toString.call(testSometing)) // [object AsyncFunction]
console.log(Object.prototype.toString.call(testSometing())) // [object Promise]
```

`testSomething()`已经是async函数，返回的是一个Promise对象需要等它 resolve 后将当前Promise 推入队列，随后先清空调用栈，所以会"跳出" test() 函数执行后续代码，随后才开始执行该 Promise。

### 参考文献

[理解 JavaScript 的 async/await](https://link.juejin.cn/?target=https%3A%2F%2Fsegmentfault.com%2Fa%2F1190000007535316)

[一次性让你懂async/await，解决回调地狱](https://juejin.cn/post/6844903621360943118#heading-0)

