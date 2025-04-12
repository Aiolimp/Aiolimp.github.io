---
title: JS中的深拷贝和浅拷贝
date: 2021-07-03 16:57:49
top_img: https://static001.geekbang.org/resource/image/f7/f8/f7c1822abb4382896b9b4d3530b02ff8.jpg
cover: https://static001.geekbang.org/resource/image/f7/f8/f7c1822abb4382896b9b4d3530b02ff8.jpg
tags:
- JavaScript
categories:
- JavaScript
---



# **JS中的深拷贝和浅拷贝**

如何区分深拷贝与浅拷贝，简单点来说，就是假设B复制了A，当修改A时，看B是否会发生变化，如果B也跟着变了，说明这是浅拷贝如果B没变，那就是深拷贝。

### **浅拷贝：**

```javascript
let a=[0,1,2,3,4],
    b=a;
console.log(a===b);
a[0]=1;
console.log(a,b);
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200923165832394.png#pic_center)
此时，修改数组a，数组b也跟着变了。
**object.assign(target,source)**
Object.assign 方法只复制源对象中可枚举的属性和对象自身的属性。
如果目标对象中的属性具有相同的键，则属性将被源中的属性覆盖。后来的源的属性将类似地覆盖早先的属性。
**使用扩展运算符**

### **实现深拷贝**

深拷贝本身只针对较为复杂的object类型数据。

**用递归去复制所有层级属性**

```javascript
function deepClone(obj){
    let objClone = Array.isArray(obj)?[]:{};
    if(obj && typeof obj==="object"){
        for(key in obj){
            if(obj.hasOwnProperty(key)){
                //判断ojb子元素是否为对象，如果是，递归复制
                if(obj[key]&&typeof obj[key] ==="object"){
                    objClone[key] = deepClone(obj[key]);
                }else{
                    //如果不是，简单复制
                    objClone[key] = obj[key];
                }
            }
        }
    }
    return objClone;
}    
let a=[1,2,3,4],
    b=deepClone(a);
a[0]=2;
console.log(a,b);
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200923170144196.png#pic_center)
此时，b不受a的影响。
**利用JSON.parse(JSON.stringify())**

```javascript
function deepCopy(o) {
    return JSON.parse(JSON.stringify(o))
}

var c = {
    age: 1,
    name: undefined,
    sex: null,
    tel: /^1[34578]\d{9}$/,
    say: () => {
        console.log('hahha')
    }
}
// { age: 1, sex: null, tel: {} }
```

注意：这种拷贝方法不可以拷贝一些特殊的属性（例如正则表达式，undefine，function）
**使用Jquery的extend方法**。

$.extend( [deep ], target, object1 [, objectN ] )

**deep**表示是否深拷贝，为true为深拷贝，为false，则为浅拷贝

**target** Object类型 目标对象，其他对象的成员属性将被附加到该对象上。

**object1  objectN**可选。 Object类型 第一个以及第N个被合并的对象。 

```javascript
let a=[0,1,[2,3],4],
    b=$.extend(true,[],a);
a[0]=1;
a[2][0]=1;
console.log(a,b);//需要导入JQ库
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200923170428871.png#pic_center)


