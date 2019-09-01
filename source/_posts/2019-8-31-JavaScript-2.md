---
title: JavaScript 筆記 - JavaScript 作用域
date:  2019-08-31 11:12:27
tags: 
  - JavaScript
  - CSS
categories: 前端
---

JavaScript 是屬於語法作用域也稱為靜態作用域，這個特性是在語法解析時就已經確定作用域，並且不會再改變，而動態作用域則是函式調用時才會決定作用域。

比方說，以下範例，在 function 建立呼叫時，JavaScript 會為該 function 創建一個執行環境，在這個執行環境裡面宣告的變數並不會影響到全域的變數，foo1 在執行的時候雖然內部的變數也是命名為 `name` 但是此變數並不會影響到全域的 name 所以再進一步呼叫 foo 的時候裡面是印出 `Jason`。

``` JavaScript
var name = "Jason"
function foo() {
  console.log(name); // Jason
}

function foo1() {
  var name = "Alice";
  foo();
}
foo1();
console.log(name);
```


