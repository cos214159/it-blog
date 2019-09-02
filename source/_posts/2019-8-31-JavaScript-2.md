---
title: JavaScript 筆記 - JavaScript 作用域
date:  2019-08-31 11:12:27
tags: 
  - JavaScript
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

## 執行環境以及執行堆疊
當 JavaScript 程式運行起來的時候會先行產生一個執行環境，在這個執行環境裡面會有一個全域物件和 this, 如果運行在瀏覽器上，這個執行環境會是 **window**, 而如果運行在 node.js 上這個執行環境會是 **global**, 除此之外還會有一個 **this** , 而當 function 被呼叫時, 會產生一個執行環境堆疊在全域執行環境之上, 這個就稱為**執行堆疊**  

比方說以下列程式來講, 他的堆疊順序為 全域環境 -> foo1() -> foo2()  等到 function 執行結束之後, 會再依序把 foo2 和 foo1 從執行堆疊中移除

``` JavaScript
function foo2() {
    var number2 = 20;
}
function foo1() {
    var number1 = 10;
    foo2();
}
foo1();
```

