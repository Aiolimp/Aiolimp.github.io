---
title: webpack常见应用场景
date: 2021-08-06 16:57:49
top_img: https://static001.geekbang.org/resource/image/73/2a/737fb9f94c18a26a875c27169222b82a.jpg
cover: https://static001.geekbang.org/resource/image/73/2a/737fb9f94c18a26a875c27169222b82a.jpg
tags:
- webpack
categories:
- webpack
---

## CSS处理

### 解析CSS文件

在前面的例子也能看到，我们解析`css`需要用到的`loader`有`css-loader`和`style-loader`。`css-loader`主要用来解析`css`文件，而`style-loader`是将`css`渲染到`DOM`节点上。

首先我们来安装一下：

```shell
 # css-loader -> 6.2.0;  style-loader -> 3.2.1
 yarn add css-loader style-loader -D
```

然后我们新建一个`css`文件。

```css
/* style.css */
body {
  background: #222;
  color: #fff;
}
```

然后在`index.js`引入一下：

```javascript
import "./style.css";
```

紧接着我们配置一下`webpack`：

```javascript
module.exports = {
   ...
  
  module: {
    rules: [
      {
        test: /\.css$/,  // 识别css文件
        use: ['style-loader', 'css-loader']  // 先使用css-loader,再使用style-loader
      }
    ]
  },
  
   ...
};
```

这时候我们打包一下，会发现`dist`路径下只有`main.js`和`index.html`。但打开一下`index.html`会发现`css`是生效的。

这是因为`style-loader`是将`css`代码插入到了`main.js`当中去了。

### 打包css文件

如果我们不想将`css`代码放进`js`中，而是直接导出一份`css`文件的话，就得使用另一个插件——`mini-css-extract-plugin`。

```shell
# 2.1.0
yarn add mini-css-extract-plugin -D
```

然后将其引入到配置文件，并且在`plugins`引入。

```javascript
const miniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
    ...
  
    plugins: [
      	// 使用miniCssExtractPlugin插件
        new miniCssExtractPlugin({
          	filename: "[name].css"  // 设置导出css名称，[name]占位符对应chunkName
        })
    ]
};
```

紧接着，我们还需要更改一下`loader`，我们不再使用`style-loader`，而是使用`miniCssExtractPlugin`提供的`loader`。

```javascript
const miniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
    ...
  
    module: {
        rules: [
            {
                test: /\.css$/,
              	// 使用miniCssExtractPlugin.loader替换style-loader
                use: [miniCssExtractPlugin.loader,'css-loader']
            }
        ]
    },
    plugins: [
        new miniCssExtractPlugin({
          	filename: "[name].css" 
        })
    ]
};
```

接下来打包一下，`dist`路径下就会多出一个`main.css`文件，并且在`index.html`中也会自动引入。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
<script defer src="main.js"></script><link href="main.css" rel="stylesheet"></head>
<body>
    HelloWorld！
</body>
</html>
```

### css添加浏览器前缀

当我们使用一下`css`新特性的时候，可能需要考虑到浏览器兼容的问题，这时候可能需要对一些`css`属性添加浏览器前缀。而这类工作，其实可以交给`webpack`去实现。准确来说，是交给`postcss`去实现。

`postcss`对于`css`犹如`babel`对于`JavaScript`，它专注于对转换`css`，比如添加前缀兼容、压缩`css`代码等等。

首先我们需要先安装一下`postcss`和`post-css-loader`。

```shell
# postcss -> 8.3.6，postcss-loader -> 6.1.1
yarn add postcss postcss-loader -D
```

接下来，我们在`webpack`配置文件先引入`postcss-loader`，它的顺序是在`css-loader`之前执行的。

```javascript
rules: [
  {
    test: /\.css$/,
    // 引入postcss-loader
    use: [miniCssExtractPlugin.loader, 'css-loader', 'postcss-loader']
  }
]
```

接下来配置`postcss`的工作，就不在`webpack`的配置文件里面了。`postcss`自身也是有配置文件的，我们需要在项目根路径下新建一个`postcss,config.js`。然后里面也有一个配置项，为`plugins`。

```javascript
module.exports = {
    plugins: []
}
```

这也意味着，`postcss`自身也支持很多第三方插件使用。

现在我们想实现的添加前缀的功能，需要安装的插件叫`autoprefixer`。

```shell
# 1.22.10
yarn add autoprefixer -D
```

然后我们只需要引入到`postcss`的配置文件中，并且它里面会有一个配置选项，叫`overrideBrowserslist`，是用来填写适用浏览器的版本。

```javascript
module.exports = {
    plugins: [
        // 将css编译为适应于多版本浏览器
        require('autoprefixer')({
            // 覆盖浏览器版本
          	// last 2 versions: 兼容各个浏览器最新的两个版本
          	// > 1%: 浏览器全球使用占有率大于1%
            overrideBrowserslist: ['last 2 versions', '> 1%']
        })
    ]
}
```

关于`overrideBrowserslist`的选项填写，我们可以去参考一下[browserslist](https://link.juejin.cn?target=https%3A%2F%2Fgithub.com%2Fbrowserslist%2Fbrowserslist)，这里就不多讲。

当然，我们其实可以在`package.json`中填写兼容浏览器信息，或者使用`browserslist`配置文件`.browserslistrc`来填写，这样子如果我们以后使用其他插件也需要考虑到兼容浏览器的时候，就可以统一用到，比如说`babel`。

```json
// package.json 文件
{
  ...
  "browserslist": ['last 2 versions', '> 1%']
}

# .browserslsetrc 文件
last 2 versions
> 1%
```

但如果你多个地方都配置的话，`overrideBrowserslist`的优先级是最高的。

接下来，我们修改一下`style.css`，使用一下比较新的特性。

```css
body {
    display: flex;
}
```

然后打包一下，看看打包出来后的`main.css`。

```css
body {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
}
```

### 压缩css代码

当我们需要压缩`css`代码的时候，可以使用`postcss`另一个插件——`cssnano`。

```shell
# 5.0.7
yarn add cssnano -D
```

然后还是在`postcss`配置文件中引入：

```javascript
module.exports = {
    plugins: [
        ... ,
        require('cssnano')
    ]
}
```

打包一下，看看`main.css`。

```css
body{display:-webkit-box;display:-ms-flexbox;display:flex}
```

### 解析CSS预处理器

在现在我们实际开发中，我们会更多使用`Sass`、`Less`或者`stylus`这类`css`预处理器。而其实`html`是无法直接解析这类文件的，因此我们需要使用对应的`loader`将其转换成`css`。

接下来，我就以`sass`为例，来讲一下如何使用`webpack`解析`sass`。

首先我们需要安装一下`sass`和`sass-loader`。

```shell
# sass -> 1.36.0, sass-loader -> 12.1.0
yarn add sass sass-loader -D
```

然后我们在`module`加上`sass`的匹配规则，`sass-loader`的执行顺序应该是排第一，我们需要先将其转换成`css`，然后才能执行后续的操作。

```javascript
rules: [
  ...
  
  {
    test: /\.(scss|sass)$/,
    use: [miniCssExtractPlugin.loader, 'css-loader', 'postcss-loader', 'sass-loader']
  }
]
```

然后我们在项目中新建一个`style.scss`。

```scss
$color-white: #fff;
$color-black: #222;

body {
    background: $color-black;

    div {
        color: $color-white;
    }
}
```

然后在`index.js`引入。

```javascript
import "./style.css";
import "./style.scss";
```

然后执行打包，再看看打包出来的`main.css`，`scss`文件内容被解析到里面，同时如果我们引入多个`css`或`css`预处理器文件的话，`miniCssExtractPlugin`也会将其打包成一个`bundle`文件里面。

```css
body{display:-webkit-box;display:-ms-flexbox;display:flex}
body{background:#222}body div{color:#fff}
```

## HTML模板

当我们是`Web`项目的时候，我们必然会存在`html`文件去实现页面。

而对于其他类型的文件，比如`css`、图片、文件等等，我们是可以通过引入入口`js`文件，然后通过`loader`进行解析打包。而对于`html`文件，我们不可能将其引入到入口文件然后解析打包，反而我们还需要将打包出来的`bundle`文件引入`html`文件去使用，

因此，其实我们需要实现的操作只有两个，一个是复制一份`html`文件到打包路径下，另一个就是将打包出来的`bundle`文件自动引入到`html`文件中去。

这时候我们需要使用一个插件来实现这些功能——`html-webpack-plugin`。

```shell
# 5.3.2
yarn add html-webpack-plugin -D
```

安装插件后，我们先在`src`文件下新建一下`index.html`。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Webpack Demo</title>
</head>
<body>
    <div>Hello World</div>
</body>
</html>
```

这里面我们暂时不需要引入任何模块。

接下来配置一下`webpack`。一般`plugin`插件都是一个类，而我们需要在`plugins`选项中需要创建一个插件实例。

对于`htmlWebpackPlugin`插件，我们需要传入一些配置：`html`模板地址`template`和打包出来的文件名`filename`。

```javascript
const path = require('path');
// 引入htmlWebpackPlugin
const htmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    entry: './src/index.js',
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: '[name].js',
    },
    plugins: [
      	// 使用htmlWebpackPlugin插件
        new htmlWebpackPlugin({
         	 // 指定html模板
            template: './src/index.html',  
          	// 自定义打包的文件名
            filename: 'index.html'
        })
    ]
};
```

接下来执行一下打包，就会发现`dist`文件下会生成一个`index.html`。打开会发现，`webpack`会自动将`bundle`文件引入：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Webpack Demo</title>
<script defer src="main.js"></script></head>
<body>
    <div>Hello World</div>
</body>
</html>
```

如果我们有多个`chunk`的时候，我们可以指定该`html`要引入哪些`chunk`。在`htmlWebpackPlugin`配置中有一个`chunks`选项，是一个数组，你只需要加入你想引入的`chunkName`即可。

```javascript
const path = require('path');
// 引入htmlWebpackPlugin
const htmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    entry: {
      	index: './src/index.js',
      	main: './src/main.js'
    },
    output: {
        path: path.resolve(__dirname, 'dist'),
        filename: '[name].js',
    },
    plugins: [
        new htmlWebpackPlugin({
            template: './src/index.html',  
            filename: 'index.html',
          	chunks: ["index"]  // 只引入index chunk
        })
    ]
};
```

打包完成后，`dist`文件下会出现`index.html`、`index.js`和`main.js`，但是`index.html`只会引入`index.js`。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
<script defer src="index.js"></script></head>
<body>
    HelloWorld！
</body>
</html>
```

如果我们需要实现多页面的话，只需要再`new`一个`htmlWebpackPlugin`实例即可，这里就不再多说。

## JavaScript转义

不仅仅`css`需要转义，`JavaScript`也要为了兼容多浏览器进行转义，因此我们需要用到`babel`。

```shell
# 8.2.2
yarn add babel-loader -D
```

同时，我们需要使用`babel`中用于`JavaScript`兼容的插件：

```shell
# @babel/preset-env -> 7.14.9; @babel/core -> 7.14.8; @core-js -> 3.16.0
yarn add @babel/preset-env @babel/core core-js -D
```

接下来，我们需要配置一下`webpack`的配置文件。

```javascript
{
  test: /\.js$/,
  use: ['babel-loader'] 
}
```

然后我们需要配置一下`babel`。当然我们可以直接在`webpack.config.js`里面配置，但是`babel`同样也提供了配置文件`.babelrc`，因此我们就直接在这边进行配置。

在根路径新建一个`.babelrc`。

```json
{
  "presets": [
    [
      "@babel/preset-env",
      {
      	// 浏览器版本
        "targets": {
          "edge": "17",
          "chrome": "67"
        },
         // 配置corejs版本，但需要额外安装corejs
        "corejs": 3,
        // 加载情况
        // entry: 需要在入口文件进入@babel/polyfill，然后babel根据使用情况按需载入
        // usage: 无需引入，自动按需加载
        // false: 入口文件引入，全部载入
        "useBuiltIns": "usage"
      }
    ]
  ]
}
```

接下来，我们来测试一下，先修改一下`index.js`。

```javascript
new Promise(resolve => {
    resolve('HelloWorld')
}).then(res => {
    console.log(res);
})
```

然后执行`yarn build`进行打包。

在使用`babel`之前，打包出来的`main.js`如下。

```javascript
!function(){"use strict";new Promise((o=>{o("HelloWorld")})).then((o=>{console.log(o)}))}();
```

上面打包代码是直接使用了`Promise`，而没有考虑到低版本浏览器的兼容。然后我们打开`babel`后，执行一下打包命令，会发现代码多出了很多。

而在打包代码中，可以看到`webpack`使用了`polyfill`实现`promise`类，然后再去调用，从而兼容了低版本浏览器没有`promise`属性问题。

## Webpack本地服务

当我们使用`webpack`的时候，不仅仅只是用于打包文件，大部分我们还会依赖`webpack`来搭建本地服务，同时利用其热更新的功能，让我们更好的开发和调试代码。

接下来我们来安装一下`webpack-dev-server`：

```shell
# 版本为3.11.2
yarn add webpack-dev-server -D
```

然后执行下列代码开启服务：

```shell
npx webpack serve
```

或者在package.json配置一下：

```json
"scripts": {
  "serve": "webpack serve --mode development"
}
```

然后通过`yarn serve`运行。

这时，webpack会默认开启`http://localhost:8080/`服务（具体看你们运行返回的代码），而该服务指向的是`dist/index.html`。

但你会发现，你的`dist`中其实是没有任何文件的，这是因为`webpack`将实时编译后的文件都保存到了内存当中。

### webpack-dev-server的好处

其实`webpack`自带提供了`--watch`命令，可以实现动态监听文件的改变并实时打包，输出新的打包文件。

但这种方案存在着几个缺点，一就是每次你一修改代码，webpack就会全部文件进行重新打包，这时候每次更新打包的速度就会慢了很多；其次，这样的监听方式做不到热更新，即每次你修改代码后，webpack重新编译打包后，你就得手动刷新浏览器，才能看到最新的页面结果。

而`webpack-dev-server`，却有效了弥补这两个问题。它的实现，是使用了`express`启动了一个`http`服务器，来伺候资源文件。然后这个`http`服务器和客户端使用了`websocket`通讯协议，当原始文件作出改动后，`webpack-dev-server`就会实时编译，然后将最后编译文件实时渲染到页面上。

### webpack-dev-server配置

在`webpack.config.js`中，有一个`devServer`选项是用来配置`webpack-dev-server`，这里简单讲几个比较常用的配置。

#### port

我们可以通过port来设置服务器端口号。

```javascript
module.exports = {
  
    ...
  
    // 配置webpack-dev-server
    devServer: {
        port: 8888,  // 自定义端口号
    },
};
```

#### open

在`devServer`中有一个`open`选项，默认是为`false`，当你设置为`true`的时候，你每次运行`webpack-dev-server`就会自动帮你打开浏览器。

```javascript
module.exports = {
  
    ...
  
    // 配置webpack-dev-server
    devServer: {
        open: true,   // 自动打开浏览器窗口
    },
};
```

#### proxy

这个选项是用来设置本地开发的跨域代理的，关于跨域的知识就不多在这说了，这里就说说如何去配置。

`proxy`的值必须是一个对象，在里面我们可以配置一个或多个跨域代理。最简单的配置写法就是地址配上`api`地址。

```javascript
module.exports = {
  
    ...
  
    devServer: {
      	// 跨域代理
        proxy: {
          '/api': 'http://localhost:3000'
        },
    },
};
```

这时候，当你请求`/api/users`的时候，就会代理到`http://localhost:3000/api/users`。

如果你不需要传递`/api`的话，你就需要使用对象的写法，从而增加一些配置选项：

```javascript
module.exports = {
    //...
    devServer: {
      	// 跨域代理
        proxy: {
            '/api': {
              target: 'http://localhost:3000',  // 代理地址
              pathRewrite: { '^/api': '' },   // 重写路径
            },
        },
    },
};
```

这时候，当你请求`/api/users`的时候，就会代理到`http://localhost:3000/users`。

在proxy中的选项，还有两个比较常用的，一个就是`changeOrigin`，默认情况下，代理时会保留主机头的来源，当我们将其设置为`true`可以覆盖这种行为；还有一个是`secure`选项，当你的接口使用了`https`的时候，需要将其设置为`false`。

```javascript
module.exports = {
    //...
    devServer: {
      	// 跨域代理
        proxy: {
            '/api': {
              target: 'http://localhost:3000',  // 代理地址
              pathRewrite: { '^/api': '' },   // 重写路径
              secure: false,  // 使用https
              changeOrigin: true   // 覆盖主机源
            },
        },
    },
};
```

## 多个打包配置

通常我们项目都会有开发环境和生产环境。

前面我们也看到了`webpack`提供了一个`mode`选项，但我们开发中不太可能说开发的时候`mode`设置为`development`，然后等到要打包才设置为`production`。当然，前面我们也说了，我们可以通过命令`--mode`来对应匹配`mode`选项。

但如果开发环境和生产环境的`webpack`配置差异不仅仅只有`mode`选项的话，我们可能需要考虑多份打包配置了。

### 多个webpack配置文件

我们默认的`webpack`配置文件名为`webpack.config.js`，而`webpack`执行的时候，也默认会找该配置文件。

但如果我们不使用该文件名，而改成`webpack.conf.js`的话，`webpack`正常执行是会使用默认配置的，因此我们需要使用一个`--config`选项，来指定配置文件。

```shell
webpack --config webpack.conf.js
```

因此，我们就可以分别配置一个开发环境的配置`webpack.dev.js`和生成环境的配置`webpack.prod.js`，然后通过指令进行执行不同配置文件：

```json
// package.json
 "scripts": {
   "dev": "webpack --config webpack.dev.js",
   "build": "webpack --config webpack.prod.js",
 }

```

### 单个配置文件

如果说，你不想创建那么多配置文件的话，我们也可以只只用`webpack.config.js`来实现多份打包配置。

按照前面说的使用`--mode`配置`mode`选项，其实我们可以在`webpack.config.js`中拿到这个变量，因此我们就可以通过这个变量去返回不同的配置文件。

```javascript
// argv.mode可以获取到配置的mode选项
module.exports = (env, argv) => {
  if (argv.mode === 'development') {
    // 返回开发环境的配置选项
    return { ... }
  }else if (argv.mode === 'production') {
    // 返回生产环境的配置选项
    return { ... }
  }
};
```

## 静态资源处理

当我们使用了图片、视频或字体等等其他静态资源的话，我们需要用到`url-loader`和`file-loader`。

```shell
# url-loader -> 4.1.1; file-loader -> 6.2.0
yarn add url-loader file-loader -D
```

首先我们在项目中引入一张图片，然后在引入到`index.js`中。

```javascript
import pic from "./image.png";

const img = new Image();
img.src= pic;
document.querySelector('body').append(img);
```

然后我先使用`url-loader`。

```javascript
module.exports = {
  ...
  
  module: {
    rules: [
      {
        test: /\.(png|je?pg|gif|webp)$/,
        use: ['url-loader']
      }
    ]
  }
};
```

然后执行一下打包。

你会发现，`dist`路径下没有图片文件，但是你打开页面是可以看到图片的，且通过调试工具，我们可以看到其实`url-loader`默认会将静态资源转成`base64`。

当然，`url-loader`选项有提供一个属性，叫`limit`，就是我们可以设置一个文件大小阈值，当文件大小超过这个值的时候，`url-loader`就不会转成`base64`，而是直接打包成文件。

```javascript
module.exports = {
  ...
  
  module: {
    rules: [
      {
        test: /\.(png|je?pg|gif|webp)$/,
        use: [{
          loader: 'url-loader',
          options: {
            name: '[name].[ext]',   // 使用占位符设置导出名称
            limit: 1024 * 10  // 设置转成base64阈值，大于10k不转成base64
          }
        }]
      }
    ]
  }
};
```

这时候我们再打包一下，`dist`文件夹下就会出现了图片文件。

而`file-loader`其实跟`url-loader`差不多，但它默认就是导出文件，而不会导出`base64`的。

```javascript
module.exports = {
  ...
  
  module: {
    rules: [
      {
        test: /\.(png|je?pg|gif|webp)$/,
        use: ['file-loader']
      }
    ]
  }
};
```

打包一下，会发现`dist`文件夹下依旧会打包成一个图片文件，但是它的名称会被改成哈希值，我们可以通过`options`选项来设置导出的名称。

```javascript
module.exports = {
  ...
  
  module: {
    rules: [
      {
        test: /\.(png|je?pg|gif|webp)$/,
        use: [{
          loader: 'file-loader',
          options: {
            name: '[name].[ext]',   // 使用占位符设置导出名称
          }
        }]
      }
    ]
  }
};
```

而对于视频文件、字体文件，也是用相同的方法，只不过是修改`test`。

```javascript
module.exports = {
  ...
  module: {
    rules: [
      // 图片
      {
        test: /\.(png|je?pg|gif|webp)$/,
        use: {
          loader: 'url-loader',
          options: {
            esModule: false,
            name: '[name].[ext]',
            limit: 1024 * 10
          }
        }
      },
      // 字体
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
        use: {
          loader: 'url-loader',
          options: {
            name: '[name].[ext]',
            limit: 1024 * 10
          }
        }
      },
      // 媒体文件
      {
        test: /\.(mp4|webm|ogg|mp3|wav|flac|aac)(\?.*)?$/,
        use: {
          loader: 'url-loader',
          options: {
            name: '[name].[ext]',
            limit: 1024 * 10
          }
        }
      }
    ]
  }
};
```

但现在有个问题，就是如果直接在`index.html`引入图片的话，可以顺利打包吗？

答案是不会的，我们可以测试一下。首先将图片引入`index.html`。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <img src="./image.png">
</body>
</html>
```

然后执行打包后，打包出来的`index.html`照样是`<img src="./image.png">`，但是我们并没有解析和打包出来`image.png`出来。

这时候我们需要借助另一个插件——`html-withimg-loader`。

```shell
# 0.1.16yarn add html-withimg-loader -D
```

然后我们再添加一条`rules`。

```javascript
{ test: /\.html$/,loader: 'html-withimg-loader' }
```

这时候打包成功后，`dist`文件成功将图片打包出来了，但是打开页面的时候，图片还是展示不出来。然后通过调试工具看的话，会发现

```html
<img src="{"default":"image.png"}">
```

这是因为`html-loader`使用的是`commonjs`进行解析的，而`url-loader`默认是使用`esmodule`解析的。因此我们需要设置一下`url-loader`。

```javascript
{  test: /\.(png|je?pg|gif|webp)$/,    use: {      loader: 'url-loader',        options: {          esModule: false,  // 不适用esmodule解析          name: '[name].[ext]',          limit: 1024 * 10        }    }}
```

这时候重新打包一下，页面就能成功展示图片了。

### Webpack5 资源模块

> [webpack.docschina.org/guides/asse…](https://link.juejin.cn?target=https%3A%2F%2Fwebpack.docschina.org%2Fguides%2Fasset-modules%2F)

在`webpack5`中，新添了一个资源模块，它允许使用资源文件（字体，图标等）而无需配置额外 `loader`，具体的内容大家可以看看文档，这里简单讲一下如何操作。

前面的例子，我们对静态资源都使用了`url-loader`或者`file-loader`，而在`webpack5`，我们甚至可以需要手动去安装和使用这两个`loader`，而直接设置一个`type`属性。

```javascript
{  test: /\.(png|jpe?g|gif|svg|eot|ttf|woff|woff2)$/i,  type: "asset/resource",}
```

然后打包测试后，静态文件都会直接打包成文件并自动引入，效果跟`file-loader`一致。

`type`值提供了四个选项：

- **`asset/resource`：** 发送一个单独的文件并导出 URL。之前通过使用 `file-loader` 实现。
- **`asset/inline` ：** 导出一个资源的 data URI。之前通过使用 `url-loader` 实现。
- **`asset/source ` ：**导出资源的源代码。之前通过使用 `raw-loader` 实现。
- **`asset`：**  在导出一个 data URI 和发送一个单独的文件之间自动选择。之前通过使用 `url-loader`，并且配置资源体积限制实现。

同时，我们可以在`output`设置输出`bundle`静态文件的名称：

```javascript
output: {  path: path.resolve(__dirname, 'dist/'),  filename: '[name].js',  // 设置静态bundle文件的名称  assetModuleFilename: '[name][ext]'}
```

## 清理打包路径

在每次打包前，我们其实都需要去清空一下打包路径的文件。

如果文件重名的话，`webpack`还会自动覆盖，但是实际中我们都会在打包文件名称中加入哈希值，因此清空的操作不得不实现。

这时候我们需要使用一个插件——`clean-webpack-plugin`。

```shell
yarn add clean-webpack-plugin -D
```

然后只需引入到配置文件且在`plugins`配置就可以使用了。

```javascript
const path = require('path');
// 引入CleanWebpackPlugin
const {CleanWebpackPlugin} = require('clean-webpack-plugin');

module.exports = {
    entry: './src/index.js',
    output: {
        path: path.resolve(__dirname, 'dist/'),
        filename: '[name].js',
        publicPath: ''
    },
    plugins: [
      	// 使用CleanWebpackPlugin
        new CleanWebpackPlugin(),
    ]
};
```

有些情况下，我们不需要完全清空打包路径，这时候我们可以使用到一个选项，叫`cleanOnceBeforeBuildPatterns`，它是一个数组，默认是`[**/*]`，也就是清理`output.path`路径下所有东西。而你可以在里面输入只想删除的文件，同时我们可以输入不想删除的文件，只需要在前面加上一个`!`。

> 需要注意的是，`cleanOnceBeforeBuildPatterns`这个选项是可以删除打包路径下之外的文件，只需要你配上绝对路径的话。因此`CleanWebpackPlugin`还提供了一个选项供测试——`dry`，默认是为`false`，当你设置为`true`后，它就不会真正的执行删除，而是只会在命令行打印出被删除的文件，这样子更好的避免测试过程中误删。

```javascript
const path = require('path');
const {CleanWebpackPlugin} = require('clean-webpack-plugin');

module.exports = {
    entry: './src/index.js',
    output: {
        path: path.resolve(__dirname, 'dist/'),
        filename: '[name].js',
        publicPath: ''
    },
    plugins: [
        new CleanWebpackPlugin({
          	// dry: true   // 打开可测试，不会真正执行删除动作
            cleanOnceBeforeBuildPatterns: [
                '**/*',  // 删除dist路径下所有文件
                `!package.json`,  // 不删除dist/package.json文件
            ],
        }),
    ]
};
```

## 文件归类

在目前我们的测试代码中，我们的`src`文件夹如下：

```shell
├── src
│   ├── Alata-Regular.ttf
│   ├── image.png
│   ├── index.html
│   ├── index.js
│   ├── style.css
│   └── style.scss
```

而正常项目的话，我们会使用文件夹将其分好类，这并不难，我们先简单归类一下。

```shell
├── src
│   ├── index.html
│   ├── js
│   │   └── index.js
│   ├── static
│   │   └── image.png
│   │   └── Alata-Regular.ttf
│   └── style
│       ├── style.css
│       └── style.scss

```

接下来，我们需要打包出来的文件也是归类好的，这里就不太复杂，直接用一个`assets`文件夹将所有静态文件放进去，然后`index.html`放外面。

```javascript
├── dist
│   ├── assets
│   │   ├── Alata-Regular.ttf
│   │   ├── image.png
│   │   ├── main.css
│   │   └── main.js
│   └── index.html
```

这里先补充一下`style.css`引入字体的代码：

```css
@font-face {
    font-family: "test-font";
    src: url("../static/Alata-Regular.ttf") format('truetype')
}

body {
    display: flex;
    font-family: "test-font";
}
```

首先，我们先将打包出来的`JavaScript`文件放入`assets`文件夹下，我们只需要修改`output.filename`即可。

```javascript
output: {
  path: path.resolve(__dirname, 'dist/'),
  filename: 'assets/[name].js'
}
```

其次，我们将打包出来的`css`文件也放入`assets`路径下，因为我们打包`css`是使用`miniCssExtractPlugin`，因此我们只需要配置一下`miniCssExtractPlugin`的`filename`即可：

```javascript
plugins: [
  ...
  new miniCssExtractPlugin({
    filename: "assets/[name].css"
  })
]
```

最后就是静态资源了，这里我们使用静态模块方案，所以直接修改`output.assetModuleFilename`即可：

```javascript
output: {
  path: path.resolve(__dirname, 'dist/'),
  filename: 'assets/[name].js',
  assetModuleFilename: 'assets/[name][ext]'
},
```

这时候打包一下，预览一下页面，发现都正常引入和使用。

