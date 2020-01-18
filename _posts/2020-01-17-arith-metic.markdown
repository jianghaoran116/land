---
layout: post
title:  "算法"
date:   2020-01-17
categories: js
tags: js
---  
## 0-1背包的回溯解决办法  
我们假设背包的最大承载重量是 9。我们有 5 个不同的物品，每个物品的重量分别是 2，2，4，6，3。在不超过背包所能装载重量的前提下，如何让背包中物品的总重量最大？  
![](./image/a-m-1.jpg)  
``` javascript  
  let maxW = -Infinity;
  /**
   * 假设背包可承受重量100，物品个数10，物品重量存储在数组a中，那可以这样调用函数：
   * f(0, 0, a, 10, 100)
   * @param {number} i - 表示考察到哪个物品了
   * @param {number} cw - 表示当前已经装进去的物品的重量和
   * @param {array} arr - 表示每个物品的重量
   * @param {number} n - 表示物品个数
   * @param {number} w - 背包重量
   */
  function f(i, cw, arr, n, w) {
    if (cw === w || i === n) {
      if ( cw > maxW ) {
        maxW = cw;
      }
      return
    };
     
    f(i+1, cw, arr, n, w);
    if (cw + arr[i] <= w) {
      f(i+1, cw + arr[i], arr, n, w);
    }
  }

  f(0, 0, [2, 2, 4, 6, 3], 5, 9)
```  
