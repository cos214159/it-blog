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

## 使用建構式定義原型
在 ES5 的建構式是使用 function 定義，並使用 `new` 運算子建立一個物件，這個 new 跟 Java 的 new 的規則不一樣，他是先使用 new 建立一個空的物件，再用這個空的物件去呼叫 function 將傳入 function 的參數都寫入到這個空的物件裡面。

``` JavaScript
function Person(name, height) {
  this.name = name;
  this.height = height;
}

var Alice = new Person('Alice', 170);
console.log(Alice);
```

這個時候可以看到 Alice 的物件下展開之後，constructor 建構式是 Person
![Alt text](https://firebasestorage.googleapis.com/v0/b/it-blog-a274d.appspot.com/o/constructor.PNG?alt=media&token=deca10e6-3093-4fd7-b906-a40d7c1df42c)


### 為建構式加上原型方法
我們先看一下關於 Dog 的物件

``` JavaScript
function Person(name, height) {
  this.name = name;
  this.height = height;
}
console.dir(Person);
```
![Alt text](https://firebasestorage.googleapis.com/v0/b/it-blog-a274d.appspot.com/o/prototype_1.PNG?alt=media&token=ebb98748-086d-4e13-bedc-8aea3f779b67)

在這個物件裡面有著 function 特有的屬性叫 prototype，透過 prototype 去新增屬性就會掛載到原型下

``` JavaScript
function Person(name, height) {
  this.name = name;
  this.height = height;
}
Person.prototype.sayHi = function() {
  console.log('Hello' + this.name);
}

var Alice = new Person('Alice', 180);
console.log(Alice);
Alice.sayHi();
```
這個時候再來看看 Alice 底下的原型，可以發現到 sayHi 已經被掛載到原型上了
![Alt text](https://firebasestorage.googleapis.com/v0/b/it-blog-a274d.appspot.com/o/new%20prototype.PNG?alt=media&token=57591352-d6e5-46d5-bcae-52ae60ffdaec)

而這種將方法掛載到原型下的好處是，當物件宣告的數量非常多的時候，只需要一個記憶體來存放這個方法，可以有效的節省記憶體空間。

## 原始型別的包裹物件和原型
原始型別在宣告出來的時候，他其實已經繼承了原型的方法和屬性，而在純值的狀態下是看不到這原型，我們必須在包裹物件下才能看得到，比方說以下範例
``` JavaScript
var a = 'AAABBBCCC';
console.log(a.toLowerCase()); // aaabbbccc

var b = new String('AAABBBCCC');
console.log(b);
```

b 是利用建構式建立的物件，在這個狀態下我們就可以看到字串可以使用的所有方法。

而 String 他本身就是一個函式跟上面定義的 function 建構式一樣，因此他也有 `prototype` 這個屬性，我們也可以針對 String 下的原型去新增方法

``` JavaScript
String.prototype.getLast = function() {
  return this[this.length - 1];
}

var a = 'AAABBBCCC';
a.getLast();
```

當然數字型別也是可以新增原型方法

``` JavaScript
Number.prototype.plus = function(number) {
  return this + number;
}
var a = 10;
a.plus(20); // 30
```