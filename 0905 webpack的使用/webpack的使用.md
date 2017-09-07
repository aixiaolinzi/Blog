Webpack的使用

##前言

首次接触Webpack。

##任务

了解Webpack

##开始Webpack之旅
Webpack可以理解为打包工具。对NPM的支持和Community的支持。
特点：：
- 同时支持Commonjs与AMD
- 一切都可以打包
- 分模块打包

**1.安装webpack：**
```groovy
//全局安装
npm install -g webpack
//安装到你的项目目录
npm install --save-dev webpack
```
**2.初始化项目**
```groovy
npm init
```
**3.我们在本项目中安装Webpack作为依赖包**
```groovy
// 安装Webpack
npm install --save-dev webpack
```
**4.我们在本项目中安装webpack-dev-server作为依赖包**
```groovy
npm install webpack-dev-server -g
```


>热加载问题，
>在webpack-dev-server 后面添加--hot，--inline；
>
>



###Loaders
鼎鼎大名的Loaders登场了！
#####Babel的安装与配置
```groovy
// npm一次性安装多个依赖模块，模块之间用空格隔开
npm install --save-dev     babel-core babel-loader babel-preset-es2015 babel-preset-react
```
在webpack中配置Babel的方法如下:
```groovy
module.exports = {
    entry: __dirname + "/app/main.js",//已多次提及的唯一入口文件
    output: {
        path: __dirname + "/public",//打包后的文件存放的地方
        filename: "bundle.js"//打包后输出文件的文件名
    },
    devtool: 'eval-source-map',
    devServer: {
        contentBase: "./public",//本地服务器所加载的页面所在的目录
        historyApiFallback: true,//不跳转
        inline: true//实时刷新
    },
    module: {
        rules: [
            {
                test: /(\.jsx|\.js)$/,
                use: {
                    loader: "babel-loader",
                    options: {
                        presets: [
                            "es2015", "react"
                        ]
                    }
                },
                exclude: /node_modules/
            }
        ]
    }
};
```

现在你的webpack的配置已经允许你使用ES6以及JSX的语法了。继续用上面的例子进行测试，不过这次我们会使用React，记得先安装 React 和 React-DOM
```groovy
npm install --save react react-dom
```

loader加载的时候：`全称·
```groovy
require('style-loader!css-loader!./admin.css');
```
另外的加载方式：：
```groovy
    module: {
        rules: [
            {
                test: /(\.jsx|\.js)$/,
                use: {loader: "babel-loader"},
                exclude: /node_modules/
            },
            {
                test: /\.css$/,
                use: [
                    {loader: "style-loader"},
                    {loader: "css-loader"}
                ]
            },
            {
                test: /images/,
                use: [
                    {loader: "file-loader"}
                ]
            },
            {
                test: /icons/,
                use: [
                    {loader: "url-loader"}
                ]
            },
            {
                test: /\.scss$/,
                use: [
                    {loader: "style-loader"},
                    {loader: "css-loader"},
                    {loader: "sass-loader"}
                ]
            },
        ]
    }
```

> 加载的时候出错：：
> Module build failed: Error: Cannot find module 'node-sass'
> 解决方案
> ```groovy
> npm install --save-dev node-sass
> 项目中加载node-sass
> ```

