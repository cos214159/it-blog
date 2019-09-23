---
title: JavaScript 筆記 - this
date: 2019-09-22 13:49:40
tags: 
  - JavaScript
categories: 前端
---

JavaScript 的 this 跟函式如何宣告無關，在於函式是如何被呼叫

this 基本觀念
* 每個執行環境都有 this 這個關鍵字
* this 與函式如何宣告並沒有關聯，與呼叫方法有關
* 嚴格模式下，簡易呼叫會有很大的改變

影響函式 this 的調用方式
* 作為物件方法，物件的方法調用時，僅需要關注是在哪一個物件下被呼叫
* 簡易呼叫
* bind、apply、call 方法 (可以讓函式綁定特定的 this)
* new
* DOM 事件處理  
* 箭頭函式 (ES6)

## 簡易呼叫
基本上在呼叫函式的時候，不是透過物件呼叫的話，大部分 this 都會指向全域，但是盡可能不要使用簡易呼叫的 this

要注意的是簡易呼叫並非是使用 window 去呼叫 function 而是在呼叫 function 時前面沒有掛上物件的話，那 this 會指向全域

``` JavaScript
var global = '全域';

function foo() {
  console.log(this.global);
}

foo();
```

## 物件下調用

``` JavaScript
var obj = {
  name: 'Alice',
  callName: function() {
    console.log(this.name);
  }
}

obj.callName();
```

## call apply bind
call、apply、bind 使用方法都很接近，只是 bind 在呼叫之後不會立即執行而是會回傳一個 funciton 的參考

來看看以下範例

### call

call 的第一個參數是帶入 this 要指向的物件，之後是帶入 function 接收的參數
``` JavaScript
var family = {
  mom: 'mom',
  me: 'Bob'
}

function foo(number1, number2) {
  console.log(this.mom, this.me, number1, number2);
}

foo.call(family, 1, 2);
```

### apply
apply 同樣的第一個參數也是帶入 this 要指向的物件，但之後的參數是用陣列的形式帶入
``` JavaScript
var family = {
  mom: 'mom',
  me: 'Bob'
}

function foo(number1, number2) {
  console.log(this.mom, this.me, number1, number2);
}

foo.apply(family, [1, 2]);
```

### bind
bind 將參數傳入的方式跟 call 是一模一樣的，只是執行過後不會立刻執行而是會回傳一個參考

``` JavaScript
var family = {
  mom: 'mom',
  me: 'Bob'
}

function foo(number1, number2) {
  console.log(this.mom, this.me, number1, number2);
}

var fn = foo.bind(family, 1, 2);
fn();
```

而 bind 可以不用一次帶入全部的參數，例如

``` JavaScript
var family = {
  mom: 'mom',
  me: 'Bob'
}

function foo(number1, number2) {
  console.log(this.mom, this.me, number1, number2);
}

var fn = foo.bind(family, 1);
fn(2);
```

## call、apply、bind 傳入的 this 參考不一定要是一個物件
在前面的例子，我們在呼叫 function 時帶入的參數都是物件，其實也可以帶入純值或 undefined，在非嚴格模式下 null 或 undefined 會被轉成全域變數，而純值將會被封裝起來

如何帶入的是純值，則 this 會指向該純值的物件型別
``` JavaScript
function foo(number1, number2) {
  console.log(this, number1, number2);
}

foo.call(10, 1, 2); // Number { 10 } 1 2
```

另外一個特別的例子，是可以傳入 undefined
``` JavaScript
function foo(number1, number2) {
  console.log(this, number1, number2);
}

foo.call(undefined, 1, 2); // Window { 10 } 1 2
```

## DOM 事件處理  
在綁定 DOM 元素的監聽事件時，this 是會自動綁定到 DOM 元素上，比如說

``` JavaScript
document.querySelector('p').addEventListener('click', function() {
  console.log(this); // <p>
})
```