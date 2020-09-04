---
layout: post  
title: "数字反转"  
date: 2020-09-03  
categories: leet code  
tags: leet code  
---  

# 数字反转
第一反应是通过转换成字符串然后进行反转, 了解后数字用模和除法来模拟数组的弹出和插入, 两种方式都需要考虑边界问题  

- 字符串方式  
  需要判断一下正负数 然后转换成字符串 最后专成数字  
  ``` javascript  
    let res;
    let subSingle;
    let min = - Math.pow(2, 31);
    let max = Math.pow(2, 31) - 1;
    // 设置正负号
    if (x > 0) {
      res = x + '';
    } else {
      subSingle = true;
      res = Math.abs(x) + '';
    }
    // 字符串反转
    res = res.split('').reverse().join('');
    // 转数字
    res = res - 0;

    if (subSingle) {
      res = -res;
    }

    if (res < min || res > max) {
      res = 0;
    }

    return res;
  ```  

- 取模和除法  
  取模可以拿到最后一个数字 除法可以去掉最后一个数字  
  ``` javascript  
    let res = 0;
    let temp = 0;
    let min = - Math.pow(2, 31);
    let max = Math.pow(2, 31) - 1;

    while (x !== 0) {
      let pop = x % 10; // 拿到最后一个数
      x = parseInt(x / 10); // 去掉最后一个数
  
      temp = res * 10 + pop; // 生成一个新的数字

      if (temp < min || temp > max) {
        temp = 0;
        x = 0;
      }
      res = temp;
    }

    return res;
  ```  