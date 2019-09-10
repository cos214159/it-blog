---
title: JavaScript 筆記 - JavaScript 型別
date: 2019-09-10 07:34:55
tags: 
  - JavaScript
categories: 前端
---

JavaScript 的型別分為**原始型別**以及**物件型別**

原始型別總共有 7 個
* Boolean 布林
* Null 空
* Undefined 未定義
* Number 數值
* String 字串
* BigInt 整數數值
* Symbol 

在每一個型別會有一些各自的方法，這些方法是被定義在包裹物件裡面，比方說
* number -> new Number()
* Boolean -> new Boolean()
* String -> new String()

那我們來看一下原始型別的 demo，要注意一下 `typeof null` 是一個 object 型態，這個是 JavaScript 以前就有的錯誤，而因為很多網站都是用這個錯誤來完成應用，如果修正的話會導致很多網站出問題。 
``` JavaScript
var a, b, c, d, e, f;
a = 10; // number
b = 'Bob'; // string
c = false; // boolean
d = {}; // object
e = null; // object
console.log(typeof a);
console.log(typeof b);
console.log(typeof c);
console.log(typeof d);
console.log(typeof e);
console.log(typeof f); // undefined
```

### not defined
如果我們直接使用一個沒有宣告過的變數，會出現 `not defined` 的錯誤，但是如果使用 `typeof a` 會印出 undefined
``` JavaScript
a // ReferenceError: a is not defined
typeof a; // undefined
```

## 包裹物件
在以下範例裡面
``` JavaScript
var name = 'Alice';
console.log(name.length); // 5
console.log(name.toUpperCase()); // ALICE
```
一個字串之所以會有這些方法可以使用，是因為這些方法是被定義在一個字串包裹物件裡面，我們來看一下用不同的方式去定義

``` JavaScript
var name = 'Alice';
var name1 = new String(name);
console.log(name, name1)
```

在印出這個結果之後可以發現，透過 `new String` 出來的物件底下會多了很多，而這些就是字串預設可以調用的方法

![Alt text](https://firebasestorage.googleapis.com/v0/b/it-blog-a274d.appspot.com/o/new%20String.PNG?alt=media&token=12e7aba7-851f-49ee-9e95-579fe59a29ae)

而一般來說建議不要使用這種型式去建立字串，主要是因為用這種方法建立的變數並不會是一個原始型別，而是一個物件。

## 動態型別
JavaScript 是動態型別，也就是在執行階段才會確定型別

比如說
``` JavaScript
var num = 10;
console.log(typeof num); // number
num = 'Alice';
console.log(typeof num); // string
```

而 JavaScript 型別在轉換時會有兩種轉換的方式，一種是顯性轉換(Explicit conversion)一種是隱性轉換(Implicit Conversion)
### 顯性轉換 (Explicit Conversion)
顯性轉換與上面提到的直接去 assign 一個明確的數值時，會直接轉為該數值的型別
### 隱性轉換
``` JavaScript
var num = 10;
console.log(typeof num); // number
num += '';
console.log(typeof num); // string
```