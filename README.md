# react-demo

## 1.初始化项目
```
$ npm init
```
建立src和build两个目录

```
# src 目录存放源码，build 目录存放编译打包之后的资源
$ mkdir src build
$ cd src && touch index.js component.js
$ cd ../build && touch index.html
```
```
/* src/index.js */
var component = require('./component.js');

component();
```

```
/* src/component.js */
module.exports = function(){
  alert('component');
}
```

```
/* build/index.html */
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Webpack demos</title>
</head>
<body>
  <div id="app"></div>
  <script src="bundle.js"></script>
</body>
</html>
```

下载webpack和webpack-dev-server
```
# 安装并保存在项目的依赖中
$ npm install webpack webpack-dev-save --save-dev
```

创建webpack的配置文件
```
$ touch webpack.config.js

# 请注意webpack.config.js这个文件命名，默认情况下需要严格按照这个命名，不然会报Output filename not configured的错误；另外，如果不按这个命名，那么在webpack运行的时候通过--conf这个参数指定配置文件，比如：webpack --config conf.js
```

进行基本配置
```
var path = require('path');

module.exports = {
    entry: path.resolve(__dirname, 'src/index.js'),
    output: {
        path: path.resolve(__dirname, 'build'),
        filename: 'bundle.js'
    },
};
```

执行webpack命令
```
$ webpack
```
build目录下也新增了一个bundle.js文件

### 2.webpack和webpack-dev-server的基本命令
```
$ webpack --help
```

执行以上命令，可以在控制台看到很多webpack相关的命令，选取几个常用的介绍下。

```
webpack 开发环境下编译
webpack -p 产品编译及压缩
webpack --watch 开发环境下持续的监听文件变动来进行编译(非常快!)
webpack -d 引入 source maps
webpack --progress 显示构建进度
webpack --display-error-details 这个很有用，显示打包过程中的出错信息
webpack --profile 输出性能数据，可以看到每一步的耗时
```

另外，让我们使用webpack-dev-server来起一个本地服务进行调试：
```
$ webpack-dev-server --progress --colors --content-base build
```
打开`localhost:8080`，回车即可。

那么执行`webpack-dev-server`后面的几个参数是什么意思呢？
```
webpack-dev-server - 在 localhost:8080 建立一个 Web 服务器
webpack-dev-server --devtool eval - 为你的代码创建源地址。当有任何报错的时候可以让你更加精确地定位到文件和行号
webpack-dev-server --progress - 显示合并代码进度
webpack-dev-server --colors - 命令行中显示颜色
webpack-dev-server --content-base build - webpack-dev-server服务会默认以当前目录伺服文件，如果设置了content-base的话，服务的根路径则为build目录
webpack-dev-server --inline 可以自动加上dev-server的管理代码，实现热更新
webpack-dev-server --hot 开启代码热替换，可以加上HotModuleReplacementPlugin
webpack-dev-server --port 3000 设置服务端口
```

- 关于webpack-dev-server的简单介绍：webpack-dev-server是一个小型的node.js Express服务器,它使用webpack-dev-middleware中间件来为通过webpack打包生成的资源文件提供Web服务。它还有一个通过Socket.IO连接着webpack-dev-server服务器的小型运行时程序。webpack-dev-server发送关于编译状态的消息到客户端，客户端根据消息作出响应。