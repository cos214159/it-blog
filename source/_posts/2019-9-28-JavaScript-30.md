---
title: JavaScript 筆記 - ES6 (let、const)
date: 2019-09-28 10:46:46
tags: 
  - JavaScript
categories: 前端
---

ES6 有提供許多新的語法，而這些語法有一些是為了解決 ES5 上的一些問題

而 let 與 const 是為了解決 ES5 var 宣告上的一些問題，以下來一一列舉

## 變數可以重複宣告

以下範例如果用 var 宣告的話，後面的變數會覆蓋掉前面的
``` JavaScript
var name = 'Alice';
var name = 'Bob';
```
### 解決方法 - 使用 let 或 const
let 以及 const 的差別是，let 可以針對宣告出來的變數重新賦值，但是 const 就不行

``` JavaScript
let name = 'Alice';
let name = 'Bob'; // SyntaxError: redeclaration of let name
```

``` JavaScript
const name = 10;
name = 20; // TypeError: invalid assignment to const `name'
```

## 作用域是 function block
因為 var 宣告的變數作用域是 function block 而且宣告出來的變數會被掛載到全域物件的屬性下，所以在迴圈執行完後，可以使用 window.i 查找到 i 
``` JavaScript
for (var i = 0; i < 10; i++) {
  console.log(i);
}

window.i // 10

// 在 block 內宣告的變數會被掛載到全域物件下
if (true) {
  var a = 10;
}

console.log(a); // 10
```

### 解決方法 1 - 利用立即函示將變數限制在 function 作用域下
``` JavaScript
(function(){
  for (var i = 0; i < 10; i++) {
    console.log(i);
  }
}());
window.i // undefined

```

### 解決方法 2 - 利用 let const 將變數作用域限制在 block scope 下
``` JavaScript
for (let i = 0; i < 10; i++) {
  console.log(i);
}
console.log(i); // i is not defined

if (true) {
  let a = 10;
}

console.log(a); // not defined
```

## 經典案例

這個範例是一個很經典的題目，因為 setTimeout 是一個非同步的 function，所以並不會立即執行，他會先被放到 event queue 裡面，等到主執行序執行完之後，會有一個 event loop 去看這個 event queue 將事件取出執行，但因為最後執行時，i 已經被累加到 10，所以會印出 10 次 10
``` JavaScript
for (var i = 0; i < 10; i++) {
  setTimeout(function() {
    console.log(i);
  }, 10);
}
```

要解決這個問題，可以使用 let 的特性，將變數的作用域限制在 block scope，這樣就不會因為外部迴圈持續累加而改變 block scope 內部的值

## 暫時性死區 (TDZ)
使用 let、const 跟 var 一樣會有 hoisting 的特性，也就是分為創造以及執行階段，但是用 let 和 const 宣告的變數在創造階段到實際執行會有一段暫時性死區，在這個區間是沒有辦法調用變數

``` JavaScript
console.log(a); // ReferenceError: can't access lexical declaration `a' before initialization
let a = 10;
```

