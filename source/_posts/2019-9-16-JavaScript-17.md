---
title: JavaScript 筆記 - 物件
date: 2019-09-16 08:12:16
tags: 
  - JavaScript
categories: 前端
---


## 物件實字以及用建構式建立物件
以下範例用大括號刮起來宣告物件的方式稱為物件實字, 物件可以允許定義巢狀的結構, 值可以是基本型別或物件型別以及 function, 屬性一律都是字串, 所以可以使用一些特殊的字元

另外一種方法則是使用建構式建立物件, 也就是 `new Object()`

``` JavaScript
var person = {
    name: 'Bob',
    height: 180,
    sayHi: function() {
        console.log('Hello!!')
    },
    friends: {
        teacher: 'Wang'
    }
}

var newPerson = new Object(person);
var strObj = new String('1'); // 字串包裹物件
var numberObj = new Number('1'); // 數字包裹物件
```

## 物件取值
物件取值的方式可以用 `.` 或是 `[]` 來做, 我們用上面的例子來看看

``` JavaScript
var person = {
    name: 'Bob',
    height: 180,
    sayHi: function() {
        console.log('Hello!!')
    },
    friends: {
        teacher: 'Wang'
    }
}

console.log(person.name, person.sayHi())
```

### 中括號取值的方式
中括號取值的方式可以帶入變數, 如果屬性是動態的話, 可以使用這個方法來取會很方便

``` JavaScript
var person = {
    name: 'Bob',
    height: 180,
    sayHi: function() {
        console.log('Hello!!')
    },
    friends: {
        teacher: 'Wang'
    }
}

var name = 'name';

console.log(person[name])
```

### 新增物件屬性
如果要新增物件屬性的話, 可以使用點或是中括號的方式去新增一個新的屬性

``` JavaScript
var person = {
    name: 'Bob'
}

person.height = 180;
```

### 刪除物件屬性
要刪除物件屬性可以使用 delete 這個運算子
``` JavaScript
var person = {
    name: 'Bob',
    height: 180
}

delete person.height
```

### 變數無法被刪除, 屬性可以

假如說我們用 var 去宣告一個變數時, 這個變數會被繼承到 window 下, 而在沒有使用 var 去宣告時, b 會被掛載到 window 下, 雖然看起來都是一樣, 但是會沒有辦法刪除 `a` 因為 a 是一個變數, 但是 b 會是一個屬性所以可以被刪除
``` JavaScript
var a = 10;
b = 20;
delete a;
delete b;
console.log(a); // 10
console.log(b); // not defined
```

## 物件與純值
物件與純值的差別就在於, 純值是無法新增屬性的, 但是屬性可以, 來看一下以下範例

``` JavaScript
var person1 = 'Alice';
person1.height = 180;
console.log(person1);

var person2 = {
    name: 'Alice'
}
person2.height = 180;
console.log(person2);
```

如果是用包裹物件建立的物件就可以新增自己的屬性

``` JavaScript
var number = new Number(1);
number.name = 'Alice';
console.log(number);
```

### function 是一個物件
藉由剛剛的概念我們來看一下 function, function 他本身是一個物件，所以我們可以對他去新增屬性, 但是如果是用 `console.log` 是看不到屬性的, 我們可以使用 console.dir 去觀看比較完整的物件結構
``` JavaScript
function callName(name) {
    console.log('name', name);
}

callName.height = 180;
console.dir(callName);
```

## 對物件調用一個不存在的屬性
當對物件調用一個不存在的屬性時, 會回傳 undefined

``` JavaScript
var person = {
    name: '小明'
}

console.log(person.height); // undefined
```