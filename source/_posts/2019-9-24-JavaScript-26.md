---
title: JavaScript 筆記 - 利用 Object.create() 實現多重繼承
date: 2019-09-24 09:11:40
tags: 
  - JavaScript
categories: 前端
---

在 Java 裡面如果要實現繼承的話，會有一個 base class 可以讓延伸類別去繼承，而繼承類別會有自己的屬性和方法，而在 JavaScript 裡面可以使用 Object.create 將其他的物件作為原型使用，達到類似 Java 繼承的效果

``` JavaScript
var Person = {
  name: 'default',
  height: 150,
  move: function() {
    console.log('走路');
  }
}

var Alice = Object.create(Person);
console.log(Alice.name, Alice.height, Alice.move());
```
這個時候可以發現 Alice 是一個空物件，但是在原型底下是 Person 結構
![Alt text](https://firebasestorage.googleapis.com/v0/b/it-blog-a274d.appspot.com/o/Object.create.PNG?alt=media&token=4df91cb3-9a31-42fc-a24f-e04fb2c649a0)

接下來我們來使用一個範例來看繼承

``` JavaScript
function Animal(height) {
  this.name = 'default';
  this.color = 'default';
  this.height = height;
}

Animal.prototype.move = function() {
  console.log('移動');
}

function Mouse(name, color) {
  Animal.call(this, 30);
  this.name = name;
  this.color = color;
}

Mouse.prototype = Object.create(Animal.prototype);
Mouse.prototype.constructor = Mouse // 要將原本的建構函式補回去，不然會被 Animal 取代
Mouse.prototype.hide = function() {
  console.log('躲避');
}

var miki = new Mouse('米奇', '白');
console.log(miki);
miki.hide();
```

主要是這一行 `Mouse.prototype = Object.create(Animal.prototype);
` 會讓 mouse 去繼承 Animal 的原型

接下來我們再宣告一個貓的建構函式去繼承 Animal，但是貓就會跟狗是無關的

``` JavaScript
function Animal(height) {
  this.name = 'default';
  this.color = 'default';
  this.height = height;
}

Animal.prototype.move = function() {
  console.log('移動');
}

function Cat(name, color) {
  Animal.call(this, 50);
  this.name = name;
  this.color = color;
}

Cat.prototype = Object.create(Animal.prototype);
Cat.prototype.constructor = Cat // 要將原本的建構函式補回去，不然會被 Animal 取代
Cat.prototype.catchObj = function() {
  console.log('捕捉獵物');
}

var kitty = new Cat('凱蒂', '白');
console.log(kitty);
kitty.catchObj();
```
