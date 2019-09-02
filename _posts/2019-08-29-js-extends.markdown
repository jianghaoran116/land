---
layout: post
title:  "js里的继承"
date:   2019-08-29
categories: javascript
tags: js基础
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
缺点: 共享引用的值，不能向父类的构造函数传参。  
### 借用构造函数  
``` javascript
function A(name) {
    this.name = name;
};

A.prototype = {
  constructor: A,
  sayName() {
    console.log(this.name);  
  },
};

function B(name, age) {
  this.name = name;
  this.age = age;
  A.call(this, name);
}

const b = new B('b', 12);
```  
缺点: 所有的函数都会被创建一遍。
### 组合继承  
``` javascript  
function A(name) {
  this.name = name;
};

A.prototype = {
  constructor: A,
  sayName() {
    console.log(`A + ${this.name}`);  
  };  
};

functioin B(name) {
  this.name = name;
  A.call(this, name);
};

B.prototype = Object.create(A.prototype);
B.prototype.constructor = B;

const b = new B('b');


```
## babel编译  
``` javascript
class A {
  constructor (name) {
  	this.name = name;
  };

  sayName () {
  	console.log(this.name);
  };
};

class B extends A {
	constructor (name) {
    	this.name = name;
    };
};

const b = new B('b');
b.sayName();
```
编译出:
``` javascripti
"use strict";

function _typeof(obj) {
  if (typeof Symbol === "function" && typeof Symbol.iterator === "symbol") {
    _typeof = function _typeof(obj) {
      return typeof obj;
    };
  } else {
    _typeof = function _typeof(obj) {
      return obj &&
        typeof Symbol === "function" &&
        obj.constructor === Symbol &&
        obj !== Symbol.prototype
        ? "symbol"
        : typeof obj;
    };
  }
  return _typeof(obj);
}

function _possibleConstructorReturn(self, call) {
  if (call && (_typeof(call) === "object" || typeof call === "function")) {
    return call;
  }
  return _assertThisInitialized(self);
}

function _assertThisInitialized(self) {
  if (self === void 0) {
    throw new ReferenceError(
      "this hasn't been initialised - super() hasn't been called"
    );
  }
  return self;
}

function _inherits(subClass, superClass) {
  if (typeof superClass !== "function" && superClass !== null) {
    throw new TypeError("Super expression must either be null or a function");
  }
  subClass.prototype = Object.create(superClass && superClass.prototype, {
    constructor: { value: subClass, writable: true, configurable: true }
  });
  if (superClass) _setPrototypeOf(subClass, superClass);
}

function _setPrototypeOf(o, p) {
  _setPrototypeOf =
    Object.setPrototypeOf ||
    function _setPrototypeOf(o, p) {
      o.__proto__ = p;
      return o;
    };
  return _setPrototypeOf(o, p);
}

function _instanceof(left, right) {
  if (
    right != null &&
    typeof Symbol !== "undefined" &&
    right[Symbol.hasInstance]
  ) {
    return !!right[Symbol.hasInstance](left);
  } else {
    return left instanceof right;
  }
}

function _classCallCheck(instance, Constructor) {
  if (!_instanceof(instance, Constructor)) {
    throw new TypeError("Cannot call a class as a function");
  }
}

function _defineProperties(target, props) {
  for (var i = 0; i < props.length; i++) {
    var descriptor = props[i];
    descriptor.enumerable = descriptor.enumerable || false;
    descriptor.configurable = true;
    if ("value" in descriptor) descriptor.writable = true;
    Object.defineProperty(target, descriptor.key, descriptor);
  }
}

function _createClass(Constructor, protoProps, staticProps) {
  if (protoProps) _defineProperties(Constructor.prototype, protoProps);
  if (staticProps) _defineProperties(Constructor, staticProps);
  return Constructor;
}

var A =
  /*#__PURE__*/
  (function() {
    function A(name) {
      _classCallCheck(this, A);

      this.name = name;
    }

    _createClass(A, [
      {
        key: "sayName",
        value: function sayName() {
          console.log(this.name);
        }
      }
    ]);

    return A;
  })();

var B =
  /*#__PURE__*/
  (function(_A) {
    _inherits(B, _A);

    function B(name) {
      var _this;

      _classCallCheck(this, B);

      _this.name = name;
      return _possibleConstructorReturn(_this);
    }

    return B;
  })(A);

var b = new B("b");
b.sayName();

```
关注:  
``` javascript
function _inherits(subClass, superClass) {
  if (typeof superClass !== "function" && superClass !== null) {
    throw new TypeError("Super expression must either be null or a function");
  }
  subClass.prototype = Object.create(superClass && superClass.prototype, {
    constructor: { value: subClass, writable: true, configurable: true }
  });
  if (superClass) _setPrototypeOf(subClass, superClass);
}
```  
可以获取到父类的静态方法。

Objec.setPrototype: 设置一个对象的原型，polyfill通过__proto__实现。

### 比较一下new和create操作  
#### new:   
* 创建一个新对象  
* 执行构造函数里的语句  
* 把this赋值给这个对象  
* 返回构造函数里的对象，如果没有则返回创建的新对象
### create:  
* 方法内定义一个新的空对象  
* 将obj.__proto__的对象指向传入的参数proto
* 将传入的对象属性复制到obj并且返回obj

