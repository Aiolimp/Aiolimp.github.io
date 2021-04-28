---
title: VUE hookEvent监听组件生命周期
date: 2021-04-14 16:57:49
top_img: https://user-gold-cdn.xitu.io/2019/6/13/16b51305d9427161?imageView2/1/w/1304/h/734/q/85/format/webp/interlace/1
cover: https://user-gold-cdn.xitu.io/2019/6/13/16b51305d9427161?imageView2/1/w/1304/h/734/q/85/format/webp/interlace/1
tags:
- VUE
categories:
- VUE
---
**1.内部监听生命周期函数**

<!-- more -->

在Vue组件中，可以用过$on,$once去监听所有的生命周期钩子函数，如监听组件的updated钩子函数可以写成 this.$on('hook:updated', () => {})


```javascript
export default {
  mounted() {
    this.chart = echarts.init(this.$el)
    // 请求数据，赋值数据 等等一系列操作...

    // 监听窗口发生变化，resize组件
    window.addEventListener('resize', this.$_handleResizeChart)
    // 通过hook监听组件销毁钩子函数，并取消监听事件
    this.$once('hook:beforeDestroy', () => {
      window.removeEventListener('resize', this.$_handleResizeChart)
    })
  },
  updated() {},
  created() {},
  methods: {
    $_handleResizeChart() {
      // this.chart.resize()
    }
  }
}
```
**2. 外部监听生命周期函数**

```javascript
<template>
  <!--通过@hook:updated监听组件的updated生命钩子函数-->
  <!--组件的所有生命周期钩子都可以通过@hook:钩子函数名 来监听触发-->
  <custom-select @hook:updated="$_handleSelectUpdated" />
</template>
<script>
import CustomSelect from '../components/custom-select'
export default {
  components: {
    CustomSelect
  },
  methods: {
    $_handleSelectUpdated() {
      console.log('custom-select组件的updated钩子函数被触发')
    }
  }
}
</script>

```
