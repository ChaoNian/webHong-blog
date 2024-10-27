---
highlight: ally-dark
theme: channing-cyan
---
# 使用webapck 从零创建  react 工程

## 1、首先使用 npm init 创建一个前端项目
```shell
mkdir my-app
cd my-app
npm init -y
```
## 2、安装 webpack
```shell
npm i -D webpack webpack-cli webpack-dev-server html-webpack-plugin
```
webpack - 前端构建工具
webpack-cli - 让 webpack 支持命令行执行
webpack-dev-server - 开发模式下启动服务器，修改代码，浏览器会自动刷新。


## 3、 安装 babel
babel： 可以将 es6 代码转变为 es5，
@babel/preset-react： 让 babel 支持 react 的预设
babel-loader：是让 webpack 支持 babel 的加载器
```javascript
npm i -D @babel/core @babel/preset-env @babel/preset-react babel-loader
```
在项目更目录新建一个 `babel.config.js` 文件，将安装的 babel 写入这个文件，babel 会在运行前读取这份配置文件

babel.config.js
```javascript
module.exports = {
  presets: ['@babel/preset-env', '@babel/preset-react'],
}

```

## 安装 CSS 加载器
```shell
npm i -D style-loader css-loader
```
css-loader 用于解析 css 文件； `style-loader` 会通过使用多个 `<style></style>`标签的形式自动把 styles 插入到 DOM 中。

## 安装 react 和 react-dom
```shell
npm i react react-dom
```
## 新建文件
建一个 `index.html` 文件
创建一个在public目录，并且在下面新建一个index.html 文件。

新建一个 `index.js` 文件
创建一个名为 src 的文件夹，所有源代码都放在该目录下，在src目录下，创建index.js文件，该文件也就是 webpack 构建的入口文件
创建文件

 `src/index.js`: webpack 构建的入口文件
 ```js
 import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App'
const root = ReactDOM.createRoot(document.getElementById('root'));

console.log(React);

root.render(
    <React.StrictMode>
      <App />
    </React.StrictMode>
  );
```
 `src/App.js`  组件
  ```js
  import React from 'react'
import './App.css'
function App() {
    return (
        <div>
            <header>
                头
            </header>
            <p>内容</p>
            <footer>脚</footer>
        </div>
    )
}
export default App
```
 在项目根目录创建一个 `webpack.config.js` 文件，   `webpack.config.js` 是 webpack 的默认配置文件名

 ![alt text](image.png)

 文件webpack.config.js：
 ```javascript
 const path = require('path')
const HtmlWebpackPlugin = require('html-webpack-plugin')

module.exports = {
    entry: './src/index.js',
    output: {
        path: path.join(__dirname, '/dist'),
        filename: 'bundle.js',
        clean: true
    },
    devtool: 'source-map',
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: /node_modules/,
                use: {
                    loader: 'babel-loader',
                }
            },
            {
                test: /\.css$/i,
                use: ['style-loader', 'css-loader'],
              },
        ]
    },
    plugins: [
        new HtmlWebpackPlugin({
            template: './public/index.html'
        })
    ],
    devServer: {
        proxy: [
            {
                context: ['/auth', '/api'],
                target: 'http://localhost:3000'
            }
        ]
    }
}
 ```

package.json
 ```json
 {
  "name": "react-demo",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack-dev-server --mode development --hot --open",
    "build": "webpack --mode production"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": "",
  "devDependencies": {
    "@babel/core": "^7.26.0",
    "@babel/preset-env": "^7.26.0",
    "@babel/preset-react": "^7.25.9",
    "babel-loader": "^9.2.1",
    "css-loader": "^7.1.2",
    "html-webpack-plugin": "^5.6.3",
    "style-loader": "^4.0.0",
    "webpack": "^5.95.0",
    "webpack-cli": "^5.1.4",
    "webpack-dev-server": "^5.1.0"
  },
  "dependencies": {
    "react": "^18.3.1",
    "react-dom": "^18.3.1"
  }
}

 ```

 ## 使用create-react-app 脚手架 创建项目
 1、创建步骤
        安装脚手架[mac 电脑需要加sudo]
        ```shell
            npm i create-react-app -g 
        ```
        检查安装情况
        ```shell
            create-react-app --version 
        ```
    基于脚手架创建React 工程化的项目
    ```shell
        create-react-app project-name
    ```
    或者
     ```shell
        npx create-react-app project-name
    ```
    注意 项目名字遵循 npm包命名规范，使用 数字、小写字母、_

项目目录
    |- node_modules
    |- src
        |- index.js
    |- public
        |- index.html
    |- package.json
    |- ...
 使用脚手架创建项目发现没有配置文件；哪为什么呢？
 一个React 项目中， 默认会安装：
   react： React 框架的核心
   react-dom: React 视图渲染的核心【基于React 构建WebApp HTML页面】
   React-native:构建和渲染App
   reat-scripts: 脚手架为了让项目目录看起来干净一些，把webpack打包的规则及相关插件/Loader 等都隐藏到node_modules目下，react-scripts 就是脚手架中自己对打包命令的一种封装，会调用node_modules 中的webpack等进行处理


## 使用Vite 创建项目
如果你的项目代码量比较大，或者你厌恶了 webpack 的打包速度，那么你可以选择使用 vite 来创建你的 React 应用。

vite 采用浏览器支持 ES 模块来解决开发时构建缓慢的问题，使用 esbuild 预构建依赖（开发时不会变动的纯 JavaScript 代码，一般是 node_modules 中的第三方包）。

vite 不但支持 vue 还支持 react、preact、svelte 等框架和原生 js。

使用 create-vite 创建应用
使用 vite 创建项目也非常简单
```shell
npm create vite@latest
```
我们可以在命令行中选择需要使用的的框架

选择使用 JavaScript 还是 typescript 开发
使用 npm run dev 启动，

资料参考

https://cloud.tencent.com/developer/article/2240912

https://juejin.cn/post/7110535158863757319