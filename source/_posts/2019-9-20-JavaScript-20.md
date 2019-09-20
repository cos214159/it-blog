---
title: JavaScript 筆記 - 陣列
date: 2019-09-20 07:46:02
tags: 
  - JavaScript
categories: 前端
---

在 JavaScript 除了原始型別以外都是物件，所以陣列也是一個物件，來看一下一般的陣列宣告方法

## 陣列實字
``` JavaScript
var arr = [
  10, 'Alice', false, { name: 'Bob' }
]

arr[0]
```

宣告陣列時裡面可以放置各種不同的型別，而取值的時候要使用中括號來取

## 新增一個值
要對陣列新增一個值可以使用 `push`

``` JavaScript
var arr = [
  10, 'Alice', false, { name: 'Bob' }
]

arr.push(20);
```

另外一種方法是直接使用索引的方式去新增一個值，但是要注意假如索引位置並不是一個連續的所以位置的話，那中間就會產生一個 undefined 的值，所以一般不建議使用這種方式

``` JavaScript
var arr = [
  10, 'Alice', false, { name: 'Bob' }
]

arr[5] = 20
console.log(arr); // [10, "Alice", false, {…}, empty, 20]
```