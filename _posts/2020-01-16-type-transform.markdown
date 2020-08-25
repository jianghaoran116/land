---
layout: post
title: "类型转换"
date: 2020-01-16  
categories: js  
tags: js基础  
---  
## toString() JSON.stringify()  
## toNumber  
## toBoolean  

## 数字和字符串的隐式类型转换
## 布尔值到数字的隐式类型转换
## 隐式强制类型转换到布尔值

## == 比较  
- 字符和数字 x == y  
如果 x 是数字  x == ToNumber(y)  
如果 y 是数字  ToNumber(x) == y
- 其他类型和布尔值  
会把布尔值ToNumber true -> 1, false -> 0  
- 对象和非对象之间  
会把对象ToPrimitive  

0 == null  
'0' == false  
[] == true  
0 == false  
false == ''  
[] == ![] --> [] == false  
'' == [null] --> '' == ''  