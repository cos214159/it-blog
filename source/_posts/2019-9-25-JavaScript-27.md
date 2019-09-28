---
title: JavaScript 筆記 - Object.defineProperty
date: 2019-09-26 23:51:15
tags: 
  - JavaScript
categories: 前端
---

在 JavaScript 中每個物件都特徵，而這個特徵是沒有辦法在 console.log() 當中看到，而我們可以透過 Object.defineProperty 去調整屬性的特徵

而 Object.defineProperty 的用法如下
``` JavaScript
Object.defineProperty(obj, prop, descriptor)
```

descriptor 是可以調整該物件的特徵，可以調整的參數如下
* value: 值 
* writable: 可否寫入 (預設為 true)
* configurable: 可否被刪除 (預設為 true)
* enumerable: 可否被列舉 (預設為 true)

``` JavaScript
var obj = {
  a: 1,
  b: 2,
  c: 3
}

console.log(obj);

Object.defineProperty(obj, 'a', {
  value: 4,
  writable: true,
  configurable: true,
  enumerable: true
})
```
接下來我們來看看這些參數調整之後會影響甚麼功能

## writable

將 writable 改成 false 之後，便沒有辦法更動屬性
``` JavaScript
var obj = {
  a: 1,
  b: 2,
  c: 3
}

Object.defineProperty(obj, 'a', {
  value: 4,
  writable: false,
  configurable: true,
  enumerable: true
})
obj.a = 5;

console.log(obj); // Object { a: 4, b: 2, c: 3 }
```

假如把 writable 設定成 false 在嚴格模式底下如果調整這個屬性會出現錯誤

``` JavaScript
var obj = {
  a: 1,
  b: 2,
  c: 3
}

Object.defineProperty(obj, 'a', {
  value: 4,
  writable: false,
  configurable: true,
  enumerable: true
});

(function(){
  'use strict';
  obj.a = 5;
})();

console.log(obj); // TypeError: "a" is read-only
```

而 writable 只能做到淺層防護，假設說要改的是一個屬性是一個物件，他只能針對屬性本身進行寫入限制，但物件內部還是可以新增屬性

``` JavaScript
var obj = {
  a: 1,
  b: 2,
  c: 3
}

Object.defineProperty(obj, 'd', {
  value: {},
  writable: false
});
obj.d.a = 10;
console.log(obj); 
```

### configurable 
configurable 如果設定為 false 那就不可以刪除這個屬性
``` JavaScript
var obj = {
  a: 1,
  b: 2,
  c: 3
}

Object.defineProperty(obj, 'b', {
  configurable: false
});

delete obj.b; // false
```

### enumerable
enumerable 預設為 true, 如果設定為 false 那就表示為不可列舉
``` JavaScript
var obj = {
  a: 1,
  b: 2,
  c: 3
}

Object.defineProperty(obj, 'b', {
  enumerable: false
});

for (var key in obj) {
  console.log(key, obj[key]); // 只會列出 a c
}
```

## 一次設定多個屬性
利用 Object.defineProperties 可以一次設定多個屬性和特徵
``` JavaScript
var obj = {
  a: 1,
  b: 2,
  c: 3
}

Object.defineProperties(obj, {
  d: {
    enumerable: false
  },
  a: {
    enumerable: false
  }
})
```

### for in 會將自定義的原型屬性列出
先來看看以下範例，先宣告一個建構函式然後將這個建構函式的原型下新增一個屬性 name 最後利用建構函式 new 出一個物件

接下來使用 for in 印出 key，這個時候會發現 key 會包含 a 和 name，原因是因為自定義的屬性預設上 enumerable 是為 true，但預設的原型方法 enumerable 為 false 所以 for in 會將物件本身以及底下的原型可以列舉的屬性都印出來

``` JavaScript
function Foo() {}
Foo.prototype.name = 'Alice';
var obj = new Foo();
obj.a = 10;

for (var key in obj) {
  console.log(key); // a name
} 

Object.getOwnPropertyDescriptor(obj, 'a'); 
Object.getOwnPropertyDescriptor(obj.__proto__, 'name'); 
```

如果想要單純只印出當下物件內的屬性可以使用 `hasOwnProperty`，這個方法可以用來檢查 for in 列舉的 key 是否在當前這個物件下

``` JavaScript
function Foo() {}
Foo.prototype.name = 'Alice';
var obj = new Foo();
obj.a = 10;

for (var key in obj) {
  if (obj.hasOwnProperty(key)) {
    console.log(key);
  }
} 
```
