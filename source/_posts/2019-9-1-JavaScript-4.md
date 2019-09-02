---
title: JavaScript 筆記 - 提升
date:  2019-09-01 11:28:48
tags: 
  - JavaScript
categories: 前端
---

在執行以下程式時, 我們很清楚會印出 1

``` JavaScript
var a = 1;
console.log(a); // 1
```

但是在執行這個程式時, 卻會印出 undefined, 這個是因為 JavaScript 的提升特性 (hoisting), 在 JavaScript 運行程式時會氛圍兩個階段, 第一個階段稱為創造環境, 第二個階段為執行, 在創造階段程式會先將程式中的變數建立起來並為他們賦予一個值, 這個值為 `undefined`, 

``` JavaScript
console.log(a); // undefined
var a = 1;
```

而這一段會等價為, 

``` JavaScript
var a; // 創造階段
console.log(a);
a = 1; // 執行
```

### function 在創造階段會先被優先載入
以下這個範例來看, 在 foo 這個 function 宣告之前我們就去呼叫他的話, 那會印出 `hello world`, 
因為 function 在創建階段會優先被載入
``` JavaScript
foo(); // hello world
console.log(a); // undefined
function foo() {
    console.log('Hello world');
}
var a = 1;
```

但是如果是以下的程式碼執行時會發生錯誤, 原因是因為在創建階段, 函式表達式並不會預先被創建好, 該變數只會先被提升並被賦予 undefined , 所以呼叫時會發生錯誤

``` JavaScript
foo(); // Uncaught TypeError: foo is not a function
var foo = function() {
    console.log('hello world');
}
```

## 陷阱題

以這一段程式來說, 會印出 1 的原因是 funciton 的創建優先序會比變數高

``` JavaScript
var greet = function() {
    console.log('1');
}
function greet() {
    console.log('2')
}
greet(); // 1
```

上述程式可以修改成，我們可以發現 function 會先被提升到最上面, 但變數一開始

``` JavaScript
function greet() {
    console.log('2');
}

var greet; // undefined
greet = function() {
    console.log('1');
}
greet(); // 1
```
