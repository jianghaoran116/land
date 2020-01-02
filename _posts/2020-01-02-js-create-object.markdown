---
layout: post  
title:  "create object"  
date:   2020-01-02  
categories: javascript  
tags: js基础  
---  
# 创建对象  
## 工厂方法  
- 所有的对象都无法识别  
``` javascript  
function createObj(name) {
  var o = new Object();
  o.name = name;
  o.getName = function() {
    return this.name;
  }

  return o;
}
```
## 构造函数模式  
- 所有的方法都会被实例化  
``` javascript  
function CreateObj(name) {
  this.name = name;
  this.getName = function() {
    return this.name
  }
}

var my = new CreateObj('my');
```
## 构造函数方式优化  
- 没有封装  
``` javascript  
function CreateObj(name) {
  this.name = name;
  this.getName = getName;
}

function getName() {
  return this.name
}

var my = new CreateObj('my');
```  
## 原型模式  
- 经典方法    
``` javascript  
function CreateObj(name) {
  this.name = name;
}

CreateObj.prototype = {
  constructor: CreateObj,
  getName: function() {
    return this.name
  }
}

var my = new CreateObj('my');
```  
## 寄生-构造函数-模式  
``` javascript  
function CreateObj(name) {
  var o = new Object();
  o.name = name;
  o.getName = function() {
    return this.name;
  }

  return o;
}

var my = new CreateObj('my');
```  