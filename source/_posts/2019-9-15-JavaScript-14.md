---
title: JavaScript 筆記 - truthy 和 falsy
date: 2019-09-15 22:27:32
tags: 
  - JavaScript
categories: 前端
---

在一個判斷式的條件中, 如果是 true 就會執行, 反之 false 就不會執行, 而我們在執行表達式時回傳之後的結果會被隱含的視為真值或假值, 這稱為 truthy 以及 falsy, 

在這個網站裡面有很多關於真值或假值的對照表 
https://dorey.github.io/JavaScript-Equality-Table/


我大概列一下我自己覺得比較特殊的例子
truthy : 
* -1
* Infinity
* []	
* {}

falsy: 
* ""	
* undefined
* null
* NaN
* 0

``` JavaScript
if (10) {
    console.log('執行')
} else {
    console.log('不執行')
}
```