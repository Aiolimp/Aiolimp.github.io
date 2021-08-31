---
title: Vue实现函数防抖和节流
date: 2021-06-15 16:57:49
top_img: https://static001.geekbang.org/resource/image/73/2a/737fb9f94c18a26a875c27169222b82a.jpg
cover: https://static001.geekbang.org/resource/image/73/2a/737fb9f94c18a26a875c27169222b82a.jpg
tags:
- VUE
categories:
- VUE
---

**vue实现函数防抖和节流**

<!--more-->

**效果：**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20201111152317390.gif#pic_center)

## 一、在utils文件加下创建globalFunction.js文件

```javascript
export default {
    //防抖
    debounce(func, wait) {
        let timeout;
        return function () {
            const context = this;
            const args = [...arguments];
            if (timeout) clearTimeout(timeout);
            timeout = setTimeout(() => {
                func.apply(context, args)
            }, wait)
        }
    },
    //节流
    throttle(func, wait) {
        let timeout;
        return function () {
            let context = this;
            let args = arguments;
            if (!timeout) {
                timeout = setTimeout(() => {
                    timeout = null;
                    func.apply(context, args)
                }, wait)
            }

        }
    }
}
```
## 二、在单页面中进行导入

```javascript
import globalFunction from "@/utils/globalFunction.js";
```
## 三、调用方法时注意！
函数调用避免使用箭头函数，以免this指向globalFunction

```javascript
 methods: {
    //debChange和thrChange为点击事件
    debChange:globalFunction.debounce(function(){
      this.debounceValue2 = this.debounceValue1;
    }, 1000),

    thrChange: globalFunction.throttle(function() {
      this.throttleValue2 = this.throttleValue1;
    }, 1000),
  },
```
**完整代码：**

```javascript
<template>
  <div>
    <a-page-header
      title="返回"
      sub-title="| 防抖节流"
      @back="gohome()"
      style="padding: 5px"
      backIcon="<"
    />
    <div style="display: flex">
      <a-input
        v-model="value"
        style="width: 200px; margin-top: 45px"
        addonAfter="未做处理"
      ></a-input>
      <span> 响应数据：{{ value }}</span>
    </div>
    <div style="display: flex">
      <a-input
        v-model="debounceValue1"
        style="width: 200px; margin-top: 45px"
        addonAfter="防抖处理"
        @change="debChange"
      ></a-input>
      <span> 响应数据：{{ debounceValue2 }}</span>
    </div>
    <div style="display: flex">
      <a-input
        v-model="throttleValue1"
        style="width: 200px; margin-top: 45px"
        addonAfter="节流处理"
        @change="thrChange"
      ></a-input>
      <span> 响应数据：{{ throttleValue2 }}</span>
    </div>
  </div>
</template>
<script>
import globalFunction from "@/utils/globalFunction.js";
export default {
  data() {
    return {
      value: "",
      debounceValue1: "",
      debounceValue2: "",
      throttleValue1: "",
      throttleValue2: "",
    };
  },
  created() {},
  methods: {
    gohome() {
      this.$router.push({ path: "/employ" });
    },
    debChange: globalFunction.debounce(function () {
      this.debounceValue2 = this.debounceValue1;
    }, 1000),

    thrChange: globalFunction.throttle(function () {
      this.throttleValue2 = this.throttleValue1;
    }, 1000),
  },
};
</script>
```
