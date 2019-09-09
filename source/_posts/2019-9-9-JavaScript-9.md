---
title: JavaScript 筆記 - 陳述式及表達式
date: 2019-09-09 07:22:26
tags: 
  - JavaScript
categories: 前端
---

在 JavaScript 裡面的語句有分為**陳述式**和**表達式**

* statement 陳述式
特徵是在執行完一系列的程式碼之後，不會回傳結果
* expression 表達式
又稱為表示式、運算式
經常透過一些符號結合上下語句並運算及回傳結果

## 表達式
剛剛有說到表達式在執行完之後會回傳一個結果，例如以下的範例

``` JavaScript
'abc'
1 + 1
20
"Hello" + "World"
foo()

var name;
name = 'Alice'; // 回傳 Alice
```

## 陳述式 (statement)
陳述式的特點如剛剛說敘述的，在執行完上下語句之後，並不會回傳一個結果，比方說

``` JavaScript
var name;
if (name) {

}
```

而陳述式因為沒有回傳一個結果，所以他不能賦值在一個變數上

``` JavaScript
var result = if (a) {
  console.log('Hello World'); // SyntaxError: expected expression, got keyword 'if'
}
```

## 函式陳述式以及函式表達式
透過上面提到的陳述式以及表達式延伸至 function 上的話，可以細分為 **函式陳述式**和**函式表達式**，定義是一樣的，函式陳述式執行完之後不會回傳一個結果，而函式表達式執行完之後會回傳一個結果。

這兩者的差別就是陳述式在程式創建環境的時候，就會被預先分配一塊記憶體空間，而表達式會在程式創建的時候只有變數會被賦值，直到執行的時候才會被賦值

宣告上如下

* 函式陳述式
``` JavaScript
function foo() {
  console.log('foo');
}
```

* 函式表達式
``` JavaScript
var foo = function () {
  console.log('foo');
}
```