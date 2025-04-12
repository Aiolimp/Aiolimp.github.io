---
title: JavaScript类型判断
date: 2021-07-07 10:23:20
top_img: https://static001.geekbang.org/resource/image/9f/28/9f68cbdfd275739a1cd3a4dfa85ead28.jpg
cover: https://static001.geekbang.org/resource/image/9f/28/9f68cbdfd275739a1cd3a4dfa85ead28.jpg
tags:
- JavaScript
categories:
- JavaScript
aplayer: true
---
## JavaScript类型判断

### 1.JavaScript数据类型

JavaScript有八种内置类型:除对象外，其他统称为“基本类型”。

这里的类型指的是值，变量是没有类型的，变量可以随时持有任何类型的值。JavaScript中变量是“弱类型”的，一个变量可以现在被赋值为 字符串类型，随后又被赋值为数字类型。

- 空值（null)
- 未定义(undefined)
- 布尔值（boolean)
- 数字（number)
- 字符串（string)
- 对象 (object)
- 符号（symbol, ES6中新增)
- 大整数（BigInt, ES2020 引入）

> `Symbol` 是ES6中引入的一种`原始数据`类型，表示独一无二的值。BigInt（大整数）是 ES2020 引入的一种新的数据类型，用来解决 JavaScript中数字只能到 53 个二进制位（JavaScript 所有数字都保存成 64 位浮点数，大于这个范围的整数，无法精确表示的问题。(在平常的开发中，数据的id 一般用 string 表示的原因)。为了与 Number 类型区别，BigInt 类型的数据必须添加后缀n。 `1234`为普通整数，`1234n`为 `BigInt`。

### 2.typeof

`typeof`是一个操作符而不是函数，用来检测给定变量的数据类型。

```javascript
typeof null // 'object'
typeof undefined; // "undefined"
typeof false; // "boolean"
typeof 1; // "number"
typeof '1'; // "string"
typeof {}; // "object" 
typeof []; // "object" 
typeof new Date(); // "object"

typeof Symbol(); // "Symbol"
typeof 123n // 'bigint'

function a() {}
typeof a //function
```

注：判断一个变量是否被声明可以用（typeof 变量 === “undefined”）来判断

**特殊值NaN返回的是 "number"**

> ```
> NaN 意指“不是一个数字”（not a number），NaN 是一个“警戒值”（sentinel value，有特殊用途的常规值），用于指出
> 数字类型中的错误情况，即“执行数学运算没有成功，这是失败后返回的结果”。
> 
> typeof NaN; // "number"
> 
> NaN 是一个特殊值，它和自身不相等，是唯一一个非自反（自反，reflexive，即 x === x 不成立）的值。而 NaN != NaN
> 为 true。
> ```

所以 typeof 能检测出六种类型的值，但是，除此之外 Object 下还有很多细分的类型，如 Array、Function、Date、RegExp、Error 等。

如果用 typeof 去检测这些类型:



```javascript
var date = new Date();
var error = new Error();
console.log(typeof date); // object
console.log(typeof error); // object
typeof({a:1}) // "object" 普通对象直接返回“object”
typeof [1,3] // 数组返回"object"
```

#### typeof原理

`typeof`原理： **不同的对象在底层都表示为二进制，在Javascript中二进制前（低）三位存储其类型信息**。

- 000: 对象
- 010: 浮点数
- 100：字符串
- 110： 布尔
- 1： 整数

typeof null 为"object", 原因是因为 不同的对象在底层都表示为二进制，在Javascript中二进制前（低）三位都为0的话会被判断为Object类型，null的二进制表示全为0，自然前三位也是0，所以执行typeof时会返回"object"。 一个不恰当的例子，假设所有的Javascript对象都是16位的，也就是有16个0或1组成的序列，猜想如下：

```javascript
Array: 1000100010001000
null:  0000000000000000

typeof []  // "object"
typeof null // "object"
```

因为Array和null的前三位都是000。为什么Array的前三位不是100?因为二进制中的“前”一般代表低位， 比如二进制00000011对应十进制数是3，它的前三位是011。

### 3.instanceof

`instanceof`的语法：

```javascript
object instanceof constructor
// 等同于
constructor.prototype.isPrototypeOf(object)
```

- object： 要检测的对象
- constructor：某个构造函数

`instanceof`的代码实现。

```javascript
function instanceof(L, R) { //L是表达式左边，R是表达式右边
    const O = R.prototype;
    L = L.__proto__;
    while(true) {
        if (L === null)
            return false;
        if (L === O) // 这里重点：当 L 严格等于 0 时，返回 true 
            return true;
        L = L.__proto__;
    }
}

```

`instanceof`原理： 检测 `constructor.prototype`是否存在于参数 object的 原型链上。`instanceof` 查找的过程中会遍历`object`的原型链，直到找到 `constructor` 的 `prototype` ,如果查找失败，则会返回`false`，告诉我们，`object` 并非是 `constructor` 的实例。

### 4.Object.prototype.toString.call( )

#### 4.1Object.prototype.toString() 的调用

对于 `Object.prototype.toString()` 方法，会返回一个形如 `"[object XXX]"` 的字符串。

如果对象的 `toString()` 方法未被重写，就会返回如上面形式的字符串。

在ECMAScript中，**Object**类型的每个实例都有toString()方法，返回对象的字符串表示，所以每个实例化的对象都可以调用toString()方法。

```javascript
({}).toString();     // => "[object Object]"
Math.toString();     // => "[object Math]"
```

我们顺着原型链，obj => obj.*proto* => Object.prototype，可以发现，toString()方法是定义在Object的原型对象Object.prototype上的，这样Object的每个实例化对象都可以共享Object.prototype.toString()方法。

如果不通过原型链查找，怎么直接调用Object.prototype.toString()方法呢？

```javascript
Object.prototype.toString();
<!--"[object Object]"-->
```

上述的代码中toString()的调用和obj对象没有关系，但还是得到了同样的结果？这是因为Object.prototype也是对象，所以返回了对象的字符串表示！

通过obj对象调用Object.prototype.toString()方法的正确方式如下所示：

```javascript
Object.prototype.toString.call/apply(obj);
<!--"[object Object]"-->
```

我们可以说，**Object类是所有子类的父类**

```javascript
Object instanceof Object; //true
Function instanceof Object; //true
Array instanceof Object; //true
String instanceof Object; //true
Boolean instanceof Object; //true
Number instanceof Object; //true
```

上面提到的定义在Object.prototype上的toString()方法，可以说是最原始的toString()方法了，其他类型都或多或少**重写了toString()方法**，导致不同类型的对象调用toString()方法产生返回值各不相同。

#### 4.2不同类型的“对象”调用toString()方法，返回值有什么不同之处

1. 对象object（Object类）

> toString()：返回对象的字符串表示

```
var obj = {a: 1};
obj.toString();//"[object Object]"
Object.prototype.toString.call(obj);//"[object Object]"

```

这里我们思考一个问题，任何对象object都可以通过this绑定调用Object.prototype.toString()方法吗？答案是可以，结果如下

```
Object.prototype.toString.call({});
<!--"[object Object]"-->
Object.prototype.toString.call([]);
<!--"[object Array]"-->
Object.prototype.toString.call(function(){});
<!--"[object Function]"-->
Object.prototype.toString.call('');
<!--"[object String]"-->
Object.prototype.toString.call(1);
<!--"[object Number]"-->
Object.prototype.toString.call(true);
<!--"[object Boolean]"-->
Object.prototype.toString.call(null);
<!--"[object Null]"-->
Object.prototype.toString.call(undefined);
<!--"[object Undefined]"-->
Object.prototype.toString.call();
<!--"[object Undefined]"-->
Object.prototype.toString.call(new Date());
<!--"[object Date]"-->
Object.prototype.toString.call(/at/);
<!--"[object RegExp]"-->

```

从上述代码可以看到，因为Object是所有子类的父类，所以任何类型的对象object都可以通过this绑定调用Object.prototype.toString()方法，返回该对象的字符串表示

2.数组array（Array类）

> toString()：返回由数组中每个值的字符串形式拼接而成的一个以逗号分隔的字符串

```
var array = [1, 's', true, {a: 2}];
array.toString();//"1,s,true,[object Object]"
Array.prototype.toString.call(array);//"1,s,true,[object Object]"

```

这里我们同样思考上述问题，非数组对象也可以通过this绑定调用Array.prototype.toString()方法吗？答案是可以，结果如下

```
Array.prototype.toString.call({});
<!--"[object Object]"-->
Array.prototype.toString.call(function(){})
<!--"[object Function]"-->
Array.prototype.toString.call(1)
<!--"[object Number]"-->
Array.prototype.toString.call('')
<!--"[object String]"-->
Array.prototype.toString.call(true)
<!--"[object Boolean]"-->
Array.prototype.toString.call(/s/)
<!--"[object RegExp]"-->

Array.prototype.toString.call();
<!--Cannot convert undefined or null to object at toString-->
Array.prototype.toString.call(undefined);
Array.prototype.toString.call(null);

```

从上述代码中我们可以发现，数组对象通过this绑定调用Array.prototype.toString()方法，返回数组值的字符串拼接，但是非数组对象通过this绑定调用Array.prototype.toString()方法，返回的是该对象的字符串表示，另外null和undefined不可以通过绑定调用Array.prototype.toString()方法。

3.函数function（Function类）

> toString()：返回函数的代码

```
function foo(){
    console.log('function');
};
foo.toString();
<!--"function foo(){-->
<!--    console.log('function');-->
<!--}"-->
Function.prototype.toString.call(foo);
<!--"function foo(){-->
<!--    console.log('function');-->
<!--}"-->

```

此处，我们还需要注意到一个问题，上述我们提到的所有“类”，本质上都是构造函数，所以调用toString()方法返回的都是函数代码。

```
Object.toString();
//"function Object() { [native code] }"
Function.toString();
//"function Function() { [native code] }"
Array.toString();
//"function Array() { [native code] }"
....

```

另外，我们再考虑一下上述提到的问题，非函数对象也可以通过this绑定调用Array.prototype.toString()方法吗？答案是不可以，结果如下

```
Function.prototype.toString.call({});
<!--Function.prototype.toString requires that 'this' be a Function-->

```

另外，通过对其他Object子类的测试，除了上述提到的Object和Array两种情况，其他类型都不支持非自身实例通过this绑定调用该Object子类原型对象上的toString()方法，这说明它们在重写toString()方法时，明确限定了调用该方法的对象类型，非自身对象实例不可调用。所以，一般我们只使用Object.prototype.toString.call/apply()方法。

4.日期（Date类）

> toString()：返回带有时区信息的日期和时间

Date类型只有构造形式，没有字面量形式

```
var date = new Date();
date.toString();
//"Fri May 11 2018 14:55:43 GMT+0800 (中国标准时间)"
Date.prototype.toString.call(date);
//"Fri May 11 2018 14:55:43 GMT+0800 (中国标准时间)"

```

5.正则表达式（RegExp类）

> toString()：返回正则表达式的字面量

```
var re = /cat/g;
re.toString();// "/cat/g"
RegExp.prototype.toString.call(re);// "/cat/g"

```

6.基本包装类型（Boolean/Number/String类）

ECMAScript提供了三个特殊的引用类型Boolean、Number、String，它们具有与各自基本类型相应的特殊行为。

以String类型为例简单说一下

```
var str = 'wangpf';
str.toString();//"wangpf"

```

关于上述代码存在疑问，首先我定义了一个基本类型的字符串变量str，它不是对象，但为什么可以调用toString()方法呢，另外，toString()方法又是哪里来的呢？

我们先看一下str和strObject的区别：

```
var str = 'I am a string';
typeof str; //"string"
str instanceof String; //false

var strObject = new String('I am a string');
typeof strObject; //"object"
strObject instanceof String; //true
strObject instanceof Object; //true

```

原来，由于String基本包装类型的存在，在必要的时候JS引擎会把字符串字面量转换成一个String对象，从而可以执行访问属性和方法的操作，具体过程如下所示：

```
（1）创建一个String类型的实例；
（2）在实例上调用指定的方法；
（3）销毁这个实例。

```

因此调用toString()方法的过程如下所示：

```
var strObject = new String('wangpf');
strObject.toString(); //'wangpf'
strObject = null;

```

注意，上述代码是JS引擎自动执行的，你无法访问strObject对象，它只存在于代码的执行瞬间，然后立即销毁，所以我们无法再运行时给基本类型添加属性和方法，除非直接通过new显示调用基本包装类型创建对象，但我们不建议！！！

7.字符串string（String类）

> toString()：返回字符串的一个副本

```
var str = "a";
str.toString(); //"a"
String.prototype.toString.call(str); //"a"

```

8.数值number（Number类）

> toString()：返回字符串形式的数值

```
var num = 520;
num.toString(); //"520"
Number.prototype.toString.call(num); //"520"

```

9.布尔值boolean（Boolean类）

> toString()：返回字符串"true"或"false"

```
var boo = true;
boo.toString(); //"true"
Boolean.prototype.toString.call(boo); //"true"

```

10.null和undefined

> null和undefined没有相应的构造函数，所以它们没有也无法调用toString()方法，也就是说它们不能访问任何属性和方法，只是基本类型而已。

11.全局对象window（Window类）

全局对象Global可以说是ECMAScript中最特别的一个对象了，它本身不存在，但是会作为终极的“兜底儿对象”，所有不属于其他对象的属性和方法，最终都是它的属性和方法。

ECMAScript无法没有指出如何直接访问Global对象，但是Web浏览器将这个Global对象作为window对象的一部分加以实现了。所以上述提到的所有对象类型，如Object、Array、Function都是window对象的属性。

> toString(): 返回对象的字符串表示

```
window.toString();
<!--"[object Window]"-->
Window.prototype.toString.call(window);//这里其实有问题
<!--"[object Window]"-->

```

经查看，Winodw类并没有在Window.prototype原型对象上重写toString()方法，它会顺着原型链查找调用Object.prototype.toString()。

所以，任何对象object都可以通过this绑定调用Window.prototype.toString()方法，也就是调用Object.prototype.toString()方法，结果和Object类一样。

故上述代码实质上是

```
Object.prototype.toString.call(window);
<!--"[object Window]"-->
```

#### 4.3直接执行toString()方法

```javascript
Object.prototype.toString.call();
<!--"[object Undefined]"-->
Object.prototype.toString.call(undefined);
<!--"[object Undefined]"-->

```

所以直接调用toString()应该就是变相的undefined.toString()方法。