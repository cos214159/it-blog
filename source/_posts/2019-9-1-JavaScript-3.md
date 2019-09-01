---
title: JavaScript 筆記 - scope chain 範圍鍊
date:  2019-09-01 10:38:23
tags: 
  - JavaScript
  - CSS
categories: 前端
---

JavaScript 的作用域為語法作用域, 而語法作用域會以函式本身的語法來決定他的範圍鍊, 這個特性是當執行環境在本身沒有變數時會往他的外層尋找, 並不會跟其他的執行環境有任何的關係, 比方說以下範例

``` JavaScript
var number = 10;
function foo2() {
    console.log(number); // 10
    function foo3() {
        console.log(number); // 10
    }
    foo3();
}
function foo1() {
    console.log(number); // undefined
    var number = 20;
    foo2();
}
foo1();
console.log(number); // 10
```
除了 foo1 執行時, 在該執行環境裡面有宣告一個 number 的變數外其餘的 function 在印出 number 時皆為最外層全域所定義變數的值 `number = 10`

