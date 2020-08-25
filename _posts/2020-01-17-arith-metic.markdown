---
layout: post
title:  "动态规划"
date:   2020-01-17
categories: 算法  
tags: 算法  
---  
## 0-1背包的回溯解决办法  
### 递归  
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
### 递归备忘录优化  
递归的过程中有些状态已经递归过了,记录一下不再递归, 使用一个二维数组记录一下状态, 通过这个状态判断  
``` javascript
let maxW = -Infinity;
let d2Arr = Array.from({length: 9}, () => []);

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
  }
  
  if (d2Arr[i][cw]) {
    return
  }
  d2Arr[i][cw] = true;
  
  f(i+1, cw, arr, n, w);
  if (cw + arr[i] <= w) {
    f(i+1, cw + arr[i], arr, n, w);
  }
}

f(0, 0, [2, 4, 5, 6, 1], 5, 9)
```  

### 动态规划  
![](./image/a-m-2.jpg)  
``` javascript  
let states = Array.from({length: 5}, () => []);
let weight = [2, 2, 4, 6, 3];
let w = 9;
states[0][0] = true;
/**
 * 动态规划0-1背包问题
 * @param {number} cw - 物品的重量 
 * @param {number} n - 物品的个数
 * @param {number} w - 背包的容量
 */
function knapsack(weight, n, w) {
  if (weight[0] <= w) {
    states[0][weight[0]] = true;
  }
  
  for (let i = 1; i < n; i += 1) { // 分成n个阶段  
    // 不把第i个物品放入背包
    for (let j = i; j <= w; j += 1) {
      if (states[i-1][j] === true) {
        states[i][j] = states[i-1][j];
      }
    }
    // 把第i个物品放入背包
    for (let j = i; j <= w - weight[i]; j += 1) {
      if (states[i-1][j] === true) {
        states[i][j+weight[i]] = true;
      }
    }
  }

  for (let i = w; i >= 0; i -= 1 ) {
    if (states[n-1][i] == true) return i;
  }

  return 0
}

console.log(knapsack(weight, weight.length, w));
```  