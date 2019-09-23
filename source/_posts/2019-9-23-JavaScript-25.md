---
title: JavaScript 筆記 - 原型 (prototype)
date: 2019-09-23 07:28:31
tags: 
  - JavaScript
categories: 前端
---

JavaScript 之所以會這樣取名，而且跟 Java 很像是因為在早期想要吸引 Java 的開發者過來使用，而光名字很像還不夠，其中 Java 有一個很大的特點是物件導向的開發，是採用類別繼承的方式可以將專案進行模組化的開發 (class -> object)，但是因為 JavaScript 除了純值以外其他都是物件並沒有 class 的概念，所以 JavaScript 採用原型的方式取模擬類似物件導向類別繼承的特性。

原型的特性
* 原型是具有物件的特性
* 繼承原型的物件，如果自己本身沒有值的時候會向上查找到原型的方法及屬性
* 共用方法及屬性，可以降低記憶體的使用

來看看以下範例，當我們把 a 印出來時，裡面會有一個 `__proto__`，在調用物件時，可以透過點或 [] 運算子向上層調用原型的屬性及方法，預設陣列的原型提供很多可以操作陣列的方法，比如說 `forEach`、`find` ... 等
``` JavaScript
var a = ['a', 'b', 'c'];
console.log(a);

a.forEach(function(i) {
  console.log(i);
})
```
![Alt text](https://firebasestorage.googleapis.com/v0/b/it-blog-a274d.appspot.com/o/prototype.PNG?alt=media&token=bd6a101c-c80c-48c4-946e-0d3545ecb0c6)

### 共用特性
原型的好處是可以在原型裡面加入一些方法共用，減少在宣告很多物件時記憶體的使用量，那在 `__proro__` 裡面可以自定義方法，這個方法將會被所有的陣列共用，但一般是不會透過 __proto__ 去新增原型方法

而在物件原型下新增的方法 `getName`，在陣列也可以使用，證明物件和陣列是共用同一個原型

``` JavaScript
var arr = ['a', 'b', 'c'];
var arr1 = [1, 2, 3];
var person = {
  name: 'Bob'
}
arr.__proto__.getLast = function() {
  return this[this.length - 1];
}
person.__proto__.getName = function() {
  return this.name;
}

arr.name = 'Alice';

console.log(arr.getLast(), arr1.getLast());
console.log(arr.getName(), person.getName());
```