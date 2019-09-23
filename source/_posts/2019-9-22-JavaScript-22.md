---
title: JavaScript 筆記 - function
date: 2019-09-22 09:47:27
tags: 
  - JavaScript
categories: 前端
---

在 JavaScript 除了純值之外，其餘都是物件那 function 當然就是一個物件，只是 function 跟一般物件會有一些不同。

* function 多了一個被呼叫的能力 (be invoked)
* 可以傳入參數
* 名稱 (optional)
* 帶有一個 this
* 可以回傳值

在前面有提到 function 可以分為函式陳述式以及函式表達式

## 函式陳述式
函式陳述式需要宣告名稱，這種又稱為具名函式
``` JavaScript
function foo() {
  console.log('function statement');
}
foo();
```

## 函式表達式
函式表達式可以不用宣告 function 名稱，沒有名稱的函式又被稱為匿名函式
``` JavaScript
var foo = function() {
  console.log('function expression')
}
foo();
```

但並不是所有的函式表達式都是匿名函式，例如以下範例

這個範例是將函式陳述式賦予給左側的變數，最後印出的結果，functionA functionB 的名稱都會是 function B，而要注意一點的是，具名函式 functionB 可以在函式內被調用，但是函式外就不行，即使他是一個具名函式
``` JavaScript
var functionA = function functionB() {
  console.log(functionA, functionB) // function functionB() function functionB()
}
functionA();
functionB(); // ReferenceError: functionB is not defined
```

## callback function

``` JavaScript
function sayHi(fn) {
  fn();
}

sayHi(function() {
  console.log('Hello !!');
})

function greet() {
  console.log('Hello~')
}

sayHi(greet);
```

JavaScript 的 function 有著一等公民的特性，他可以被視為物件當作參數傳遞，所以在以上範例，他可以被當作參數傳入 function 被調用

## 函式參數 
``` JavaScript
var obj = {
  foo: function(params) {
    console.log(params, arguments, this);
  }
}

obj.foo('a', 1, 2);
```

以上針對參數有幾個特性，假如說 function 只接收一個參數，那他只會接收傳入 function 的第一個參數，其餘都會被 argument 接收，而這個 argument 為一個類陣列

### 傳入物件參數
因為物件傳遞時是傳參考，所以如果在 function 內部改變物件的值，就會同步修改到原有的物件
``` JavaScript
function foo(obj) {
  obj.name = 'Bob'
}

var obj = {
  name: 'Alice'
}

foo(obj);

console.log(obj); //  { name: "Bob" }
```

### arguments
arguments 是可以用來接收所有傳入的參數，並且他是一個類陣列
``` JavaScript
function foo() {
  console.log(arguments);
}

foo(1, 2, 3); // Arguments { 0: 1, 1: 2, 2: 3, … }
```

### 特殊案例
``` JavaScript
function foo(v) {
  console.log(v);
  var v;
  console.log(v);
}

foo(10);
```

這個範例可以想成是，所以在 function 內部重複宣告變數，並不會改變傳入的參數
``` JavaScript
function foo(v) {
  var v
  var v;
  v = 10
  console.log(v);
}

foo(10);
```

但是如果宣告的是同名 function 就會被覆蓋掉
``` JavaScript
function foo(v) {
  function v() {

  }
  console.log(v); // function v()
}

foo(10);
```


## 立即函式 (IIFE)
具名函式如果沒有名稱的話是沒有辦法被執行的，而有一種方法是在函式被宣告出來之後可以立即被執行，這個叫立即函式 (Immediately Invoked Functions Expressions)

而立即函式無法在函式外被呼叫，立即函式可以不用加上 function name，最後呼叫的 `()` 可以擺在最後，或大括號後都沒問題。

``` JavaScript
(function IIFE(){
  console.log('立即函式')
}());

(function(){
  console.log('立即函式')
})();
```



### 立即函式可以限制作用域
先前有講到用 var 宣告的變數作用域式 function scope，而立即函式很常宣告是用來限制變數的作用域，讓他不會掛載到全域下

``` JavaScript
(function(){
  var a = 'Alice';
})();

console.log(a); // not defined
```

### 傳遞參數給立即函式
``` JavaScript
var globalVariable = 'global';
(function(params){
  console.log(params)
})(globalVariable);
```

### 立即函式回傳值
因為立即函式是一個表達式，可以回傳值
``` JavaScript
var greet = 'Hello';
var result = (function(greet){
  return '你好' + greet;
})(greet);
console.log(result);
```