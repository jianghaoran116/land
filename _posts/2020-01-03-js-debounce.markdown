---
layout: post
title: "防截"
date: 2020-01-03
categories: js  
tags: js  
---

# 防抖
在事件触发N秒后才执行，如果在N秒内又触发了这个事件，就以新的时间为准，N秒后才执行。

``` javascript  
function debounce(fun, time) {
  let timeout;

  return function(...args) {
    clearTimeout(timeout)
    timeout = setTimeout(fun.bind(context, ...args), time)
  }
}
```  

# 截流  
如果你持续触发事件，每隔一段时间，只执行一次事件。  
时间戳方式:  
``` javascript  
function throttle(fnc, time) {
  let previous = 0;

  return function(...args) {
    let now = +new Date();
    const context = this;
    if (now - previous > time) {
      previous = now;
      fnc.apply(context, args);
    }
  };
}
```  

计时器方式:  
``` javascript  
function throttle(fnc, time) {
  let timeout = null,
      previous = 0;

  return function(...args) {
    let context = this;
    
    if (!timeout) {
      timeout = setTimeout(function() {
        timeout = null;
        fnc.apply(context, args)
      }, time)
    }
  };
}
```
