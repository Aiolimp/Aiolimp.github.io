---
title: 前端图片转Base64编码
date: 2021-04-25 16:57:49
top_img: https://static001.geekbang.org/resource/image/1d/6b/1d57a4fde1c266da2e6a8e90808f5b6b.jpg
cover: https://static001.geekbang.org/resource/image/1d/6b/1d57a4fde1c266da2e6a8e90808f5b6b.jpg
tags:
- Base64
categories:
- Base64
aplayer: true
---

## Base64编码

### 什么是Base64编码

我们所看到的网页上的每一个图片，都是需要消耗一个 http 请求下载而来的，不管如何，图片的下载始终都要向服务器发出请求，要是图片的下载不用向服务器发出请求，而可以随着 HTML 的下载同时下载到本地那就太好了，而 base64 正好能解决这个问题。  **图片的 base64 编码就是可以将一副图片数据编码成一串字符串，使用该字符串代替图像地址。**

如图所示：google搜索框右侧的搜索小图标使用的就是base64编码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210412095816792.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0Fpb2xpbXA=,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/2021041209582719.png)
在css里的写法：

```javascript
#fkbx-spch, #fkbx-hspch {
  background: url(data:image/gif;base64,R0lGODlhHAAmAKIHAKqqqsvLy0hISObm5vf394uLiwAAAP///yH5B…EoqQqJKAIBaQOVKHAXr3t7txgBjboSvB8EpLoFZywOAo3LFE5lYs/QW9LT1TRk1V7S2xYJADs=) no-repeat center;
}
```

在html代码img标签里的写法：

```javascript
<img src="data:image/gif;base64,R0lGODlhHAAmAKIHAKqqqsvLy0hISObm5vf394uLiwAAAP///yH5B…EoqQqJKAIBaQOVKHAXr3t7txgBjboSvB8EpLoFZywOAo3LFE5lYs/QW9LT1TRk1V7S2xYJADs=">
```

### 为什么要是用Base64编码？

图片的 base64 编码可以算是前端优化的一环。效益虽小，但却缺能积少成多。

说到这里，不得不提的是 CssSprites 技术，后者也是为了减少 http 请求，而将页面中许多细小的图片合并为一张大图。那么图片的 base64 编码和 CssSprites 有什么异同，又该如何取舍呢？

所以，在这里要明确使用 base64 的一个前提，那就是被 base64 编码的图片足够尺寸小。以CSDN的 logo 为例：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210412100859257.png)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210412100859272.jpg)
如图所示，CSDN的 Logo 只有 3.27KB，已经很小了，但是如果将其制作转化成 base64 编码，生成的 base64 字符串编码足足有 4406 个,也就是说，图片被编码之后，生成的字符串编码大小一般而言都会比原文件稍大一些。即便 base64 编码能够被 gzip 压缩，压缩率能达到 50% 以上，想象一下，一个元素的 css 样式编写居然超过了 2000个 字符，那对 css 整体的可读性将会造成十分大的影响，代码的冗余使得在此使用 base64 编码将得不偿失。
**如果图片足够小且因为用处的特殊性无法被制作成雪碧图（CssSprites），在整个网站的复用性很高且基本不会被更新。**

那么此时使用 base64 编码传输图片就可谓好钢用在刀刃上，思前想后，符合这个规则的，有一个是我们经常会遇到的，就是页面的背景图 background-image 。在很多地方，我们会制作一个很小的图片大概是几px * 几px，然后平铺它页面当背景图。因为是背景图的缘故，所以无法将它放入雪碧图，而它却存在网站的很多页面，这种图片往往只有几十字节，却需要一个 http 请求，十分不值得。那么此时将它转化为 base64 编码，恰到好处。
只有 50 字节的2*2的的背景图。将其转化成 base64 编码，只有 100 多个字符，相比一个 http 请求，这种转换无疑更值得推崇。

### CssSprites与Base64编码　　

使用CssSprites合并为一张大图：

 - 页面具有多种风格，需要换肤功能，可使用CssSprites
 - 网站已经趋于完美，不会再三天两头的改动（例如button大小、颜色等）
- 使用时无需重复图形内容
- 没有 Base64 编码成本，降低图片更新的维护难度。（但注意 Sprites 同时修改 css 和图片某些时候可能造成负担）
- 不会增加 CSS 文件体积

使用base64直接把图片编码成字符串写入CSS文件：

- 无额外请求
- 对于极小或者极简单图片
- 可像单独图片一样使用，比如背景图片重复使用等
- 没有跨域问题，无需考虑缓存、文件头或者cookies问题  

### 更便捷的将图片转化为Base64编码　　

利用 Chrome 浏览器。在 chrome 下新建一个窗口，然后把要转化的图片直接拖入浏览器，打开控制台，点 Source，如下图所示，点击图片，右侧就会显示该图片的 base64 编码，是不是很方便。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210412101429264.png)

### 一些误区

**1. 使用 Base64 不代表性能优化**

使用 Base64 的好处是能够减少一个图片的 HTTP 请求，然而，与之同时付出的代价则是 CSS 文件体积的增大。

而 CSS 文件体积的增大意味着什么呢？意味着 CRP 的阻塞。

**CRP**（Critical Rendering Path，关键渲染路径）：当浏览器从服务器接收到一个HTML页面的请求时，到屏幕上渲染出来要经过很多个步骤。浏览器完成这一系列的运行，或者说渲染出来我们常常称之为“关键渲染路径”。

通俗而言，就是图片不会导致关键渲染路径的阻塞，而转化为 Base64 的图片大大增加了 CSS 文件的体积，CSS 文件的体积直接影响渲染，导致用户会长时间注视空白屏幕。HTML 和 CSS 会阻塞渲染，而图片不会。

 **2. 页面解析 CSS 生成的 CSSOM 时间增加**

Base64 跟 CSS 混在一起，大大增加了浏览器需要解析CSS树的耗时。
**CSSOM** 生成过程大致是，解析 HTML ，在文档的 head 部分遇到了一个 link 标记，该标记引用一个外部 CSS 样式表，下载该样式表后根据上述过程生成 CSSOM 树。
**CSSOM 阻止任何东西渲染**，（意味着在CSS没处理好之前所有东西都不会展示），而如果CSS文件中混入了Base64，那么（因为文件体积的大幅增长）解析时间会增长到十倍以上。