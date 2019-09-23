---
title: JavaScript 筆記 - 閉包 (closure)
date: 2019-09-22 11:33:35
tags: 
  - JavaScript
categories: 前端
---

閉包的概念就是我們可以在子涵式裡面使用父函式的變數，這個也是基於範圍鍊的特性，如果說目前的 block 內沒有變數便會依據語法作用域向上尋找，而外層的變數因為會一直被參照到，所以也沒有辦法被釋放掉，以下來看一個閉包的範例

``` JavaScript
function foo() {
  var result = 1000;
  return function(number) {
    result += number;
    return result;
  }
}

foo()(100); // 1100

var fn = foo();
fn(10);
fn(20);
fn(30);
```

這種作法有點像設計樣式中的工廠模式，透過給予不一樣的參數，可以得到一個全新的物件，而這個物件跟其他的物件是分離的不會有任何關連，這個又稱為函式工廠，除此之外還可以在閉包裡面稱加私有方法

``` JavaScript
function foo() {
  var result = 1000;
  return {
    add: function(number) {
      return result += number
    },
    minus: function(number) {
      return result -= number;
    }
  }
}
var result = foo();
result.add(30);
```

## 經典題目
以下是一個經典題目

``` JavaScript
function foo() {
  var arr = [];
  for (var i = 0; i < 3; i++) {
    arr.push(function() {
      console.log(i);
    })
  }
  return arr;
}

var result = foo();
result[0](); // 3
result[1](); // 3
result[2](); // 3
```

這個例子的重點在於因為 迴圈的 i 是用 var 去宣告，所以範圍是在 function 裡面，這時候在執行陣列內的 function 時，會印出執行完的 i，如果不想要這種結果的話，可以採用立即函式限制變數的作用域

``` JavaScript
function foo() {
  var arr = [];
  for (var i = 0; i < 3; i++) {
    (function(j){
      arr.push(function() {
        console.log(j);
      })
    })(i)
  }
  return arr;
}

var result = foo();
result[0](); // 0
result[1](); // 1
result[2](); // 2
```

那另外的方式是透過 ES6 的 let 宣告變數的方式，利用 let 宣告的變數作用域是在 block scope，因此印出時就可以正確依序印出 0 1 2

``` JavaScript
function foo() {
  var arr = [];
  for (let i = 0; i < 3; i++) {
    arr.push(function() {
      console.log(i);
    })
  }
  return arr;
}

var result = foo();
result[0](); // 0
result[1](); // 1
result[2](); // 2
```