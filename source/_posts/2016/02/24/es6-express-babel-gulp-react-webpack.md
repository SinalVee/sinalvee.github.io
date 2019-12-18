title: ES6 + Express + Babel + Gulp + React + Webpack
date: 2016-02-24 15:06:58
updated: 2016-02-24 15:06:58
categories:
  - NodeJS
tags:
  - nodejs
  - es2015
  - express
  - babel
  - gulp
---

自己动手才发现原来写篇好文章真的是太难了，如果评个等级的话，这篇文章大概是渣渣水平，哈哈。
不过，总算是写完了，也算是最近学习的一个总结吧 :)

## 本文目录
* [准备工作](#prepare)
* [项目目录结构](#toc)
* [Express](#express)
  * [安装](#express-install)
  * [写个server测试一下](#express-server)
* [Babel](#babel)
  * [安装](#babel-install)
  * [配置.babelrc](#babelrc)
  * [使用es6语法编写代码](#babel-es6)
  * [使用bebel转换](#babel-transform)
* [Gulp](#gulp)
  * [安装](#gulp-install)
  * [编写gulpfile](#gulpfile)
    * [一个简单的gulpfile](#gulpfile-default)
    * [能够监听文件更改的gulpfile](#gulpfile-watch)
* [React + Webpack](#react-webpack)
  * [安装](#react-webpack-install)
  * [编写webpack.config.js](#webpack-config)
  * [在gulp中使用webpack](#gulp-webpack)
  * [使用React](#react)
* [总结](#summary)

<!-- more --> 

<a id="prepare"></a>
## 准备工作
作为一个noder，开始一切之前当然少不了`npm init`
关于express, babel, gulp, webpack, react本文不进行介绍，如有需要可以到其主页自行了解。
[Express](http://expressjs.com/)
[Babel](https://babeljs.io/)
[Gulp](http://gulpjs.com/)
[React](https://facebook.github.io/react/)
[Webpack](https://webpack.github.io/)

<a id="toc"></a>
## 项目目录结构
```
app
├─ lib/
├─ public/
│  ├─ dist/
│  └─ src/
│      └─ index.js
├─ src/
│  └─ app.js
├─ .babelrc
├─ index.html
├─ gulpfile.babel.js
├─ package.json
├─ README.md
└─ webpack.config.js
```

<a id="express"></a>
## Express

<a id="express-install"></a>
### 安装
这次准备纯手写，所以不用`express-generator`生成了，简单的示例尽量不使用多余的中间件。
`npm install --save express body-parser`

<a id="express-server"></a>
### 写个server测试一下
```js
// src/app.js

var express = require('express');
var bodyParser = require('body-parser');

var app = express();
var router = express.Router();

app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
app.use(cookieParser());

router.get('/', function(req, res, next) {
  res.end('Hello World!');
});

app.use('/', router);

app.listen(3000, function() {
  console.log('server listening at port 3000...');
});
```
简单起见，404和error handler就不写了。
`node src/app.js`跑起来，浏览器打开[http://localhost:3000](http://localhost:3000)，可以看到页面中显示`Hello World!`了。

<a id="babel"></a>
## Babel
babel升级到6.x之后，需要明确指定转换，我们使用预设的es2015。

<a id="babel-install"></a>
### 安装
`npm install -g babel-core`
`npm install --save-dev babel-core babel-preset-es2015`

<a id="babelrc"></a>
### 配置.babelrc
在根目录新建.babelrc文件，配置如下：
```
{
  "presets": ["es2015"]
}
```
<a id="babel-es6"></a>
### 使用es6语法编写代码
我们现在将之前写过的`src/app.js`修改为es6语法
```
// src/app.js

import express, { Router } from 'express';
import bodyParser from 'body-parser';

let app = express();
let router = Router();

app.use(bodyParser.json());
app.use(bodyParser.urlencoded({ extended: true }));
app.use(cookieParser());

router.get('/', (req, res, next) => {
  res.end('Hello World!');
});
app.use('/', router);

app.listen(3000, () => {
  console.log('server listening at port 3000...');
});
```

<a id="babel-transform"></a>
### 使用bebel转换
将代码修改为es6语法之后，就可以使用babel进行转换了。
使用命令`babel src -d lib`
可以看到控制台中显示：
```
src/app.js -> lib/app.js
```
之后还是运行一下看看效果，浏览器打开[http://localhost:3000](http://localhost:3000)，我们和熟悉的`Hello World`又见面啦。

<a id="gulp"></a>
## Gulp
每次修改完代码都要重新用babel转换一次是不是很麻烦？别怕，我们有gulp。

<a id="gulp-install"></a>
### 安装
```
npm install -g gulp-cli
npm install --save-dev gulp gulp-babel
```

<a id="gulpfile"></a>
### 编写gulpfile
在根目录新建一个gulpfile.babel.js文件。
gulp原生并不支持es6语法，但是我们可以告诉gulp使用babel将gulpfile转换为es5，方法就是将gulpfile命名为gulpfile.babel.js

<a id="gulpfile-default"></a>
#### 一个简单的gulpfile
我们先写一个简单的gulpfile测试一下gulp是否能够正常工作。
```
import gulp from 'gulp';
import babel from 'gulp-babel';

gulp.task('default', () => {
  return gulp.src('src/**/*.js')
    .pipe(babel())
    .pipe(gulp.dest('lib'));
});
```
控制台执行`gulp`，可以看到：
```
[13:00:35] Requiring external module babel-register
[13:00:36] Using gulpfile /path/to/file/gulpfile.babel.js
[13:00:36] Starting 'default'...
[13:00:36] Finished 'default' after 191 ms
```
然后lib文件夹下就生成了转换后的文件了。

<a id="gulpfile-watch"></a>
#### 能够监听文件更改的gulpfile
虽然上面的gulpfile能够将使用了，但是还是跟之前直接用babel一样，每次都要gulp一下，下面我们就来写一个能够监听文件的gulpfile。当文件被修改之后，自动将文件转换。
这里使用`gulp-watch`监听文件更改。
`npm install --save-dev gulp-watch`
```
import gulp from 'gulp';
import watch from 'gulp-watch';
import babel from 'gulp-babel';

gulp.task('transform', () => {
  return gulp.src('src/**/*.js')
    .pipe(babel())
    .pipe(gulp.dest('lib'));
});

gulp.task('watch', () => {
  return gulp.src('src/**/*.js')
    .pipe(watch('src/**/*.js', {
      verbose: true
    }))
    .pipe(babel())
    .pipe(gulp.dest('lib'));
});

gulp.task('default', () => {
  gulp.start('transform');
});
```
控制台执行`gulp watch`
```
[14:11:46] Requiring external module babel-register
[14:11:47] Using gulpfile /file/to/path/gulpfile.babel.js
[14:11:47] Starting 'watch'...
```
然后修改app.js并保存，可以看到控制台中多了一行信息
```
[14:13:20] app.js was changed
```
对于gulp其他的用法这里就不细说了，有兴趣的可以去看桑大`i5ting`的[Gulp实战和原理解析（以weui作为项目实例）](http://i5ting.github.io/stuq-gulp/)

<a id="react-webpack"></a>
## React + Webpack

<a id="react-webpack-install"></a>
### 安装
react
`npm install --save react react-dom`
webpack
`npm install --save-dev webpack webpack-dev-server`
为了编译jsx，我们还需要其他一些模块
`npm install --save-dev babel-loader babel-preset-react`

<a id="webpack-config"></a>
### 编写webpack.config.js
```
var path = require('path');
var webpack = require('webpack');

module.exports = {
  devtool: 'eval',
  entry: {
    public: './public/src/index'
  },
  output: {
    path: path.join(__dirname, 'public/dist'),
    filename: 'bundle.js',
    publicPath: 'public/dist'
  },
  plugins: [],
  module: {
    loaders: [
      {
        test: /\.js$/,
        include: [path.join(__dirname, 'public/src')],
        loader: 'babel',
        query: {
          presets: ['react', 'es2015']
        }
      }
    ]
  }
};

```

<a id="gulp-webpack"></a>
### 在gulp中使用webpack
修改`gulpfile.babel.js`，添加`build`和`webpack-dev-server`任务，分别是生成打包文件和启动开发服务器，并将`default`修改为同时执行`transform`和`webpack-dev-server`。
```
import gulp from 'gulp';
import gutil from 'gulp-util';
import watch from 'gulp-watch';
import babel from 'gulp-babel';
import webpack from 'webpack';
import WebpackDevServer from 'webpack-dev-server';

import webpackConfig from './webpack.config.js';

// transform
gulp.task('transform', () => {
  return gulp.src('src/**/*.js')
    .pipe(babel())
    .pipe(gulp.dest('lib'));
});

// watch transform
gulp.task('watch-transform', () => {
  return gulp.src('src/**/*.js')
    .pipe(watch('src/**/*.js', {
      verbose: true
    }))
    .pipe(babel())
    .pipe(gulp.dest('lib'));
});

gulp.task('webpack:build', (callback) => {
  // modify some webpack config options
  var myConfig = Object.create(webpackConfig);
  myConfig.plugins = myConfig.plugins.concat(
    new webpack.DefinePlugin({
      'process.env': {
        // This has effect on the react lib size
        'NODE_ENV': JSON.stringify('production')
      }
    }),
    new webpack.optimize.DedupePlugin(),
    new webpack.optimize.UglifyJsPlugin()
  );

  // run webpack
  webpack(myConfig, (err, stats) => {
    if (err)
      throw new gutil.PluginError('webpack:build', err);
    gutil.log('[webpack:build]', stats.toString({
      colors: true
    }));
    callback();
  });
});

gulp.task('webpack-dev-server', (callback) => {
  // modify some webpack config options
  var myConfig = Object.create(webpackConfig);
  myConfig.devtool = 'eval';
  myConfig.debug = true;

  // Start a webpack-dev-server
  new WebpackDevServer(webpack(myConfig), {
    publicPath: '/' + myConfig.output.publicPath,
    stats: {
      colors: true
    }
  }).listen(3001, 'localhost', (err) => {
    if (err) throw new gutil.PluginError('webpack-dev-server', err);
    gutil.log('[webpack-dev-server]', 'http://localhost:3001/');
  });
});

gulp.task('default', ['watch-transform', 'webpack-dev-server']);
```

<a id="react"></a>
### 使用React
在根目录下创建`index.html`文件

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>Hello Wrold</title>
  </head>
  <body>
    <div id="root"></div>
    <script src="http://127.0.0.1:3001/public/dist/bundle.js"></script>
  </body>
</html>
```

之后在`public/src`目录创建`index.js`文件，简单的写个hello world.

```js
import React, { Component } from 'react';
import ReactDOM from 'react-dom';

class App extends Component {
  render() {
    return (
      <h1>Hello Wrold!!</h1>
    );
  }
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```
控制台执行`gulp webpack-dev-server`，打开[http://localhost:3001](http://localhost:3001/)
我们和`Hello world!`第三次见面了～

<a id="summary"></a>
## 总结
到这里文章就结束了，然而这只是文章的结束，现在只是把大体的结构搭建起来了，剩下要做的还有很多，继续加油把～
PS: 我也不知道这种目录结构是否合理，如有不同意见，欢迎拍砖～
