---
layout: post
title:  "webpack"
date:   2019-09-02
categories: webpack
tags: webpack
---

# webpack  

## PostCss autoprefixer补齐css3前缀  
- px2rem-loader  
- [lib-flexible](https://github.com/amfe/lib-flexible)  
## 资源内联  
- 页面框架脚本的初始化  
- 上报相关打点  
- css内联避免页面闪动  
- 减少http请求  
- 小图片或字体内联  
### HTML和JS内联raw-loader  
``` javascript  
  <script>${require('raw-loader!babel-loader!../node_modules/lib-flexible')}</script>
```  
### CSS内联  
- style-loader  
``` javascript  
rules: [{
  test: /\.scss$/,
  use: [{
    loader: 'style-loader',
    options: {
      insertAt: 'top', // 样式插入到<head>
      singleton: true, //将所有的style标签合并成一个
      }
    },
    "css-loader",
    "sass-loader"
  ]},
]}
```  
- html-inline-css-webpack-plugin  

## 多页打包  
一个页面一个入口 一个html-webpack-plugin  
动态获取入口和设置html-webpack-plugin数量  
- glob.sync  

## Source map  
Source map就是一个信息文件，里面储存着位置信息.
- source map: 产⽣生.map文件  
- eval: 使⽤用eval包裹模块代码  
- cheap: 不不包含列列信息  
- inline: 将.map作为DataURI嵌入，不单独生成.map文件  
- module: 包含loader的sourcemap

## 基础库分离  
- 通过CDN引入 html-webpack-externals-plugin  
- CommonsChunkPlugin插件 webpack4内置了SplitChunksPlugin  

## three shaking DCE(Dead Code Elimination)  
利利⽤用ES6模块的特点:
- 只能作为模块顶层的语句句出现  
- import 的模块名只能是字符串串常量量  
- import binding是immutable的  
代码擦除：
uglify阶段删除无用代码

### webpack的模块机制  
包裹在匿名闭包里  
modules是一个一个的函数数组 函数用于初始化模块  
__webpack_require⽤用来加载模块，返回module.exports  
通过WEBPACK_REQUIRE_METHOD(0)启动程序  

## scope hoisting  
原理：将所有模块的代码按照引⽤顺序放在⼀个函数作用域⾥，然后适当的重命名  
通过scope hoisting可以减少函数声明代码和内存开销

## 代码分割  
动态import  

## ssr

## 构建日志  

## 构建异常和中断  

- 代码压缩  
- 减少闭包  
- 预编译?  


