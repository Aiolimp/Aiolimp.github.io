---
title: three.js物体基本操作
date: 2022-11-01 16:57:49
top_img: https://static001.geekbang.org/resource/image/37/e4/37fbe168a471e8ee1267fd741d096fe4.jpg
cover: https://static001.geekbang.org/resource/image/37/e4/37fbe168a471e8ee1267fd741d096fe4.jpg
tags:
- Three.js
categories:
- Three.js
---

## three.js物体基本操作
- [一、本地搭建Threejs官方文档网站](#一本地搭建threejs官方文档网站)
- [二、使用parcel搭建three.js开发环境](#二使用parcel搭建threejs开发环境)
- [三、Threejs渲染第一个场景和物体](#三threejs渲染第一个场景和物体)
- [四、Threejs轨道控制器查看物体](#四threejs轨道控制器查看物体)
- [五、Threejs控制物体移动](#五threejs控制物体移动)
- [六、Threejs添加坐标轴辅助器](#六threejs添加坐标轴辅助器)
- [七、Threejs物体缩放与旋转](#七threejs物体缩放与旋转)
- [八、Threejs正确处理动画运动](#八threejs正确处理动画运动)
- [九、Threejs中Clock跟踪时间处理动画](#九threejs中clock跟踪时间处理动画)
- [十、Gsap动画库使用](#十gsap动画库使用)
- [十一、画布自适应屏幕大小与全屏](#十一画布自适应屏幕大小与全屏)
- [十二、应用图形用户界面更改变量](#十二应用图形用户界面更改变量)

## 一、本地搭建Threejs官方文档网站

因为Three.js官网是国外的服务器，所以为了方便学习和快速的查阅文档，我们可以自己搭建Three.js官网和文档，方便随时查看案例和文档内容进行学习。

**1、首先进入threejs库GitHub地址：https://github.com/mrdoob/three.js**

**2、下载完整代码**

![img](https://i0.hdslb.com/bfs/article/48fcf222da2afa75e20a3e6e5292a854bebabb01.png@942w_474h_progressive.webp)

**3、项目文件解压缩**

![img](https://i0.hdslb.com/bfs/article/064d111714e45ec2f3ca076377539f869641ff9f.png@942w_998h_progressive.webp)

**4、命令行安装依赖**

一般安装可以用npm、yarn等包管理工具，课程以yarn举例，如果没有安装可以用npm install yarn -g进行安装。

```
yarn install
```

![img](https://i0.hdslb.com/bfs/article/629b6eb8813a49a70866a39614d525bfbf32e241.png@942w_345h_progressive.webp)

**5、启动项目**

```
yarn start
```

![img](https://i0.hdslb.com/bfs/article/69aca9f2b22bd1b2a473e82fc9911888210026ee.png@942w_321h_progressive.webp)


浏览器访问即可：http://localhost:8080

**6、文档目录介绍**

![img](https://i0.hdslb.com/bfs/article/f06ea28ece3bf4966884d12f31ae3ef2fa21b198.png@942w_639h_progressive.webp)

**build目录：**

![img](https://i0.hdslb.com/bfs/article/2eaf352481ba76ff7b60cf55e0b19074c99d0556.png@654w_333h_progressive.webp)

**docs文档：**

选择中文，查看中文文档。

![img](https://i0.hdslb.com/bfs/article/b3ac7dbfef00f80cf43c0875b0706a9a5ee601b8.png@942w_569h_progressive.webp)

**examples案例：**

![img](https://i0.hdslb.com/bfs/article/d0bf8aea2caa58cd11ae82a9aa0a8d2029c097fb.png@942w_483h_progressive.webp)

可以通过网址，找到具体的案例代码，如此处的文件名称是：webgl_animation_keyframes。因此可以在文件夹找到对应的代码文件

![img](https://i0.hdslb.com/bfs/article/ee61b8caef44caf66dc54719ed3ddac4480f39ff.png@942w_288h_progressive.webp)

**editor目录：**

官方提供的可视化编辑器，可以直接导入模型，修改材质，添加光照效果等等。

![img](https://i0.hdslb.com/bfs/article/2a94f42e1f19fdbf31bde24cd3640926ae03ca65.png@942w_410h_progressive.webp)

## 二、使用parcel搭建three.js开发环境

为了方便模块化进行three.js项目的学习和开发，又不用学习太多的配置，增加学习成本，所以就使用Parcel这个web应用打包工具。

Parcel官网：https://v2.parceljs.cn/getting-started/webapp/

### 1、安装

在开始之前，您需要安装 Node 和 Yarn 或 npm，并为您的项目创建一个目录。然后，使用 Yarn 将 Parcel 安装到您的应用程序中：

```
yarn add --dev parcel
```

或者在使用 npm 运行时：

```
npm install --save-dev parcel
```

### 2、项目设置

现在已经安装了 Parcel，让我们为我们的应用程序创建一些源文件。Parcel 接受任何类型的文件作为入口点，但 HTML 文件是一个很好的起点。Parcel 将从那里遵循您的所有依赖项来构建您的应用程序。

创建src文件夹，并且创建index.html文件

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <link rel="stylesheet" href="./assets/css/style.css" />
  </head>
  <body>
    <script src="./main/main.js" type="module"></script>
  </body>
</html>
```

![img](https://i0.hdslb.com/bfs/article/a1ca45e07ba3ee682df2e98d6e3c66e8b84c6335.png@942w_359h_progressive.webp)

### 设置1个css文件

```css
* {
  margin: 0;
  padding: 0;
}
body {
  background-color: skyblue;
} 
```

![img](https://i0.hdslb.com/bfs/article/41d22c3b03134b6e21022d2a639de44dd7d4e6b5.png@942w_462h_progressive.webp)

### 创建一个main.js

```js
import * as THREE from "three";

// console.log(THREE);

// 目标：了解three.js最基本的内容

// 1、创建场景
const scene = new THREE.Scene();

// 2、创建相机
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);

// 设置相机位置
camera.position.set(0, 0, 10);
scene.add(camera);

// 添加物体
// 创建几何体
const cubeGeometry = new THREE.BoxGeometry(1, 1, 1);
const cubeMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 });
// 根据几何体和材质创建物体
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);
// 将几何体添加到场景中
scene.add(cube);

// 初始化渲染器
const renderer = new THREE.WebGLRenderer();
// 设置渲染的尺寸大小
renderer.setSize(window.innerWidth, window.innerHeight);
// console.log(renderer);
// 将webgl渲染的canvas内容添加到body
document.body.appendChild(renderer.domElement);

// 使用渲染器，通过相机将场景渲染进来
renderer.render(scene, camera);
```

### 3、打包脚本

到目前为止，我们一直在parcel直接运行 CLI，但在您的package.json文件中创建一些脚本以简化此操作会很有用。我们还将设置一个脚本来使用该命令构建您的应用程序以进行生产。parcel build最后，您还可以使用该字段在一个地方声明您的条目source，这样您就不需要在每个parcel命令中重复它们。

package.json：

```json
{
  "name": "01-three_basic",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "dev": "parcel src/index.html",
    "build": "parcel build src/index.html"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "parcel": "^2.4.1"
  },
  "dependencies": {
    "dat.gui": "^0.7.9",
    "gsap": "^3.10.3",
    "three": "^0.139.2"
  }
} 
```

安装依赖package.json设置的依赖

```
yarn install
```

现在您可以运行yarn build以构建您的生产项目并yarn dev启动开发服务器。

```
yarn dev 
```

## 三、Threejs渲染第一个场景和物体

### 1 基本概念

三维的物体要渲染在二维的屏幕上。首先要创建一个场景来放置物体，那么最终怎么显示三维的内容，就应该找一个相机，将相机放在场景的某个位置，然后想要显示就要把相机拍的内容渲染出来。所以就引出三个基本概念：场景、相机、渲染器。

#### 1.1 场景

three.js创建场景非常的简单。

```js
const scene = new THREE.Scene();
```

#### 1.2 相机

three.js创建相机对象

```js
// 2、创建相机
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);

// 设置相机位置
camera.position.set(0, 0, 10); 
```


three.js里有几种不同的相机，在这里，我们使用的是**PerspectiveCamera**（透视摄像机）。

第一个参数是**视野角度（FOV**）。视野角度就是无论在什么时候，你所能在显示器上看到的场景的范围，它的单位是角度(与弧度区分开)。

第二个参数是**长宽比（aspect ratio**）。 也就是你用一个物体的宽除以它的高的值。比如说，当你在一个宽屏电视上播放老电影时，可以看到图像仿佛是被压扁的。

接下来的两个参数是**近截面**（near）和远截面（far）。 当物体某些部分比摄像机的**远截面**远或者比**近截面**近的时候，该这些部分将不会被渲染到场景中。或许现在你不用担心这个值的影响，但未来为了获得更好的渲染性能，你将可以在你的应用程序里去设置它。

下图椎体就是上面设置视野角度、长宽比、近截面和远截面的演示的相机透视椎体。

![img](https://i0.hdslb.com/bfs/article/11c2c2d27b322c07ee20b70018cd0df551eaea2e.png@597w_402h_progressive.webp)



#### 1.3 渲染器

接下来是渲染器。这里是施展魔法的地方。

```js
// 初始化渲染器
const renderer = new THREE.WebGLRenderer();
// 设置渲染的尺寸大小
renderer.setSize(window.innerWidth, window.innerHeight);
// console.log(renderer);
// 将webgl渲染的canvas内容添加到body
document.body.appendChild(renderer.domElement);

// 使用渲染器，通过相机将场景渲染进来
renderer.render(scene, camera);
```

除了创建一个渲染器的实例之外，我们还需要在我们的应用程序里设置一个渲染器的尺寸。比如说，我们可以使用所需要的渲染区域的宽高，来让渲染器渲染出的场景填充满我们的应用程序。因此，我们可以将渲染器宽高设置为浏览器窗口宽高。对于性能比较敏感的应用程序来说，你可以使用`setSize`传入一个较小的值，例如`window.innerWidth/2`和`window.innerHeight/2`，这将使得应用程序在渲染时，以一半的长宽尺寸渲染场景。

接下来将`renderer`（渲染器）的dom元素（renderer.domElement）添加到我们的HTML文档中。渲染器用来显示场景给我们看的<canvas>元素。

最后就是对将相机对场景进行拍照渲染啦。这一句就可以将画面渲染到canvas上显示出来

```js
renderer.render(scene, camera);
```

#### 1.4 加入立方体

要创建一个立方体，我们需要一个**BoxGeometry**（立方体）对象. 这个对象包含了一个立方体中所有的顶点（**vertices**）和面（**faces**）。

接下来，对于这个立方体，我们需要给它一个材质，来让它有颜色。这里我们使用的是**MeshBasicMaterial**。所有的材质都存有应用于他们的属性的对象。为了简单起见，我们只设置一个color属性，值为**0x00ff00**，也就是绿色。这里和CSS或者Photoshop中使用十六进制(**hex colors**)颜色格式来设置颜色的方式一致。

第三步，我们需要一个**Mesh**（网格）。 网格包含一个几何体以及作用在此几何体上的材质，我们可以直接将网格对象放入到我们的场景中，并让它在场景中自由移动。

默认情况下，当我们调用**scene.add()**的时候，物体将会被添加到**(0,0,0)**坐标。但将使得摄像机和立方体彼此在一起。为了防止这种情况的发生，我们只需要将摄像机稍微向外移动一些即可。

```js
// 添加物体
// 创建几何体
const cubeGeometry = new THREE.BoxGeometry(1, 1, 1);
const cubeMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 });
// 根据几何体和材质创建物体
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);
// 将几何体添加到场景中
scene.add(cube);
```

### 2 综合上述代码

**在前面创建的项目中的main.js文件写入代码**

```js
import * as THREE from "three";

// console.log(THREE);

// 目标：了解three.js最基本的内容

// 1、创建场景
const scene = new THREE.Scene();

// 2、创建相机
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);

// 设置相机位置
camera.position.set(0, 0, 10);
scene.add(camera);

// 添加物体
// 创建几何体
const cubeGeometry = new THREE.BoxGeometry(1, 1, 1);
const cubeMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 });
// 根据几何体和材质创建物体
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);
// 将几何体添加到场景中
scene.add(cube);

// 初始化渲染器
const renderer = new THREE.WebGLRenderer();
// 设置渲染的尺寸大小
renderer.setSize(window.innerWidth, window.innerHeight);
// console.log(renderer);
// 将webgl渲染的canvas内容添加到body
document.body.appendChild(renderer.domElement);

// 使用渲染器，通过相机将场景渲染进来
renderer.render(scene, camera);
```

效果演示：

![img](https://i0.hdslb.com/bfs/article/0b50a12934a7e62acc12251115c3448d85ffa1a7.png@704w_416h_progressive.webp)

## 四、Threejs轨道控制器查看物体

### 1 如何360度的查看立方体

使用控制控制器，让相机围绕立方体运动，就像地球围绕太阳一样运动，去观察立方体。

![img](https://i0.hdslb.com/bfs/article/46eb73b99d98da462440cad79aa34795a2294ef4.gif@489w_282h_progressive.webp)

#### 1.1 创建轨道控制器

```js
// 创建轨道控制器
const controls = new OrbitControls(camera, renderer.domElement);
```

**必须传入2个参数：**

1. 相机，让哪一个相机围绕目标运动。默认目标是原点。立方体在原点处。

2. 渲染的画布dom对象，用于监听鼠标事件控制相机的围绕运动。

#### 1.2 每一帧根据控制器更新画面

因为控制器监听鼠标事件之后，要根据鼠标的拖动，来控制相机围绕目标运动，并根据运动之后的效果，显示出画面来。为了保证画面流畅渲染，选择使用请求动画帧requestAnimationFrame，在屏幕渲染下一帧画面时触发回调函数来执行画面的渲染。

```js
function render() {
  //如果后期需要控制器带有阻尼效果，或者自动旋转等效果，就需要加入controls.update()
  controls.update()
  renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}

render();
```

##### 1.2.1 requestAnimationFrame

是HTML5的新特性，区别于`setTimeout`和`setInterval`。`requestAnimationFrame`比后两者精确，采用系统时间间隔，保持最佳绘制效率，不会因为间隔时间过短，造成过度绘制，增加开销；也不会因为间隔时间太长，使动画卡顿不流畅，让各种网页动画效果能够有一个统一的刷新机制，从而节省系统资源，提高系统性能，改善视觉效果。

`requestAnimationFrame`是由浏览器专门为动画提供的API，在运行时浏览器会自动优化方法的调用，并且如果页面不是激活状态下的话，动画会自动暂停，有效节省了CPU开销。

因此屏幕每一帧都刷新一次画面，就需要执行

```js
function render() {
  renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}

render();
```

### 2 综合上述代码

**在前面创建的项目中的main.js文件写入代码**

```js
import * as THREE from "three";
// 导入轨道控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

// console.log(THREE);

// 目标：使用控制器查看3d物体

// 1、创建场景
const scene = new THREE.Scene();

// 2、创建相机
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);

// 设置相机位置
camera.position.set(0, 0, 10);
scene.add(camera);

// 添加物体
// 创建几何体
const cubeGeometry = new THREE.BoxGeometry(1, 1, 1);
const cubeMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 });
// 根据几何体和材质创建物体
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);
// 将几何体添加到场景中
scene.add(cube);

// 初始化渲染器
const renderer = new THREE.WebGLRenderer();
// 设置渲染的尺寸大小
renderer.setSize(window.innerWidth, window.innerHeight);
// console.log(renderer);
// 将webgl渲染的canvas内容添加到body
document.body.appendChild(renderer.domElement);

// // 使用渲染器，通过相机将场景渲染进来
// renderer.render(scene, camera);

// 创建轨道控制器
const controls = new OrbitControls(camera, renderer.domElement);

// 添加坐标轴辅助器
const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper);

function render() {
  renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}

render();
```

效果演示：

![img](https://i0.hdslb.com/bfs/article/46eb73b99d98da462440cad79aa34795a2294ef4.gif@489w_282h_progressive.webp)

## 五、Threejs控制物体移动

### 1. 控制物体移动

![img](https://i0.hdslb.com/bfs/article/a35394deeabd8745cb7306363341f4d7e71fe513.gif@489w_282h_progressive.webp)

为了让物体移动起来。我们可以设置它的`position`属性进行位置的设置。

相机和立方体都是物体。每个物体都是1个对象。

在官方文档里，我们可以看到相机`camera`和物体`mesh`都继承`Object3D`类。所以`camera`、`mesh`都属于3d对象。从3d对象的官方文档里，我们可以找到`position`属性，并且该属性一个`vector3`对象。因此通过官方`vector3`类的文档，我们可以简单使用下面2种方式来修改`position`位置。

```js
//设置该向量的x、y 和 z 分量。
mesh.position.set(x,y,z);
//直接设置position的x,y,z属性
mesh.position.x = x;
mesh.position.y = y;
mesh.position.z = z;
```

官方文档：https://threejs.org/docs/index.html?q=vect#api/zh/math/Vector3

![img](https://i0.hdslb.com/bfs/article/daedc9f76ed4fc5800af96f60ff560843806e3ed.png@909w_251h_progressive.webp)

![img](https://i0.hdslb.com/bfs/article/daedc9f76ed4fc5800af96f60ff560843806e3ed.png@909w_251h_progressive.webp)

![img](https://i0.hdslb.com/bfs/article/b1d8877a6b77718e509e95c59ce4cf22cc7db0cc.png@798w_147h_progressive.webp)

#### 1.1 每一帧修改一点位置形成动画

例如，每一帧让立方体向右移动0.01，并且当位置大于5时，从0开始。那么可以这么设置。

```js
function render() {
  cube.position.x += 0.01;
  if (cube.position.x > 5) {
    cube.position.x = 0;
  }
  renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}

render();
```

### 2 综合上述代码

**在前面创建的项目中的main.js文件写入代码**

```js
import * as THREE from "three";
// 导入轨道控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

// console.log(THREE);

// 目标：控制3d物体移动

// 1、创建场景
const scene = new THREE.Scene();

// 2、创建相机
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);

// 设置相机位置
camera.position.set(0, 0, 10);
scene.add(camera);

// 添加物体
// 创建几何体
const cubeGeometry = new THREE.BoxGeometry(1, 1, 1);
const cubeMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 });
// 根据几何体和材质创建物体
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);

// 修改物体的位置
// cube.position.set(5, 0, 0);
cube.position.x = 3;

// 将几何体添加到场景中
scene.add(cube);

console.log(cube);

// 初始化渲染器
const renderer = new THREE.WebGLRenderer();
// 设置渲染的尺寸大小
renderer.setSize(window.innerWidth, window.innerHeight);
// console.log(renderer);
// 将webgl渲染的canvas内容添加到body
document.body.appendChild(renderer.domElement);

// // 使用渲染器，通过相机将场景渲染进来
// renderer.render(scene, camera);

// 创建轨道控制器
const controls = new OrbitControls(camera, renderer.domElement);

// 添加坐标轴辅助器
const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper);

function render() {
  cube.position.x += 0.01;
  if (cube.position.x > 5) {
    cube.position.x = 0;
  }
  renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}

render();
```

效果演示：

![img](https://i0.hdslb.com/bfs/article/a35394deeabd8745cb7306363341f4d7e71fe513.gif@489w_282h_progressive.webp)

## 六、Threejs添加坐标轴辅助器

### 1. 坐标轴辅助器

一般我们在开发阶段，添加物体和设置物体位置，都需要参考一下坐标轴，方便查看是否放置到对应位置。所以一般添加坐标轴辅助器来作为参考，辅助器简单模拟3个坐标轴的对象。红色代表 X 轴. 绿色代表 Y 轴. 蓝色代表 Z 轴。

```js
const axesHelper = new THREE.AxesHelper( 5 );
scene.add( axesHelper );
```

### 2. ArrowHelper箭头辅助器

**用于模拟方向的3维箭头对象**

```js
const dir = new THREE.Vector3( 1, 2, 0 );

//normalize the direction vector (convert to vector of length 1)
dir.normalize();

const origin = new THREE.Vector3( 0, 0, 0 );
const length = 1;
const hex = 0xffff00;

const arrowHelper = new THREE.ArrowHelper( dir, origin, length, hex );
scene.add( arrowHelper )
```

**构造函数**
ArrowHelper(dir : Vector3, origin : Vector3, length : Number, hex : Number, headLength : Number, headWidth : Number )

`dir` -- 基于箭头原点的方向. 必须为单位向量.
`origin `-- 箭头的原点.
`length `-- 箭头的长度. 默认为 1.
`hex `-- 定义的16进制颜色值. 默认为 0xffff00.
`headLength `-- 箭头头部(锥体)的长度. 默认为箭头长度的0.2倍(0.2 * length).
`headWidth `-- The width of the head of the arrow. Default is 0.2 * headLength. 

## 七、Threejs物体缩放与旋转

### 1. scale设置缩放

因为物体的scale属性是vector3对象，因此按照vector的属性和方法，设置x/y/z轴方向的缩放大小。

![img](https://i0.hdslb.com/bfs/article/6787f2503d0cf27e1c4b9d672f5a5484131f8a4f.png@690w_171h_progressive.webp)

```js
//例如设置x轴放大3倍、y轴方向放大2倍、z轴方向不变
cube.scale.set(3, 2, 1);
//单独设置某个轴的缩放
cube.scale.x = 3
```

### 2.  rotation设置旋转

因为的旋转通过设置rotation属性，该属性是Euler类的实例，因此可以通过Euler类的方法进行设置旋转角度。

![img](https://i0.hdslb.com/bfs/article/ff0425e4f2395e403eaec39d9bc0b797e65df7a2.png@426w_128h_progressive.webp)


因此可以通过以下方式设置旋转物体

```js
//直接设置旋转属性，例如围绕x轴旋转90度
cube.rotation.x = -Math.PI/2

//围绕x轴旋转45度
cube.rotation.set(-Math.PI / 4, 0, 0, "XZY");

```

**set方法，每个参数具体定义:**

.set ( x : Float, y : Float, z : Float, order : String ) : this

x - 用弧度表示x轴旋转量。
y - 用弧度表示y轴旋转量。
z - 用弧度表示z轴旋转量。
order - (optional) 表示旋转顺序的字符串。

#### 2.1 旋转动画

每一帧旋转弧度制的0.01角度，实现动画代码

```js
function render() {
  cube.position.x += 0.01;
  cube.rotation.x += 0.01;
  if (cube.position.x > 5) {
    cube.position.x = 0;
  }
  renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}

```

### 3.综合上述代码

```js
import * as THREE from "three";
// 导入轨道控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

// console.log(THREE);

// 目标：控制3d物体移动

// 1、创建场景
const scene = new THREE.Scene();

// 2、创建相机
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);

// 设置相机位置
camera.position.set(0, 0, 10);
scene.add(camera);

// 添加物体
// 创建几何体
const cubeGeometry = new THREE.BoxGeometry(1, 1, 1);
const cubeMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 });
// 根据几何体和材质创建物体
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);

// 修改物体的位置
// cube.position.set(5, 0, 0);
cube.position.x = 3;

// 将几何体添加到场景中
scene.add(cube);

console.log(cube);

// 初始化渲染器
const renderer = new THREE.WebGLRenderer();
// 设置渲染的尺寸大小
renderer.setSize(window.innerWidth, window.innerHeight);
// console.log(renderer);
// 将webgl渲染的canvas内容添加到body
document.body.appendChild(renderer.domElement);

// // 使用渲染器，通过相机将场景渲染进来
// renderer.render(scene, camera);

// 创建轨道控制器
const controls = new OrbitControls(camera, renderer.domElement);

// 添加坐标轴辅助器
const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper);

function render() {
  cube.position.x += 0.01;
  if (cube.position.x > 5) {
    cube.position.x = 0;
  }
  renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}

render();
```

效果演示：

![img](https://i0.hdslb.com/bfs/article/8d96d25daeaf34e6608bf5a6e695bb72e873ea46.gif@830w_543h_progressive.webp)

## 八、Threejs正确处理动画运动

为了最好的利用性能和渲染效果，那么我们只需要在绘制每一帧画面的时候，计算需要渲染的画面即可。这个时候就可以使用window.requestAnimationFrame方法。

![img](https://i0.hdslb.com/bfs/article/30598fa39925444ccd5284764b35499960fd9699.gif@761w_543h_progressive.webp)

### 1. 使用方式

**window.requestAnimationFrame()** 告诉浏览器——你希望执行一个动画，并且要求浏览器在下次重绘之前调用指定的回调函数更新动画。该方法需要传入一个回调函数作为参数，该回调函数会在浏览器下一次重绘之前执行。

```js
function callback(){
  //下一帧渲染画面前，需要执行处理的函数
}
window.requestAnimationFrame(callback);
```

### 1. 请求动画帧间隔不固定

当你准备更新动画时你应该调用此方法。这将使浏览器在下一次重绘之前调用你传入给该方法的动画函数 (即你的回调函数)。回调函数执行次数通常是每秒 60 次，但在大多数遵循 W3C 建议的浏览器中，回调函数执行次数通常与浏览器屏幕刷新次数相匹配。因此具体回调函数执行的间隔时间跟屏幕刷新次数、当前页面运行时负荷等因素有关。

#### 1.2.1 解决确保不同帧率的画面运行速度一致

回调函数会被传入`DOMHighResTimeStamp`参数，`DOMHighResTimeStamp`指示当前被 `requestAnimationFrame() `排序的回调函数被触发的时间。

请确保总是使用第一个参数 (或其它获得当前时间的方法) 计算每次调用之间的时间间隔，否则动画在高刷新率的屏幕中会运行得更快。

```js
let preTime;
function render(time) {
  //第一次调用render函数，没有上一帧的时间
  if (preTime === undefined) {
    preTime = time;
  }

  //计算每帧画面的间隔时间，单位毫秒
  const deltaTime = time - preTime;
  console.log(deltaTime)
  //保留当前时间作为上一帧时间，用于下一帧计算2帧间隔
  preTime = time;
  //renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}

render();
```

代码执行后，我们通过输出可以看出，每一帧间隔非常相近，但又不同。

![img](https://i0.hdslb.com/bfs/article/f02caef1668448e3e4f9ca0668897e1b77461ce6.png@306w_113h_progressive.webp)


为了确保不同时间间隔，运动的速度一致，那么应该按照

移动距离 = 速度 * 时间

那么如果想要1m/s的速度远离原点的匀速运动，那么就按照这么编写代码

```js
let preTime;
function render(time) {
  //第一次调用render函数，没有上一帧的时间
  if (preTime === undefined) {
    preTime = time;
  }

  //计算每帧画面的间隔时间，单位毫秒，当前时间减去上一帧的时间，即为2帧直接的间隔时间
  const deltaTime = time - preTime;
  console.log(deltaTime)

  preTime = time;

  //cube物体允许运动
  //elapsedTime/1000是将毫秒改为秒
  //1m/s的速度* 时间（秒）= 移动的距离
  //将当前位置+=移动的距离，即为最后的距离
  cube.position.x += 1 * (deltaTime/1000)


  //renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}

render();
```

### 2 综合上述代码

**在前面创建的项目中的main.js文件写入代码，实现每5秒，即从原点出发匀速在x轴进行1m/s的匀速运动**

```
import * as THREE from "three";
// 导入轨道控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

// console.log(THREE);

// 目标：requestAnimationFrame 时间参数 控制物体动画

// 1、创建场景
const scene = new THREE.Scene();

// 2、创建相机
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);

// 设置相机位置
camera.position.set(0, 0, 10);
scene.add(camera);

// 添加物体
// 创建几何体
const cubeGeometry = new THREE.BoxGeometry(1, 1, 1);
const cubeMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 });
// 根据几何体和材质创建物体
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);

// 修改物体的位置
// cube.position.set(5, 0, 0);
// cube.position.x = 3;
// 缩放
// cube.scale.set(3, 2, 1);
// cube.scale.x = 5;
// 旋转
cube.rotation.set(Math.PI / 4, 0, 0, "XZY");

// 将几何体添加到场景中
scene.add(cube);

console.log(cube);

// 初始化渲染器
const renderer = new THREE.WebGLRenderer();
// 设置渲染的尺寸大小
renderer.setSize(window.innerWidth, window.innerHeight);
// console.log(renderer);
// 将webgl渲染的canvas内容添加到body
document.body.appendChild(renderer.domElement);

// // 使用渲染器，通过相机将场景渲染进来
// renderer.render(scene, camera);

// 创建轨道控制器
const controls = new OrbitControls(camera, renderer.domElement);

// 添加坐标轴辅助器
const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper);

function render(time) {

  let t = (time / 1000) % 5;
  cube.position.x = t * 1;
  renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}

render();
```

效果演示：


![img](https://i0.hdslb.com/bfs/article/30598fa39925444ccd5284764b35499960fd9699.gif@761w_543h_progressive.webp)

## 九、Threejs中Clock跟踪时间处理动画

### 1. Clock

该对象用于跟踪时间。如果`performance.now`可用，则 `Clock `对象通过该方法实现，否则回落到使用略欠精准的`Date.now`来实现。

实例化`clock`对象，`new Clock( autoStart : Boolean )`，autoStart — (可选) 是否要在第一次调用 .`getDelta() `时自动开启时钟。默认值是 true。

```js
// 初始化时钟
const clock = new THREE.Clock();
```

#### 1.1 获取运行当前帧的时间

**getElapsedTime ()获取自时钟启动后的秒数。**

```js
// 设置时钟
const clock = new THREE.Clock();
function render() {
  // 获取时钟运行的总时长
  let time = clock.getElapsedTime();
  console.log("时钟运行总时长：", time);

  renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}

render();
```

**getDelta () 获取2帧之间的时间间隔。**

```js
// 设置时钟
const clock = new THREE.Clock();
function render() {
  // 获取2帧之间的时间间隔
  let deltaTime = clock.getElapsedTime();
  console.log("2帧之间的时间间隔：", deltaTime);


  renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}

render();
```

注意：getDelta、getElapsedTime请勿同时用于同一帧，会导致getDelta计时不准。因为每次调用这2个函数，都会对oldTime属性进行重置，所以getDelta计算出来的就不是上一帧的时间。

### 2 综合上述代码

在前面创建的项目中的main.js文件写入代码，实现每5秒，即从原点出发匀速在x轴进行1m/s的匀速运动

```js
import * as THREE from "three";
// 导入轨道控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";

// console.log(THREE);

// 目标：Clock该对象用于跟踪时间

// 1、创建场景
const scene = new THREE.Scene();

// 2、创建相机
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);

// 设置相机位置
camera.position.set(0, 0, 10);
scene.add(camera);

// 添加物体
// 创建几何体
const cubeGeometry = new THREE.BoxGeometry(1, 1, 1);
const cubeMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 });
// 根据几何体和材质创建物体
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);

// 修改物体的位置
// cube.position.set(5, 0, 0);
// cube.position.x = 3;
// 缩放
// cube.scale.set(3, 2, 1);
// cube.scale.x = 5;
// 旋转
cube.rotation.set(Math.PI / 4, 0, 0, "XZY");

// 将几何体添加到场景中
scene.add(cube);

console.log(cube);

// 初始化渲染器
const renderer = new THREE.WebGLRenderer();
// 设置渲染的尺寸大小
renderer.setSize(window.innerWidth, window.innerHeight);
// console.log(renderer);
// 将webgl渲染的canvas内容添加到body
document.body.appendChild(renderer.domElement);

// // 使用渲染器，通过相机将场景渲染进来
// renderer.render(scene, camera);

// 创建轨道控制器
const controls = new OrbitControls(camera, renderer.domElement);

// 添加坐标轴辅助器
const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper);
// 设置时钟
const clock = new THREE.Clock();
function render() {
  // 获取时钟运行的总时长
  let time = clock.getElapsedTime();
  console.log("时钟运行总时长：", time);
  //   let deltaTime = clock.getDelta();
  //     console.log("两次获取时间的间隔时间：", deltaTime);
  let t = time % 5;
  cube.position.x = t * 1;

  renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}

render();
```

效果演示：


![img](https://i0.hdslb.com/bfs/article/30598fa39925444ccd5284764b35499960fd9699.gif@761w_543h_progressive.webp)

## 十、Gsap动画库使用

GSAP 是一个强大的 JavaScript 工具集，让大家秒变动画大佬。构建适用于所有主流浏览器的高性能动画。动画 CSS、SVG、画布、React、Vue、WebGL、颜色、字符串、运动路径、通用对象...... JavaScript 可以触摸的任何东西！GSAP 的ScrollTrigger插件让您可以用最少的代码创建令人瞠目结舌的滚动动画。

### 1 安装GSAP模块

```
npm install gsap
```

### 2 创建动画

例如，如果html元素创建动画，将 '.box' 类的元素设置1秒时间水平移动 200px 的动画。可以这么编写

```js
// 导入动画库
import gsap from "gsap";
gsap.to(".box", { x: 200 })
```

![img](https://i0.hdslb.com/bfs/article/bf128cad580f9386046607041ddd42a2ef7ad705.gif@638w_303h_progressive.webp)

在three.js中如果我们想要将物体，例如立方体移动设置1秒时间水平移动 200px 的动画。可以这么编写

```js
// 导入动画库
import gsap from "gsap";
gsap.to(cube.position, { x: 200 })
```

`gsap.to() -` 这是最常见的补间类型。是设置当前元素或者变量的状态，到设置的状态的补间动画。所谓的补间动画，就是2个关键帧（即2种物体的状态）有了，框架自带计算出中间某个时刻的状态，从而填补2个状态间，动画的空白时刻，从而实现完整动画。

`gsap.to`有2个参数，第一个是目标元素或者变量。如果传入的是.box之类的css字符串选择器，GSAP 在后台使用document.querySelectorAll()选中页面的匹配的元素。当第一个目标是对象时，GSAP就会对其属性值进行修改来实现补间动画。

### 3 GSAP设置动画的属性

如何是html元素，可以设置的属性有

![img](https://i0.hdslb.com/bfs/article/6fe9a5204d47e41dbbde6b7407d97aec71b276ef.png@242w_758h_progressive.webp)

对应CSS样式属性

![img](https://i0.hdslb.com/bfs/article/729c19134fa8c4499934502561074c9164a8cf4b.png@647w_657h_progressive.webp)


下面演示向右水平移动+旋转.box元素的效果

```js
gsap.to(".box", {
  duration: 2,
  x: 200,
  rotation: 360,
});
```


![img](https://i0.hdslb.com/bfs/article/b1aa9500c48d8dcb9cfcb706663ac9b4f6aa4435.gif@638w_303h_progressive.webp)

默认情况下，GSAP 将使用 px 和度数进行变换，但您可以使用其他单位，例如 vw、弧度，甚至可以进行自己的 JS 计算或相对值！

例如：

```js
x : 200 , // 使用 px 的默认值
x : "+=200" // 相对值
x : '40vw' , // 或者传入一个具有不同单位的字符串以供 GSAP 解析
x : () => window 。innerWidth / 2 , // 你甚至可以使用函数值进行计算！
rotation：360 // 使用默认的度数
rotation：“1.25rad” // 使用弧度
```

如果第一个参数目标不是html元素，也可以是对象。。您可以针对任何对象的任何属性，甚至是您创建的任意属性，如下所示

```js
let obj = { myNum: 10, myColor: "red" };
gsap.to(obj, {
  myNum: 200,
  myColor: "blue",
  onUpdate: () => console.log(obj.myNum, obj.myColor)
});
```

这里可以让obj.myNum值从10变化到200，也可以让颜色myColor的值从红色变化到蓝色。每一次更新值的时候，执行onUpdate所设置的回调函数。

### 4 GSAP特殊属性控制动画

`duration`：动画持续时间（秒） 默认值：0.5

`delay`：动画开始前的延迟量（秒）

`repeat`：动画应该重复多少次。-1为一直重复

`yoyo`：如果为 true，则每隔一个重复，补间将沿相反方向运行。（像悠悠球一样）默认值：false

`ease`：控制动画期间的变化率。

`onComplete`：动画完成时调用的函数

`onUpdate`：动画值更新时调用的函数

**ease动画属性设置**
缓动可能是动作设计中最重要的部分。精心挑选的轻松将为您的动画增添个性并注入活力。

在下面的演示中看看 no ease 和bounce ease 之间的区别！绿色盒子以匀速的速度旋转，而紫色盒子带有“反弹”旋转动画，感觉就不一样。

```js
gsap.to(".green", { rotation: 360, duration: 2, ease: "none" });

gsap.to(".purple", { rotation: 360, duration: 2, ease: "bounce.out" });
```

![img](https://i0.hdslb.com/bfs/article/feb4ece2328520705c6108b135f560fdc80fcee0.gif@942w_246h_progressive.webp)

在引擎内部，“ease”是一种数学计算，用于控制补间期间的变化率。但不用担心，框架会为您做所有的数学计算！您只需坐下来选择最适合我们的动画的效果即可。

对于大多数效果，分为三种类型in、out、inOut。这些控制了轻松过程中的动量。

像这样的 设置ease："power1.out" 是 UI 过渡的最佳选择；它们启动速度很快，这有助于 UI 感觉反应灵敏，然后它们在接近尾声时放松，给人一种自然的摩擦感。

理解ease的最好方法是玩转ease配置的可视化工具！

地址：https://greensock.com/get-started/#greenSockEaseVisualizer

效果：

![img](https://i0.hdslb.com/bfs/article/571505cca304e007f8f140f8ac404c23f4cfbfc5.gif@942w_872h_progressive.webp)

**Staggers交错**
这是我们最喜欢的技巧之一！如果补间有多个目标，您可以轻松地在每个动画的开始之间添加一些交错效果：

```js
gsap.from(".box", {
  duration: 2,
  scale: 0.5,
  opacity: 0,
  delay: 0.5,
  stagger: 0.2,
  ease: "elastic",
  force3D: true
});

document.querySelectorAll(".box").forEach(function(box) {
  box.addEventListener("click", function() {
    gsap.to(".box", {
      duration: 0.5,
      opacity: 0,
      y: -100,
      stagger: 0.1,
      ease: "back.in"
    });
  });
});
```

![img](https://i0.hdslb.com/bfs/article/06690133d728fa622024e5958e8b7408d1919715.gif@942w_171h_progressive.webp)

这里`stagger`设置0.2，即为将.box选中多个元素设置为每隔0.2秒开始运动1个元素实现效果。

**时间线-Timelines**
时间线是创建易于调整、有弹性的动画序列的关键。当您将补间添加到时间线时，默认情况下，它们会按照添加的顺序一个接一个地播放。

```js
// 创建时间线动画
let tl = gsap.timeline()

// 现在用tl代替以前的gsap来设置动画即可。
tl.to(".green", { x: 600, duration: 2 });
tl.to(".purple", { x: 600, duration: 1 });
tl.to(".orange", { x: 600, duration: 1 });
```

![img](https://i0.hdslb.com/bfs/article/1bab035b89eb2ef011aed574f7937065fba17e14.gif@942w_447h_progressive.webp)

### 5 Threejs场景种应用

设置立方体旋转

```js
gsap.to(
  cube.rotation,
  {
    x: 2 * Math.PI,
    duration: 5,
    ease: "power1.inOut"
  }
);
```

设置立方体来回往返运动

```js
// 设置动画
var animate1 = gsap.to(cube.position, {
  x: 5,
  duration: 5,
  ease: "power1.inOut",
  //   设置重复的次数，无限次循环-1
  repeat: -1,
  //   往返运动
  yoyo: true,
  //   delay，延迟2秒运动
  delay: 2,
  // 当动画完成时，执行回调函数
  onComplete: () => {
    console.log("动画完成");
  },
  //当动画开始时，执行回调函数
  onStart: () => {
    console.log("动画开始");
  },
});
```

让双击画面，控制立方体动画暂停和恢复动画，前面创建的animate1这个动画实例，有isActive方法，可以用来获取当前动画是暂停还是播放状态，播放状态时isActive方法返回为true，暂停时为false，根据这个状态来调用pause方法来暂停动画和恢复动画。

```js
window.addEventListener("dblclick", () => {
  //   console.log(animate1);
  if (animate1.isActive()) {
    //   暂停
    animate1.pause();
  } else {
    //   恢复
    animate1.resume();
  }
});
```

### 6 综合上述代码

**在前面创建的项目中的main.js文件写入代码**

```js
import * as THREE from "three";
// 导入轨道控制器
import { OrbitControls } from "three/examples/jsm/controls/OrbitControls";
// 导入动画库
import gsap from "gsap";

// console.log(THREE);

// 目标：掌握gsap设置各种动画效果

// 1、创建场景
const scene = new THREE.Scene();

// 2、创建相机
const camera = new THREE.PerspectiveCamera(
  75,
  window.innerWidth / window.innerHeight,
  0.1,
  1000
);

// 设置相机位置
camera.position.set(0, 0, 10);
scene.add(camera);

// 添加物体
// 创建几何体
const cubeGeometry = new THREE.BoxGeometry(1, 1, 1);
const cubeMaterial = new THREE.MeshBasicMaterial({ color: 0xffff00 });
// 根据几何体和材质创建物体
const cube = new THREE.Mesh(cubeGeometry, cubeMaterial);

// 修改物体的位置
// cube.position.set(5, 0, 0);
// cube.position.x = 3;
// 缩放
// cube.scale.set(3, 2, 1);
// cube.scale.x = 5;
// 旋转
cube.rotation.set(Math.PI / 4, 0, 0, "XZY");

// 将几何体添加到场景中
scene.add(cube);

console.log(cube);

// 初始化渲染器
const renderer = new THREE.WebGLRenderer();
// 设置渲染的尺寸大小
renderer.setSize(window.innerWidth, window.innerHeight);
// console.log(renderer);
// 将webgl渲染的canvas内容添加到body
document.body.appendChild(renderer.domElement);

// // 使用渲染器，通过相机将场景渲染进来
// renderer.render(scene, camera);

// 创建轨道控制器
const controls = new OrbitControls(camera, renderer.domElement);

// 添加坐标轴辅助器
const axesHelper = new THREE.AxesHelper(5);
scene.add(axesHelper);
// 设置时钟
const clock = new THREE.Clock();

// 设置动画
var animate1 = gsap.to(cube.position, {
  x: 5,
  duration: 5,
  ease: "power1.inOut",
  //   设置重复的次数，无限次循环-1
  repeat: -1,
  //   往返运动
  yoyo: true,
  //   delay，延迟2秒运动
  delay: 2,
  onComplete: () => {
    console.log("动画完成");
  },
  onStart: () => {
    console.log("动画开始");
  },
});
gsap.to(cube.rotation, { x: 2 * Math.PI, duration: 5, ease: "power1.inOut" });

window.addEventListener("dblclick", () => {
  //   console.log(animate1);
  if (animate1.isActive()) {
    //   暂停
    animate1.pause();
  } else {
    //   恢复
    animate1.resume();
  }
});

function render() {
  renderer.render(scene, camera);
  //   渲染下一帧的时候就会调用render函数
  requestAnimationFrame(render);
}

render();
```

## 十一、画布自适应屏幕大小与全屏

![img](https://i0.hdslb.com/bfs/article/983a66c2cfbc4f101ae88c9f6691ff19ec666de9.gif@827w_480h_progressive.webp)

### 1.1 自适应屏幕大小 

你会发现，我们前面写好的代码，在页面尺寸发生改变的时候，并不能自适应的改变尺寸，而出现空白或者滚动条突出的情况。所以监听屏幕大小的改变，来重新设置相机的宽高比例和渲染器的尺寸大小，代码如下：

```js
// 监听画面变化，更新渲染画面
window.addEventListener("resize", () => {
  //   console.log("画面变化了");
  // 更新摄像头
  camera.aspect = window.innerWidth / window.innerHeight;
  //   更新摄像机的投影矩阵
  camera.updateProjectionMatrix();

  //   更新渲染器
  renderer.setSize(window.innerWidth, window.innerHeight);
  //   设置渲染器的像素比
  renderer.setPixelRatio(window.devicePixelRatio);
});
```

aspect属性是设置摄像机视锥体的长宽比，通常是使用画布的宽/画布的高。camera.updateProjectionMatrix()用于更新摄像机投影矩阵，相机任何参数被改变以后必须被调用

### 1.2 控制场景全屏

经常我们需要全屏的展示三维场景。例如，我们想要双击，实现全屏效果，代码如下：

```js
window.addEventListener("dblclick", () => {
  const fullScreenElement = document.fullscreenElement;
  if (!fullScreenElement) {
    //   双击控制屏幕进入全屏，退出全屏
    // 让画布对象全屏
    renderer.domElement.requestFullscreen();
  } else {
    //   退出全屏，使用document对象
    document.exitFullscreen();
  }
});
```

`fullscreenElement`只读属性返回当前在此文档中以全屏模式显示的元素。

如果文档当前未使用全屏模式，则返回值为null。

使用`element.requestFullscreen()`方法以全屏模式查看元素，`exitFullscreen`方法退出全屏。 

## 十二、应用图形用户界面更改变量

```js
// 应用图形用户界面更改变量
const gui = new dat.GUI();

// 修改物体移动位置
gui.add(cube.position, "x")
    .min(0) // 最小值
    .max(5) // 最大值
    .step(0.01) // 每次移动的变量值
    .name("移动x轴") // 变量名称
    .onChange((value) => {
        console.log("值被修改：", value)
    }).onFinishChange((value) => {
        console.log("完全停下来的值：", value)
    })

const params = {
    color: "#ffff00",
    // 控制物体运动
    fn: () => {
        gsap.to(cube.position, { x: 5, duration: 3, yoyo: true, repeat: - 1 })
}
}
// 修改物体颜色
gui.addColor(params, "color").onChange(value => {
    console.log("颜色被修改了", value)
    cube.material.color.set(value)
})
// 设置物体是否显示
gui.add(cube, "visible").name("是否显示")

// 设置按钮点击触发某个事件
gui.add(params,"fn").name("物体运动")

// 设置文件夹,
const folder = gui.addFolder("设置物体")
folder.add(cube.material,"wireframe")
folder.add(params,"fn").name("物体运动")
```



