---
layout: post  
title:  "extends"  
date:   2020-01-02  
categories: javascript
tags: js
---  
# 继承:  
## 原型链继承  
- 引用类型的属性会被共享  
- 无法向构造函数传参数  
``` javascript  
function Parent() {
  this.name = 'parent'
}

Parent.prototype = {
  constructor: Parent,
  getName: function() {
    return this.name;
  }
}

Child.prototype = new Parent();

function Child() {

}

var child = new Child();
console.log(child.getName());
```  
## 借用构造函数  
- 每个方法都会被实例化    
``` javascript  
function Parent(name) {
  this.name = name;
  this.getName = function() {
    return this.name;
  }
}

function Child(name) {
  Parent.call(this, name)
}

var child = new Child('child');
console.log(child.getName());
```  
## 组合继承  
- 会运行两次构造函数  
``` javascript 
function Parent(name) {
  this.name = name;
}

Parent.prototype = {
  constructor: Parent,
  getName: function() {
    return this.name;
  }
}

function Child(name) {
  Parent.call(this, name)
}

Child.prototype = new Parent();
Child.prototype.constructor = Child;

var child = new Child('child');
console.log(child.getName());
```
## 寄生组合式继承  
``` javascript  
function prototype(child, parent) {
  var F = function f() {};
  F.prototype = parent.prototype;
  var prototype = new F();
  prototype.constructor = child;
  child.prototype = prototype;
}

function Parent(name) {
  this.name = name;
}

Parent.prototype = {
  constructor: Parent,
  getName: function() {
    return this.name;
  }
}

function Child(name) {
  Parent.call(this, name)
}

prototype(Child, Parent);

var child = new Child('child');
console.log(child.getName());
console.log(child)
```  
  
- 静态方法？  