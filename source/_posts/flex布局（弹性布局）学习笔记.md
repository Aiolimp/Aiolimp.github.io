---
title: flex布局（弹性布局）学习笔记
date: 2021-04-23 16:57:49
top_img: https://static001.geekbang.org/resource/image/61/3a/61fa339e781c1e6e6668f94dd6ccc53a.jpg
cover: https://static001.geekbang.org/resource/image/61/3a/61fa339e781c1e6e6668f94dd6ccc53a.jpg
tags:
- CSS
categories:
- CSS
aplayer: true
---
# flex布局（弹性布局）学习笔记



**弹性容器：** 设置了display：flex;这个元素为弹性容器，弹性容器中的子元素会按照弹性布局进行排列。
**弹性子元素**：设置了display:flex;元素的子容器即为弹性子元素，但不包括孙子元素。

## flex容器属性：

**1.display属性**：

- display:flex  将对象作为弹性伸缩盒展示，相当于块级属性，有默认宽度100%
- display:inline-flex  将对象作为内联块级弹性伸缩盒展示，即行级元素，没有默认宽度

**2.flex-direction:指定容器的主轴方向，主轴默认为水平向右方向，项目排列的方向**

- flex-direction:row  默认值，主轴横向往右排列
- flex-direction:row-reverse  主轴横向往左排列
- flex-direction:column  垂直方向排列
- flex-direction:column-reverse 垂直方向反向排列

**3.flex-wrap:当容器子元素在容器中排列不下时，如何进行换行**

- flex-wrap:nowarp  默认不换行，压缩子元素
- flex-warp:warp  子元素换行，从上往下
- flex-warp:warp-reverse   子元素换行，从上往下

**4.justify-content：定义子元素在主轴上的对齐方式**

- justify-conten:flex-start  默认的是从主轴开始位置对齐
- justify-conten:flex-end  向主轴结束位置对齐
- justify-conten:center  向主轴的中心位置靠拢
- justify-conten:space-between  两端没有间距，中间间隔宽度一样平均分布
- justify-conten:space-around  平均分布，两端有间距，间距为中间间距的一半
- justify-conten:space-evenly 平均分布，两边有间距，间距和中间间距一样

**5.align-item：定义子元素在侧轴上的堆砌方式**

- align-item:flex-start  向侧轴开始位置靠拢
- align-item:flex-end 向侧轴结束位置靠拢
- align-item:center  向侧轴中心位置靠拢
- align-item:stretch  拉伸占满整个高度(默认,如果设置高度默认则无效)

**6.align-content：设置侧轴的多行分布**

- align-content: flex-start   多行内容往侧轴开端靠拢
- align-content: flex-end   多行内容往侧轴结束段靠拢
- align-content: center  多行内容居中
- align-content: space-between  平均分布,两边没有间距
- align-content: space-around  平均分布,两边间距为中间间距的一半
- align-content:spece-evenly  平均分布,两边间距和中间间距相等
- 注意：如果项目只有一行，该属性不起作用，只有使用了wrap后才起作用

**7.弹性布局剩余空间分布设置**

```javascript
   flex:1;//剩余空间分布设置，flex:1 则占用剩余空间整体一份
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200825223804852.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70#pic_center)

## flex项目属性：

**1.order 定义项目的排列顺序，取值为Interger，设置在子元素上，按照从小到大进行排序**

**2.align-self 单独设置子元素侧轴排布(可以覆盖父元素的align-items属性)**

-align-self：center  居中
-align-self：flex-start   靠近侧轴开端
-align-self：flex-end   靠近侧轴结束端
-align-self：stretch  拉伸
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200825223828824.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70#pic_center)