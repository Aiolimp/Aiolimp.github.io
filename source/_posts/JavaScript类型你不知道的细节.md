---
title: JavaScript类型你不知道的细节
date: 2021-07-06 16:57:49
top_img: https://static001.geekbang.org/resource/image/9f/28/9f68cbdfd275739a1cd3a4dfa85ead28.jpg
cover: https://static001.geekbang.org/resource/image/9f/28/9f68cbdfd275739a1cd3a4dfa85ead28.jpg
tags:
- JavaScript
categories:
- JavaScript
---

JavaScript 类型对每个前端程序员来说，几乎都是最为熟悉的概念了。但是你真的很了解它们吗？
<!-- more -->
1. **为什么有的编程规范要求用 void 0 代替 undefined？** 

   ​      Undefined 类型表示未定义，它的类型只有一个值，就是 undefined。任何变量在赋值前是 Undefined 类型、值为 undefined，一般我们可以用全局变量 undefined（就是名为 undefined 的这个变量）来表达这个值，或者 void 运算来把任意一个表达式变成 undefined 值。

   ​       但是呢，因为 JavaScript 的代码 undefined 是一个变量，而并非是一个关键字，这是 JavaScript 语言公认的    设计失误之一，所以，我们为了避免无意中被篡改，建议使用 void 0 来获取 undefined 值。

   ​        Undefined 跟 Null 有一定的表意差别，Null 表示的是：“定义了但是为空”。所以，在实际编程时，我们一般不会把变量赋值为 undefined，这样可以保证所有值为 undefined 的变量，都是从未赋值的自然状态

   。Null 类型也只有一个值，就是 null，它的语义表示空值，与 undefined 不同，null 是 JavaScript 关键字，所以在任何代码中，你都可以放心用 null 关键字来获取 null 值。 

2. **字符串有最大长度吗？** 

   ​       String 用于表示文本数据。String 有最大长度是 2^53 - 1，这在一般开发中都是够用的，但是有趣的是，这个所谓最大长度，并不完全是你理解中的字符数。

   ​      因为 String 的意义并非“字符串”，而是字符串的 UTF16 编码，我们字符串的操作 charAt、charCodeAt、length 等方法针对的都是 UTF16 编码。所以，字符串的最大长度，实际上是受字符串的编码长度影响的。

   >现行的字符集国际标准，字符是以 Unicode 的方式表示的，每一个 Unicode 的码点表示一个字符，理论上，Unicode 的范围是无限的。UTF 是 Unicode 的编码方式，规定>了码点在计算机中的表示方法，常见的有 UTF16 和 UTF8。 Unicode 的码点通常用 U+??? 来表示，其中 ??? 是十六进制的码点值。 0-65536（U+0000 - U+FFFF）的码点被称为基本字符区域（BMP）。

   ​      JavaScript 中的字符串是永远无法变更的，一旦字符串构造出来，无法用任何方式改变字符串的内容，所以字符串具有值类型的特征。JavaScript 字符串把每个 UTF16 单元当作一个字符来处理，所以处理非 BMP（超出 U+0000 - U+FFFF 范围）的字符时，你应该格外小心。JavaScript 这个设计继承自 Java，最新标准中是这样解释的，这样设计是为了“性能和尽可能实现起来简单”。因为现实中很少用到 BMP 之外的字符。 

3. **0.1 + 0.2 不是等于 0.3 么？为什么 JavaScript 里不是这样的？** 

   ​        Number 类型表示我们通常意义上的“数字”。这个数字大致对应数学中的有理数，当然，在计算机中，我们有一定的精度限制。JavaScript 中的 Number 类型有 18437736874454810627(即 2^64-2^53+3) 个值。 

   ​        JavaScript 中的 Number 类型基本符合 IEEE 754-2008 规定的双精度浮点数规则，但是 JavaScript 为了表达几个额外的语言场景（比如不让除以 0 出错，而引入了无穷大的概念），规定了几个例外情况： 

- NaN，占用了 9007199254740990，这原本是符合 IEEE 规则的数字；

- Infinity，无穷大；-Infinity，

- 负无穷大。 

  ​         另外，值得注意的是，JavaScript 中有 +0 和 -0，在加法类运算中它们没有区别，但是除法的场合则需要特别留意区分，“忘记检测除以 -0，而得到负无穷大”的情况经常会导致错误，而区分 +0 和 -0 的方式，正是检测 1/x 是 Infinity 还是 -Infinity。 

  ​        同样根据浮点数的定义，非整数的 Number 类型无法用 ==（=== 也不行） 来比较，一段著名的代码，这也正是我们第三题的问题，为什么在 JavaScript 中，0.1+0.2 不能 =0.3： 

```javascript
console.log( 0.1 + 0.2 == 0.3);//false
```

​           这里输出的结果是 false，说明两边不相等的，**这是浮点运算的特点，浮点数运算的精度问题导致等式左右的结果并不是严格相等，所有语言都要进行二进制编码，不同进制数进行转换的时候都会有精度的问题，相差了个微小的值**。所以实际上，这里错误的不是结论，而是比较的方法，正确的比较方法是使用 JavaScript 提供的最小精度值：（Number.EPSILON） 

```javascript
console.log( Math.abs(0.1 + 0.2 - 0.3) <= Number.EPSILON);
```

 检查等式左右两边差的绝对值是否小于最小精度，才是正确的比较浮点数的方法。这段代码结果就是 true 了。 





