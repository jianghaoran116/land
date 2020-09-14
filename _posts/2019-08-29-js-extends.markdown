---
layout: post  
title: "面向对象 原型 原型链 继承"  
date: 2019-08-29  
categories: javascript  
tags: js  
---

主要讲一下JS中的面向对象  
来个问题先  
![](https://raw.githubusercontent.com/jianghaoran116/land/gh-pages/_posts/image/object-1.png)  
然后如何把 prototype, \__proto__, constructor对应上  

- #### prototype  
每个函数都有一个prototype属性。*prototype是函数才会有的属性*  
这个函数的prototype属性指向该对象的原型  

- #### __proto__  
每一个JavaScript对象(除了null)都具有的一个属性, 叫__proto__, 这个属性会指向该对象的原型。  
*当使用 obj.__proto__ 时，可以理解成返回了 Object.getPrototypeOf(obj)*  

- #### constructor  
原型对象指向构造函数  

![](https://raw.githubusercontent.com/jianghaoran116/land/gh-pages/_posts/image/object-2.png)  

``` javascript  
  function Person() {}
  var person = new Person();
  console.log(person.__proto__ === Person.prototype);
```  

没有 Object.create、Object.setPrototypeOf 的早期版本中，**new运算是唯一一个可以指定[[prototype]]的方法**。  

### 原型的原型  
![](https://raw.githubusercontent.com/jianghaoran116/land/gh-pages/_posts/image/object-3.png)  

## JS实现面向对象的方式选择的是原型系统  
- JS通过原型的方式来描述对象  
- 使用原型和原型链来实现继承，很多同学说继承不合适，可以理解成委托  

## V8里的对象  
这里主要介绍一下隐藏类(map)  
还实现了隐藏类(map)描述了对象的属性布局  
隐藏类(map)它主要包括了属性名称和每个属性所对应的偏移量，拿到对象的起始位置加上偏移量，就得到属性的值在内存中的位置  
有了这个隐藏类，V8在解析的时候可以提高速度  
**两个对象的形状是相同的，V8 就会为其复用同一个隐藏类**  
两个对象的形状是相同的，要满足以下两点  
- 相同的属性名称  
- 相等的属性个数  

现在我们构造对象的时候基本使用class，所以名称和个数都是一样的，但是有些对对象的操作也会重新构建隐藏类，所以我们在开发的时候尽量避免  
我们用d8工具来看看哪些操作会重新构建隐藏类  
首先我们来创建一个类，并且用它来实例化一些对象  
``` javascript  
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

const person1 = new Person('person1', 20);
const person2 = new Person('person2', 22);
const person3 = new Person('person3', 23);
%DebugPrint(person1);
%DebugPrint(person2);
%DebugPrint(person3);
```  
然后我们通过d8观察一下对象，其中map隐藏类的地址都为一样的:  
![](https://raw.githubusercontent.com/jianghaoran116/land/gh-pages/_posts/image/object-4.png)  
- 修改属性  
  ``` javascript  
  class Person {
    constructor(name, age) {
      this.name = name;
      this.age = age;
    }
  }

  const person1 = new Person('person1', 20);
  const person2 = new Person('person2', 22);
  const person3 = new Person('person3', 23);
  %DebugPrint(person1);
  %DebugPrint(person2);
  %DebugPrint(person3);

  person1.age = 30;
  person2.age = 31;
  person3.age = 29;
  %DebugPrint(person1);
  %DebugPrint(person2);
  %DebugPrint(person3);
  ```  
  ![](https://raw.githubusercontent.com/jianghaoran116/land/gh-pages/_posts/image/object-5.png)  
  map隐藏类的地址都为一样的  
- 添加属性  
  ``` javascript  
  class Person {
    constructor(name, age) {
      this.name = name;
      this.age = age;
    }
  }

  const person1 = new Person('person1', 20);
  const person2 = new Person('person2', 22);
  const person3 = new Person('person3', 23);
  %DebugPrint(person1);
  %DebugPrint(person2);
  %DebugPrint(person3);

  person1.job = '';
  person2.car = '';
  person3.bag = '';
  %DebugPrint(person1);
  %DebugPrint(person2);
  %DebugPrint(person3);
  ```  
  ![](https://raw.githubusercontent.com/jianghaoran116/land/gh-pages/_posts/image/object-6.png)  
  发现添加新的属性都会重新构建隐藏类  
- 删除属性  
  ``` javascript  
  class Person {
    constructor(name, age) {
      this.name = name;
      this.age = age;
    }
  }

  const person1 = new Person('person1', 20);
  const person2 = new Person('person2', 22);
  const person3 = new Person('person3', 23);
  %DebugPrint(person1);
  %DebugPrint(person2);
  %DebugPrint(person3);

  delete person1.name;
  delete person2.age;
  %DebugPrint(person1);
  %DebugPrint(person2);
  %DebugPrint(person3);
  ```  
  ![](https://raw.githubusercontent.com/jianghaoran116/land/gh-pages/_posts/image/object-7.png)  
  删除属性也会重新构建隐藏类  



javascript面向对象的程序设计。
# 面向对象  
先了解下JS中的对象及历史，这样方便我们了解为何JS这么设计。然后再看看JS的对象特征。最后看看对象里的属性。
## JS中的对象及历史  
JS通过原型的方式来描述对象，然后为了模仿Java，在原型的基础上引入了new，this。
## JS对象的特征  
对象的一般特征:  
- 对象具有唯一标识性  
- 对象有状态  
- 对象具有行为  

JS的对象就是一堆属性和值的集合  

JS如何实现的
- 对象具有唯一标识性（通过内存地址来实现）  
  ``` javascript  
    var o1 = { a: 1 };
    var o2 = { a: 1 };
    console.log(o1 == o2);
  ```  
- 将状态和行为统一抽象为"属性"  
  ``` javascript  
    var o = {
      a: 1,
      b: function() {
        console.log(this.a);
      },
    };
  ```  
**对象具有高度的动态性**  

## JS对象的两类属性  
### 第一类属性: 数据属性。  
- value: 属性的值, 默认值为undefined  
- writable: 属性能否被赋值  
- enumerable: 能否枚举  
- configurable: 属性能否被删除或者改变特征值  

### 第二类属性: 访问器（getter/setter）属性。  
- getter: 函数或 undefined，在取属性值时被调用  
- setter: 函数或 undefined，在设置属性值时被调用  
- enumerable: 能否枚举  
- configurable: 属性能否被删除或者改变特征值  

使用getOwnPropertyDescripter()来查看, 使用Object.defineProperty来定义属性, Object.defineProperties()定义多个属性  

*"基于原型"的编程看起来更为提倡程序员去**关注一系列对象实例的行为**，然后才去关心如何将这些对象，**划分到最近的使用方式相似的原型对象**，而不是将它们分成类。*

## 对象的属性值有三种类型  
- 原始类型  
  null、undefined、boolean、number、string、bigint、symbol  
- 对象类型  
- 函数类型  

V8 内部是怎么实现函数可调用特性的呢  
有两个隐藏属性
- name  
  函数的名称，匿名函数为anonymous  
- code  
  函数代码  
函数到底关联了哪些内容
- 函数作为一个对象，它有自己的属性和值，所以函数关联了基础的属性和值  
- 函数之所以成为特殊的对象，这个特殊的地方是函数可以“被调用”，所以一个函数被调用时，它还需要关联相关的执行上下文  

然而在 V8 实现对象存储时，并没有完全采用字典的存储方式，这主要是出于性能的考量。因为字典是非线性的数据结构，查询效率会低于线性的数据结构，V8 为了提升存储和查找效率，采用了一套复杂的存储策略  
- 常规属性 (properties)  
- 排序属性 (element)  

隐藏类(map)描述了对象的属性布局  
  它主要包括了属性名称和每个属性所对应的偏移量  
  V8 会查询 point 的 map 中 x 属性相对 point 对象的偏移量，然后将 point 对象的起始位置加上偏移量，就得到了 x 属性的值在内存中的位置，有了这个位置也就拿到了 x 的值，这样我们就省去了一个比较复杂的查找过程。

``` javascript  
function Foo() {
  this[1] = 1;
};
let bar = new Foo();
let bar1 = new Foo();
%DebugPrint(bar);
%DebugPrint(bar1);
```  

``` javascript  
function Foo() {
  this[1] = 1;
};
let bar = new Foo();
let bar1 = new Foo();
%DebugPrint(bar);
%DebugPrint(bar1);
bar[2] = 2;
bar1[2] = 2;
%DebugPrint(bar);
%DebugPrint(bar1);
```  

``` javascript  
function CreatePerson(name) {
  this.name = name;
};

CreatePerson.prototype.sayname = function() {
  console.log(this.name);
};

var person = new CreatePerson('kevin');

%DebugPrint(person);
person.age = 20;
%DebugPrint(person);
```  

``` javascript  
class Person {
  constructor(name) {
    this.name = name;
  }
}

const person = new Person('joge');
%DebugPrint(person);
person.name = 'test';
%DebugPrint(person);
person.age = 20;
%DebugPrint(person);
```  

# 原型和原型链  
可以理解成JS是如何实现面向对象的。  

原型系统  
- 如果所有的对象都有私有字段[[prototype]](对象的原型)  
- 读一个属性，如果对象本身没有，则会继续访问对象的原型，直到原型为空或者找到为止。  

没有 Object.create、Object.setPrototypeOf 的早期版本中，**new运算是唯一一个可以指定[[prototype]]的方法**。  
new运算实际上做了几件事:  
- 以构造器的prototype属性为原型，创建新对象  
- 将this和参数传给构造器，执行  
- 如果构造器返回的是对象，则返回，否则返回第一步的对象  

#### prototype  
每个函数都有一个prototype属性。*prototype是函数才会有的属性*  
这个函数的 prototype 属性到底指向的是什么呢?  
``` javascript  
  function Person() {}
  var person = new Person();
  console.log(person.__proto__ === Person.prototype);
```  
#### __proto__  
每一个JavaScript对象(除了null)都具有的一个属性, 叫__proto__, 这个属性会指向该对象的原型。  

原型的原型又是什么呢？  
Object.prototype  

#### constructor  
原型对象指向构造函数  

了解了函数的prototype, 对象的__proto__, 原型对象的constructor。再来看看静态方法。  

#### 静态方法  
静态方法和实例方法最主要的区别就是实例方法可以访问到实例，可以对实例进行操作，而静态方法一般用于跟实例无关的操作。  

# 对象  

## JS中的对象分类  
- 宿主对象 由JS宿主坏境提供的对象  
- 内置对象 由JS语言提供的对象  
  - 固有对象 由标准规定 随着JS运行时创建而自动创建的对象实例  
  - 原生对象 可以由用户通过Array, RegRxp等内置构造器或特殊语法创建的对象  
  - 普通对象 由{}语法 Object构造器 或者class关键字定义的对象  

## 创建对象的方法  
- 工厂模式  
  ``` javascript  
    function createPerson(name) {
      var o = new Object();
      o.name = name;
      o.sayName = function() {};
      return o;
    }
    var person = createPerson('kevin');
  ```  
  缺点是对象无法识别  
- 构造函数模式  
  ``` javascript  
    function CreatePerson(name) {
      this.name = name;
      this.sayName = function() {
        console.log(this.name);
      };
    }
    var person = new CreatePerson('kevin');
  ```  
  缺点是每个方法都会被重新创建一次  
- 原型模式  
  ``` javascript  
    function CreatePerson() {};

    CreatePerson.prototype.name = 'kevin';
    CreatePerson.prototype.sayName = function() {
      console.log(this.name);
    };

    var person = new CreatePerson();
  ```  
  缺点是一些引用类型会共享同一个对象  
- 组合使用构造函数和原型模式  
  ``` javascript  
    function CreatePerson(name) {
      this.name = name;
    };

    CreatePerson.prototype.sayname = function() {
      console.log(this.name);
    };

    var person = new CreatePerson('kevin');
  ```  

# 继承  
核心都是让子类拥有父类的方法和属性  
- 原型链  
  ``` javascript  
    function Parent() {
      this.pValue = "parent";
    };

    Parent.prototype.getPValue = function() {
      return this.pValue;
    };

    function Child() {
      this.cValue = "child";
    };

    Child.prototype = new Parent();

    Child.prototype.getCValue = function() {
      return this.cValue;
    };

    var instance = new Child();
    console.log(instance.getCValue());
    console.log(instance.getPValue());
  ```  
  问题: 创建的时候不能向parent传参、共享引用类型的数据  
- 借用构造函数  
  ``` javascript  
    function Parent() {
      this.pValue = "parent";
      this.color = ["red"];
    };

    function Child() {
      Parent.call(this);
    }

    var instance1 = new Child();
    var instance2 = new Child();

    instance1.color.push("blue");
    console.log(instance1.color);
    console.log(instance2.color);
  ```  
- 组合继承  
  ``` javascript  
    function Parent(name) {
      this.name = name;
      this.color = ["red"];
    };

    Parent.prototype.getName = function() {
      return this.name;
    };

    function Child(name, age) {
      Parent.call(this, name);
      this.age = age;
    }

    Child.prototype = new Parent();

    Child.prototype.sayAge = function() {
      return this.age;
    };

  ```  
bable编译es6的class的部分代码:  
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

