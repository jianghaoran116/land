---
layout: post
title:  "js里的继承"
date:   2019-08-29
categories: javascript
---

# 继<sup>TM</sup>承
## 手写
### 原型继承
``` javascript
function A() {}

function B() {}

B.prototype = new A();

const b = new B();
```
![原型链继承](https://raw.githubusercontent.com/jianghaoran116/land/gh-pages/_posts/image/yxl.png "原型链继承")
### 借用构造函数
``` javascript
```
### 组合继承
``` javascript
```
## babel编译
``` javascript
class A {}
class B extend A {}
```
编译出:
``` javascript
```
