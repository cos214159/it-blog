---
title: JavaScript 筆記 - 邏輯運算子
date: 2019-09-15 23:06:14
tags: 
  - JavaScript
categories: 前端
---

邏輯運算子有分三個 `&&`、`||`、`!` 各別來看一下

## && 
這個運算子指的是假如第一個表達式為真時會直接回傳第二個表達式, 否則會回傳第一個

``` JavaScript
console.log(undefined && 5); // undefined
console.log(0 && 5); // 0
console.log(5 && undefined); // undefined
console.log(1 && 10); // 10
```

## ||
``` JavaScript
console.log(1 || 0)); // 1
console.log(0 || 10); // 10
console.log(undefined || 1); // 1
```

## !
``` JavaScript
console.log(!0); // true
console.log(!1); // false
console.log(![]); // false
```

