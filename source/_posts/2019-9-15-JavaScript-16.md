---
title: JavaScript 筆記 - 函式預設值
date: 2019-09-15 23:43:16
tags: 
  - JavaScript
categories: 前端
---

當函式預設值為 0 時可以使用 三元運算子去判斷

``` JavaScript
function foo(val = 500) {
    if (val) {
        console.log('val', val)
    } else {
        console.log('請輸入數值')
    }
}
foo(10);
foo(0);
foo();
```