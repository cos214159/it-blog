---
title: JavaScript 筆記 - not defined 與 undefined 與 null
date:  2019-09-01 13:59:22
tags: 
  - JavaScript
categories: 前端
---

在 JavaScript 提供的型態 undefined 與 null 是基本型別的一種, 而 not defined 的意思是當使用一個不存在的變數時會有這個錯誤

比方說 , 我們在沒有宣告 a 的情況之下印出 a 的話會出現 not defined 的錯誤

``` JavaScript
console.log(a); // Uncaught ReferenceError: a is not defined
```

而 undefined 是 JavaScript 在運行時自動賦予給變數的值, 若是在程式執行時是最好不要手動賦予 undefined 給變數, 不然會搞不清楚到底這個值是系統賦予的還是自己給的, 若是要賦予一個空值的話, 可以使用 null

``` JavaScript
var a = undefined; // not good
var b = null; // good 
```