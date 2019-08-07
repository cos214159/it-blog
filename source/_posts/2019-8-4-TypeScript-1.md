---
title: TypeScript 系列筆記 (一) - 什麼是 TypeScript
date: 2019-08-04
tags: 
  - TypeScript
  - JavaScript
categories: 
  - 前端
---

我們都知道 JavaScript 是一個腳本語言，最初是運行在瀏覽器上，可以能動態更新畫面內容，讓使用者可以和畫面有更多的互動，但是因為一開始 JavaScript 撰寫的時間不是很長，在後來的新版本 ES6 有解決了一些問題，比方說 `hoisting`。

而其中 JavaScript 的`動態型別`就是導致 TypeScript 產生的原因，原先我一開始學的程式語言是 C 以及 Java，這些程式語言都是強型別的語言，在一開始宣告變數時，必須要指定變數的型別，而假如本來是用整數宣告的變數被賦予一個字串的話，在編譯階段就會出錯，而 JavaScript 動態型別則是在執行階段，會動態更動變數型別。

而就我個人的看法，動態型別在自己開發上其實也不是什麼大不了的事情，比方說在執行以下的 function 時，一定會知道這個 function 接受兩個數字型別去進行相加回傳，但是假如是給別人使用的話，別人可能會傳 `sum('2', '3')`，雖然說一樣可以得到回傳的結果，但是不會是預期的，這個時候我們還會需要去看一下裡面的原始碼才知道說原來我們是要傳數字進去。

``` JavaScript
function sum(a, b) {
  return a + b;
}
sum(1, 2) // 3 (正確)
sum('2', '3') // 23 (錯誤)
```

而 TypeScript 可以在編譯階段幫我們檢查傳入的變數型別是否有符合預期，而上面的寫法要改成

``` JavaScript
function sum(a: number, b: number) {
  return a + b;
}
sum(1, 2) // 3 (正確)
```

我們會在 function 的參數旁邊註記型別，這樣 TypeScript 就會幫我們檢查，但這種語法必須依賴相對應的編譯工具編譯為 JavaScript 才可以在瀏覽器上運行，就跟 Sass 一樣，那至於編譯工具要怎麼安裝，就在後續的文章中會有提到。