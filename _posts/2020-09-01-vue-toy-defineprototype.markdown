---
layout: post  
title: "通过defineprototype来模拟vue的双向绑定"  
date: 2020-08-26  
categories: vue  
tags: vue  
---  

# 双向绑定之definePrototype  
大体思路是: 通过get的时候收集依赖，set的时候执行依赖，关键在如何处理依赖  

## 第一步依旧是如何把VUE里的对象渲染到DOM上  
- createElement创建DOM节点  
  ``` javascript  
    _createElement(targetName, data, children) {
      // 创建属性 创建文本节点 通过children来创建子节点
      // ...
    }
  ```  
- data里的函数，通过代理到this上实现this.xxx来访问data里的数据  
  ``` javascript  
    function proxy(target, data, key) {
      Reflect.definePrototype(target, key, {
        get() {
          return data[key];
        }
        set(val) {
          data[key] = val;
        }
      })
    }
    // 使用的时候
    for (let key in this._data) { // this.data 代理到 this上
      proxy(this, this._data, key);
    }
  ```  
- update或render来触发渲染成真的DOM  
  ``` javascript  
    // 传入新节点来替换旧的节点
    replaceChild: function(oldElement, element) {
      return oldElement.parentElement.replaceChild(element, oldElement);
    }
  ```  

## 第二步通过get和set来触发渲染  
- 可以通过把update传到代理对象里 然后set的时候来渲染 没有依赖判断 更新就会全部渲染  
  ``` javascript  
    // render先用传进来的方式然后set的时候刷新
    function defineReactive(target, key, value, render) {
      Reflect.definePrototype(target, key, {
        get() {
          return value;
        },
        set(newVal) {
          value = newVal;
          render(); // 这里触发渲染
        },
      })
    }
  ```  

- 然后通过遍历属性的方式把所有的属性都代理一遍
  ``` javascript  
    // 初始化的时候遍历一下属性 进行代理 并把render函数传进去
    function walk(obj, render) {
      Reflect.ownKeys(obj).forEach((key => {
        defineReactive(obj, key, obj[key], render);
      }));
    }
  ```  
这样所有的属性有修改的时候都会触发渲染  

## 依赖收集 按依赖来触发渲染  
- 监听类 把walk函数封装成一个简单的Observal  
  ``` javascript  
    class Observal {
      constructor(obj) {
        this.walk(obj);
      }

      walk(obj) {
        // ...
      }
    }
  ```  
- Watcher  
  ``` javascript  
    class Watcher {

    }
  ```  
- 依赖类  
  ``` javascript  
    class Dep {
      addDepend() {
        Dep.targets[Dep.targets.length - 1].addDep(this);
      }
    }

    Dep.targets = []; // 静态数据
  ```  
- get的过程中注入依赖  
  ``` javascript  
    function defineReactive(target, key, value) {
      const dep = new Dep();

      // ...
      get() {
        if (Dep.targets.length > 0) {
          dep.addDepend();
        }
      }
      // ...
    }
  ```  