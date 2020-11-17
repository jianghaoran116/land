---
layout: post  
title: "JS中的遍历"  
date: 2020-11-17  
categories: javascript  
tags: js  
---

# JS中的遍历  
## 对象  
``` javascript  
let obj = {
  a: 1,
  b: 123,
  list: [],
  data: {},
  [Symbol('foo')]: 'foo',
}

for (let key in obj) {
  console.log('key:::', key, ' value:::', obj[key]);
}

const myKeys = Object.keys(obj);
const myValues = Object.values(obj);

console.log(myKeys);
console.log(myValues);

const objName = Object.getOwnPropertyNames(obj);
console.log('objName:::', objName);

const objSym = Object.getOwnPropertySymbols(obj);
console.log('objSym:::', objSym);

const ownKeys = Reflect.ownKeys(obj);
console.log('ownKeys:::', ownKeys);
```  
- for…in是为遍历对象设计的  
  for…in代码块中不能有return, 不然会抛出异常  
- Object.values()  
- Object.keys()  
- Object.getOwnPrototypeNames()  
  遍历的属性不包含symbol  
- Object.getOwnPrototypeSymbols()  
  遍历的属性只包含symbol  
- Refect.ownKeys()  
  遍历的属性包含symbl  

## 数组  

- 会改变原数组的方法  
  push、pop、shift、unshift  
  splice  
  short、reverse  
  copyWhitin  
遍历  
- for of  
- forEach、map、filter、every、some、reduce  
- entries、find、findIndex、keys、values、fill、from、of、copyWithin  

``` javascript  
const arr = [1, 3, 4, 5, 6, 9];

for (let [key, value] of arr.entries()) {
  console.log(key, value);
}

for (let value of arr.values()) {
  console.log('value:::', value);
}
```  

## Map Set  

- keys  
- values  
- entries  

## 迭代器  

- [Symbol.iterator]
  是一个函数 返回一个无参数的next函数 里面包含一个对象 有done属性和value done表示是否结束 value表示当前遍历的值  
``` javascript  
let authors = {
  allAuthors: {
    fiction: [
      'Agatha Christie',
      'J. K. Rowling',
      'Dr. Seuss'
    ],
    scienceFiction: [
      'Neal Stephenson',
      'Arthur Clarke',
      'Isaac Asimov',
      'Robert Heinlein'
    ],
    fantasy: [
      'J. R. R. Tolkien',
      'J. K. Rowling',
      'Terry Pratchett'
    ]
  }
}

authors[Symbol.iterator] = function() {
  let allAuthors = this.allAuthors
  let keys = Reflect.ownKeys(allAuthors)
  let values = []

  return {
    next() {
      if (!values.length) {
        if (keys.length) {
          values = allAuthors[keys[0]]
          keys.shift()
        }
      }
      return {
        done: !values.length,
        value: values.shift()
      }
    }
  }
}

for (let value of authors) {
  console.log(value)
}

```  

## 异步迭代器  

- [Symbol.asyncIterator]  