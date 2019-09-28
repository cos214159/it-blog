---
title: JavaScript 筆記 - 物件 getter setter
date: 2019-09-28 10:07:26
tags: 
  - JavaScript
categories: 前端
---

在 C# 的方法中，可以定義 get 和 set function 在存取值的時候做一些處理，而 JavaScript 有類似的用法

要注意的是 get 如果是在 console.log 印出預設值是 `(...)` 要在點開之後才會將最後的結果計算顯示 

``` JavaScript
var person = {
  total: 100,
  set save(price) {
    this.total = this.total + price
  },
  get save() {
    return this.total - 100;
  }
}

console.log(person);
person.save = 200;
console.log(person.save);

```

除此之外也可以使用 `Object.defineProperty` 新增 get set，但是用 Object.defineProperty 新增的 get 和 set 在特徵值上 configurable 和 enumerable 都是 false，也就是無法被刪除也無法被列舉

``` JavaScript
var person = {
  total: 100
}
Object.defineProperty(person, 'save', {
  set: function(price) {
    this.total = this.total + price
  },
  get: function() {
    return this.total - 100;
  }
})

person.save = 300;
```