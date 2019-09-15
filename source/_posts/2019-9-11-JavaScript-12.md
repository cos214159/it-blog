---
title: JavaScript 筆記 - JavaScript 運算子
date: 2019-09-10 07:49:32
tags: 
  - JavaScript
categories: 前端
---

這篇來講到 JavaScript 的運算子，在以下的範例 `=` 是一個運算子，左右兩邊稱為 `運算元`。

``` JavaScript
name = 'Alice'
```

在一個運算式中如果只有一個運算元和運算子稱為 `一元運算子`，如果有兩個運算元和運算子稱為 `二元運算子`，如果有兩個運算子和三個運算元則稱為 `三元運算子`。

## 一元運算子
運算子在執行完之後會回傳一個值，以下範例中 執行完 delete 之後會回傳 true
``` JavaScript
var object = { name: 'Jack' };
delete object.name // true

typeof a // undefined
```

## 二元運算子
二元運算子種類就很多，以下就以算數運算子為例
``` JavaScript
1 + 1 // 2
3 * 4 //12
```

## 三元運算子
三元運算子的就只有一種，包含了兩個運算子和三個運算元
在 `:` 前後在文件上來看是一個表達式，表達式的特徵為會回傳一個值
``` JavaScript
var flag = true;
flag ? '真值' : '假值'
```

## 運算子相依性與優先性
運算子優先性 (Precedence) : 是決定如果有多個運算子，優先序較高的運算子優先序會成為優先序較低運算子的運算元

運算子相依性 (Associality) : 當有多個相同的運算子，相依性會決定運算子的運算方向

讓我們先來看一下以下的程式碼, 這個範例其實就拿我們現實生活當中的運算規則, 先乘除後加減去執行可以得知答案是 34, 而他實際的運作規則是因為 `*` 的優先序比較高, 而 `+` 的優先序比較低, 所以會先執行乘法的運算, 最後執行加法, 而 `=` 的運算式最低, 所以最後會把右邊的結果賦予給左側的變數

``` JavaScript
var result = 9 * 2 + 2 * 8
console.log(result); // 34 
```

而跟相依性有關的範例如下, 在 3 > 2 > 1 的範例居然會是 false 這個結果, 原因是因為小於的相依性, 會是從左而右執行, 所以會先執行 3 > 2 而這個結果回傳 true, 然後再用 true > 1 進行比較所以會是 false

``` JavaScript
1 < 2 < 3 // true
3 > 2 > 1 // false
```

最後來看一個例子, 關於相依性以及表達式, 在這個範例裡面透過 `Object.defineProperty` 定義了屬性, 額外還有給一個不可覆寫的 flag, 在這個結果就算進行賦值也沒有辦法複寫, 而在賦值之後會回傳一個結果, 這個是表達式的特性, 所以不管物件屬性的值如何最左邊的變數接收到的值是表達式回傳之後的結果

``` JavaScript
let obj = {};
Object.defineProperty(obj, 'a', {
  value: 10,
  writable: false
})

Object.defineProperty(obj, 'b', {
  value: 15,
  writable: false
})

console.log(obj.b)

let result = 1;
result = obj.a = obj.b = 100;
console.log(result)

```
