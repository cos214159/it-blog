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
