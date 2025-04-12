---
title: Node.js学习笔记
date: 2021-05-09 16:57:49
top_img: https://static001.geekbang.org/resource/image/bd/2e/bddfad3dc8fb2f7c4942a0dc1286c92e.jpg
cover: https://static001.geekbang.org/resource/image/bd/2e/bddfad3dc8fb2f7c4942a0dc1286c92e.jpg
tags:
- Node
categories:
- Node
aplayer: true
---
# node.js

Node.js是一个运行在服务端的JavsScript。

Node.js 是一个基于Chrome JavaScript 运行时建立的一个平台。

Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。

安装地址：[https://nodejs.org/en/download/](https://nodejs.org/en/download/)

### **NPM使用介绍**

NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题，常见的使用场景有以下几种：

- 允许用户从NPM服务器下载别人编写的第三方包到本地使用。
- 允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
- 允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

npm 安装 Node.js 模块语法格式如下：

```javascript
$ npm install <Module Name>
```

以下实例，我们使用 npm 命令安装常用的 Node.js web框架模块 express:

```javascript
$ npm install express
```

安装好之后，express 包就放在了工程目录下的 node_modules 目录中，因此在代码中只需要通过 require('express') 的方式就好，无需指定第三方包路径。

```javascript
var express = require('express');
```

### **全局安装与本地安装**

**本地安装**

1. 将安装包放在 ./node_modules 下（运行 npm 命令时所在的目录），如果没有 node_modules 目录，会在当前执行 npm 命令的目录下生成 node_modules 目录。
2. 可以通过 require() 来引入本地安装的包。

```javascript
npm install express          # 本地安装
```

**全局安装**

1. 将安装包放在 /usr/local 下或者你 node 的安装目录。
2. 可以直接在命令行里使用。

```javascript
npm install express -g   # 全局安装
```

**使用package.json**
package.json 位于模块的目录下，用于定义包的属性。

```javascript
$ npm init
```

**使用淘宝 NPM 镜像**

```javascript
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```

```javascript
$ cnpm install [name]
```

**express搭建简单服务器：**

```javascript
let express = require('express')//获取安装的模块

let app = express();

app.get('/', (req, res) => {
    res.send("这是首页")
})

app.listen(8080, () => {
    console.log('服务器启动', 'http://localhost:8080/');
})
```

### **模块的导入与导出**

CommonJs的规范：

- 模块引用：模块通过require方法来同步加载所依赖的模块
- 模块定义：在node中一个文件就是一个模块，提供export对象导出当前模块的方法或者变量
- 模块标识：模块标识传递给require()方法的参数，可以是按驼峰命名的方式，也可以是文件路径

**模块的导出：**
方法一：使用module.exports对象整体导出一个变量对象或者函数
方法二：可将需要导出的变量或者函数挂载到exports对象的属性上

```javascript
function fn() {
    console.log('fn');
}

let student = {
    username: 'Aiolimp'
}

console.log('student:', student.username);

//模块的导出 
//方法一：使用module.exports对象整体导出一个变量对象或者函数
// module.exports = { student, fn }
//方法二：可将需要导出的变量或者函数挂载到exports对象的属性上
exports.fn = fn 
```

**模块的导入：**

```javascript
let file1 = require('./index1')
//模块路径：
//1.如果有当前相对路径，则直接从相对路径的文件中引用
//2.如果没有相对路径则从当前目录的node_modules中引用，如果还是没有就往上一层级的node_modules查找一直到F盘的根目录为止

console.log('file:', file1);

file1.fn();
```

### **node文件的写入（操作文件）**

-  同步写入：

```javascript
//同步写入文件
//1.导入fs
let fs = require('fs')

//2.同步打开文件
//参数一：打开的文件格式 参数二：以写入的方式
let fd = fs.openSync('demo.html', 'w');

//3.写入内容
//参数一：写入的文件  参数二：写入的内容
let str = 'hello word';
fs.writeFileSync(fd, str);

//4.退出文件
fs.closeSync(fd)
```

- 文件异步写入

```javascript
//异步写入文件
let fs = require('fs')

fs.open('demo2.html', 'w', (err, fd) => {
    console.log('创建了文件');
    fs.writeFile(fd, '<h1>你好</h2>', (err) => {
        if (!err) {
            console.log('写入了文件');
            fs.close(fd, () => {
                console.log('关闭了文件');
            });
        } else {
            console.log(err);
        }
    })
})

console.log('我最先输出');//异步操作 不会等待
```

- 文件流写入

```javascript
//node文件流写入
//1.导入fs
let fs = require('fs')

//2.创建写入流
let ws = fs.createWriteStream('text1.txt')

//3.监听通道打开
ws.once('open', () => {
    console.log('通道打开了');
    ws.write('我真帅！')
    ws.write('我真厉害！')
    //写入结束
    ws.end()
})

//4.监听通道关闭
ws.once('close', () => {
    console.log('通道关闭了');
})
```

- 异步文件读取

```javascript
//node异步文件读取

let fs = require('fs')

fs.readFile('demo.html', (err, data) => {
    if (!err) {
        console.log(data.toString())//直接读取的是二进制文件 需要toString转换为字符串
    } else {
        console.log(err);
    }
})

console.log('读取数据');
```

-  异步文件读取并重新写入文件

```javascript
//node异步读取并重新写入文件

let fs = require('fs')
//1.读取文件
fs.readFile('1.jpg', (err, data) => {
    if (!err) {
        //2.重新写入文件
        fs.writeFile('2.jpg', data, (err) => {
            if (!err) {
                console.log('文件图片写入成功');
            } else {
                console.log(err);
            }
        })
    } else {
        console.log(err);
    }
})

console.log('读取数据');
```

- 通过管道读取并写入流(pipe)

```javascript
//通过管道读取并写入流 

let fs = require('fs')

let rs = fs.createReadStream('demo1.html')
let ws = fs.createWriteStream('demo2.html')

//创建管道，将读取流通过管道流出
rs.pipe(ws)
```

- 删除指定文件(unlink)

```javascript
//删除指定文件  unlink
let fs = require('fs')

fs.unlink('demo2.html', (err) => {
    if (!err) {
        console.log('文件删除成功');
    } else {
        console.log(err);
    }
})
```

- 读取当前目录下所有文件(readdir)

```javascript
//读取当前目录下所有文件  readdir

let fs = require('fs')

fs.readdir('./', (err, data) => {
    if (!err) {
        console.log(data);
    } else {
        console.log(err);
    }
})
```

- 创建目录（mkdir）

```javascript
//创建目录  mkdir

let fs = require('fs')

fs.mkdir('./img', () => {
    console.log('创建目录成功');
})

```

- 删除目录（rmdir）

```javascript
//删除目录   rmdir

let fs = require('fs')

fs.rmdir('./img', (err) => {
    if (!err) {
        console.log('删除目录成功');
    } else {
        console.log(err);//当目录不为空时，调用此方法不能删除目录
    }
})
```

- 递归删除空目录

```javascript
let fs = require('fs')

function delDir(dirpath) {
    var fileArr = fs.readdirSync(dirpath);
    for (var i in fileArr) {
        var filePath = dirpath + '/' + fileArr[i]

        //读取文件信息
        var stat = fs.statSync(filePath)
        //判断是文件还是目录
        if (stat.isFile()) {
            fs.unlinkSync(filePath)
        } else if (stat.isDirectory()) {
            delDir(filePath)//递归
        }
    }

    //删除空目录
    fs.rmdirSync(dirpath)
}

delDir('./img')

```

### **node事件**

```javascript
let fs = require('fs')
//引入events模块
let events = require('events')

//创建事件对象，实例化EventEmitter类
var eventLog = new events.EventEmitter();


//监听事件
eventLog.on('byDir', () => {
    console.log('创建目录事件触发');
})

fs.mkdir('./img', () => {
    console.log('目录创建成功');
})
//触发事件
eventLog.emit('byDir')
console.log('over');
```

### **Buffer缓冲区**

```javascript
//Buffer用于读取或者操作二进制数据流
//创建Buffer的方式：Buffer.from(),Buffer.alloc(),Buffer.allocUnsafe().

//1.Buffer.from()将字符串放置到缓冲区
let b1 = Buffer.from('10')
console.log(b1)
console.log(b1.toString());//二进制文件需要转换为字符串

//2.Buffer.alloc()初始化缓冲区，创建一个字节大小为10字节的缓冲区
//可以保证新创建的缓存区的数据不会包含旧的数据
let b2 = Buffer.alloc(20);
console.log(b2);

//3.Buffer.allocUnsafe()不会重置数据，不太安全
let b3 = Buffer.allocUnsafe(20);
console.log(b3);

b3[0] = 3
console.log(b3);
console.log(b3[0].toString());

```

### **node创建异步子进程**

index1.js

```javascript
console.log('进程：' + process.argv[2] + "执行");
```

- 通过exec创建异步子进程：衍生一个shell并在shell中执行命令command，且缓存任何产生的输出，有一个回调函数获知子进程的状况

```javascript
//通过exec创建异步子进程：衍生一个shell并在shell中执行命令command，且缓存任何产生的输出，有一个回调函数获知子进程的状况
const child_process = require('child_process');
for (var i = 0; i < 3; i++) {
    var wokerProcess = child_process.exec('node index1.js', i, (err, stdout, stderr) => {
        if (!err) {
            console.log('stdout:', stdout);
            console.log('stderr:', stderr);
        } else {
            console.log(err);
        }
    })

    wokerProcess.on('exit', (code) => {
        console.log('子进程已退出，状态码：', code);
    })
}
```

- 通过fork创建异步子进程：衍生新的nodejs进程，每一个进程都有自己的内存，使用自己的V8实例

```javascript
//通过fork创建异步子进程：衍生新的nodejs进程，每一个进程都有自己的内存，使用自己的V8实例
const child_process = require('child_process');
for (var i = 0; i < 3; i++) {
    //stdout:子进程的输出结果
    //stderr子进程的错误输出
    var wokerProcess = child_process.fork('index1.js', [i])

    wokerProcess.on('close', (code) => {
        console.log('子进程已退出，状态码：', code);
    })
}
```

- 通过spawn创建异步子进程：使用给定的command和args中的命令参数衍生一个新进程

```javascript
//通过spawn创建异步子进程：使用给定的command和args中的命令参数衍生一个新进程
const child_process = require('child_process');
for (var i = 0; i < 3; i++) {
    //stdout:子进程的输出结果
    //stderr子进程的错误输出
    var wokerProcess = child_process.spawn('node', ['index1.js', i])

    wokerProcess.stdout.on('data', (data) => {
        console.log('data:' + data);
    })
    wokerProcess.stderr.on('data', (data) => {
        console.log('err:' + data);
    })
    wokerProcess.on('close', (code) => {
        console.log('子进程已退出，状态码：', code);
    })
}
```

### node路径模块path

```javascript
console.log(__filename);//当前正在执行的脚本的文件名称
console.log(__dirname);//当前正在执行的脚步的目录名称

let path = require('path')

let srtPath = 'F:/node_prj/node_demo1/07_node路径模块path/index.png';
//获取扩展名
console.log(path.extname(srtPath));
console.log(path.extname(__filename));

//获取文件名称
console.log(path.basename(srtPath));
console.log(path.basename(__filename));

//获取目录名称
console.log(path.dirname(srtPath));
console.log(path.dirname(__filename));

//路径解析和规范化路径
console.log(path.normalize(srtPath));
//路径合并
console.log(path.join(__dirname + '/abc.png'));

//获取绝对路径合并
console.log(path.resolve(__dirname + '/abc.png'));

```

### **http搭建简易服务器**

```javascript
//导入node的http模块
let http = require('http')
//通过createServe获取http实例
let server = http.createServer();
//给服务器绑定接受请求的处理事件，当服务器接收到客户端发送的请求后，会调用后面的处理函数，处理函数接受两个参数
server.on('request', (res, req) => {
    console.log(req.url);
    res.end('hello word')
})
//绑定监听的端口号，可以添加回调函数，服务器启动后，触发回调函数
server.listen(3000, () => {
    console.log('服务器启动:', 'http://127.0.0.1:3000');
})
```

2.express实现静态服务器和自定义接口

```javascript
//导入express框架
let express = require('express')

//实例化服务器应用
let app = express()

//实现静态服务器
app.use(express.static('static'))

//自定义接口

app.get('/api/userList', (req, res) => {
    //请求的信息：req对象
    //响应的操作和信息：res对象
    res.json({
        state: "ok",
        user: [
            { username: "李白", sex: "男" },
            { username: "李白", sex: "男" },
            { username: "李清照", sex: "女" },
        ]
    })
})

//启动服务器
app.listen(3000, () => {
    console.log('启动了服务器：', 'http://127.0.0.1:3000');
})
```

调用接口

```javascript
<script>
    fetch('/api/userList').then(res => {
        return res.json()
    }).then(res => {
        console.log(res);
        res.user.forEach((item) => {
            let div = document.createElement('div')
            div.innerHTML = item.username + "的性别是" + item.sex
            document.body.appendChild(div)
        });
    })
</script>
```