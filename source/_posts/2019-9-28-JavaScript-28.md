---
title: JavaScript 筆記 - Object.preventExtensions、Object.seal、Object.freeze
date: 2019-09-28 08:45:53
tags: 
  - JavaScript
categories: 前端
---

這個章節要來介紹這三個用法

## Object.preventExtensions
這個方法是可以防止新增屬性，來看看以下範例，假如說調用 Object.preventExtensions(obj) 之後便沒有辦法針對物件新增屬性，但是屬性的值如果是物件的話，還是可以針對物件新增巢狀的屬性

而這個方法並不會針對物件的屬性特徵進行任何修改，如果說要看屬性特徵的話可以使用 `Object.getOwnPropertyDescriptor` 查看

``` JavaScript
var obj = {
  a: 1,
  b: 2,
  c: {}
}

Object.preventExtensions(obj);
console.log(Object.isExtensible(obj));

// 調整屬性
obj.a = 10; // ok

// 擴充屬性
obj.d = 20; // 不合法

// 擴充巢狀屬性
obj.c.a = 30; // 合法

```

## Object.seal()
seal 會使物件屬性無法新增刪除，也無法重新設定特徵，但是可以調整目前屬性值
而物件預設會被加上 preventExtensions

而如果我們來看看物件的屬性特徵，可以利用 `Object.getOwnPropertyDescriptor` 會發現 每個屬性的特徵上，configurable 會被設定為 false 也就是無法被刪除

``` JavaScript
var obj = {
  a: 1,
  b: 2,
  c: {}
}

Object.seal(obj);
console.log(Object.isExtensible(obj));
console.log(Object.getOwnPropertyDescriptor(obj, 'a'));
// 調整屬性
obj.a = 10; // ok

// 擴充屬性
obj.d = 20; // 不合法

// 擴充巢狀屬性
obj.c.a = 30; // 合法

// 設定屬性特徵 (不合法)
Object.defineProperty(obj, 'a', {
  writable: false
});

// 刪除
delete obj.a; // 因為 configurable 被設定成 false 所以無法被刪除

```

## Object.freeze
物件會被加上 seal，並且無法調整值，而每個屬性 的 configurable 和 writable 都會被設定為 false

``` JavaScript
var obj = {
  a: 1,
  b: 2,
  c: {}
}

Object.freeze(obj);
console.log(Object.isSealed(obj)); // true
console.log(Object.isExtensible(obj)); // false
console.log(Object.isFrozen(obj)); // true
console.log(Object.getOwnPropertyDescriptor(obj, 'a'));
// 調整屬性
obj.a = 10; // 不合法

// 擴充屬性
obj.d = 20; // 不合法

// 擴充巢狀屬性
obj.c.a = 30; // 合法

// 設定屬性特徵 (不合法) TypeError: can't redefine non-configurable property "a"
Object.defineProperty(obj, 'a', {
  enumerable: false
});

// 刪除
delete obj.a; // 因為 configurable 被設定成 false 所以無法被刪除

```