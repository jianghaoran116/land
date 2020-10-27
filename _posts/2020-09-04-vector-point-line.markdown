---
layout: post  
title: "向量"  
date: 2020-09-04  
categories: threeJs  
tags: threeJs vector  
---  

简单的基于向量和矩阵运算的数学体系

坐标系与坐标映射  
- 转换坐标系  
  而WebGL本身不提供tranform的API，但我们可以在shader里做矩阵运算来实现坐标转换，

  canvas里画一些图形:  
  ``` javascript  
  ```  

  它能够简化计算量，这不仅让代码更容易理解，也可以节省 CPU 运算的时间

  那在直角坐标系下，我们是怎么表示点和线段的呢？
  - 向量的加法  
    V1 + V2 V1向量的终点(x1, y1)沿着V2向量的方向移动了一段距离。  
  - 一个向量包含有长度和方向信息  
    ``` javascript  
      // 向量的长度
      v.length = function(){return Math.hypot(this.x, this.y)};
    ```  
    ``` javascript  
      // 向量的角度可以用通过和X轴的夹角来表示  
      // Math.atan2 的取值范围是 -π到π，负数表示在 x 轴下方，正数表示在 x 轴上方
      v.dir = function() { return Math.atan2(this.y, this.x);}
    ```  

- 如何用向量和坐标系描述点和线段  
- 向量乘法知识  
  - 点乘  
  - 叉乘  
  在向量乘法里，如果 a、b 都是长度为 1 的归一化向量，那么|a X b| 的结果就是 a、b 夹角的正弦值，而|a • b|的结果就是 a、b 夹角的余弦值。
- 如何用向量和参数方程描述曲线  
- 如何利用三角剖分和向量操作描述并处理多边形  
- 如何用仿射变换对几何图形进行坐标变换  

- 投影矩阵  
- 视图矩阵  
- 透视投影矩阵  
- 正交投影矩阵  

通过 https://github.com/alexjoverm/typescript-library-starter 学习前端工程化  
复杂的系统性的问题  

- 包管理工具  
  将自己写好的模块化代码发布到平台上，任何人也可以下载其他人的模块化代码，bower、npm、yarn  
  npm:  
  - name  
  - version  
  - main: 默认加载的入口文件  
  - scripts: 定义一些脚本  
  - dependencies: 运行时需要的模块  
  - devDependencies: 本地开发需要的运行的模块  
  - optionalDependencies: 可选的模块，即使安装失败也会正常退出  
  - peerDependencise: 必要的版本依赖  
  - 版本号 ^1.2.3最终会安装1.x.x的最新版本 ~1.2.3最终会安装1.2.x的最新版本  
  yarn:  
  优势是并发和快  

  npx  

  存储的平台 同时有一套规范实现包的更新等操作  

- 源代码静态检查和格式化工具  
  - 统一代码风格，写出"正确"代码  
  - .eslintrc: rc - runtime config  

  ES6及其他泛JS语言  
  - babel  
    - babel-core 核心的编译和生成代码的方法  
    - babel-cli (commond line tool)  
    - babel/preset-env 告诉babel把代码编译成什么东西(can i use)
      编译了语法层面的东西 const、() => {}、"es6 modules" ...  
    - babel/polyfill 补充缺失的特性（兼容一些方法）runtime polyfill、运行时兼容  
      arr.includes()、promise ...

- 打包工具  
  - JS模块化  
    commonJs、AMD、ES6 module 通过打包工具来实现这些模块能自由切换  
    - browserify  
    - rollup 最早提出的treeshaking  

    commonJS:  
    ``` javascrip  
      const test = require()

      exports - modules.exports
    ```  

- 压缩工具  
  uglify  

其他工具(前端解决方案)  
  grunt gulp webpack  


## vue-cli  
cli全局命令行vue  

# https://github.com/alexjoverm/typescript-library-starter  
## 打包的输出路径  
通过读取package.json里的main字段(umd)和module(es5)字段和typings(ts)字段  


## 配置信息  
- .editorconfig  
  在这里配置的代码规范规则优先级高于编辑器默认的代码格式化规则  
  ``` javascript  
  [*]
  indent_style = space
  end_of_line = lf // unix样式的换行符，在每个文件后面都有一个换行符
  charset = utf-8
  trim_trailing_whitespace = true
  insert_final_newline = true
  max_line_length = 100
  indent_size = 2

  [*.md]
  trim_trailing_whitespace = false
  ```  
- .travis.yml  
- tsconfig.json  
- tslint.json  
- tools/
