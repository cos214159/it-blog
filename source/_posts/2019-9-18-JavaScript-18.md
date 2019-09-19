---
title: JavaScript 筆記 - 物件參考特性
date: 2019-09-18 06:59:51
tags: 
  - JavaScript
categories: 前端
---

JavaScript 在賦予值在變數上時, 如果是原始型別是傳值, 如果是物件型別則是傳參考, 來看看以下範例

``` JavaScript
var name = 'Alice';
var newName = name;
newName = 'Bob';
console.log(name, newName); // Alice Bob

var person = {
    name: 'Alice'
}
var newPerson = person;
newPerson.name = 'Bob';
console.log(person, newPerson);
console.log(person === newPerson);
```

在以上範例純值在被賦予到另外一個變數時, 會新配置一塊記憶體給變數存值, 而該變數在被賦予到另外一個變數上時, 會將原先變數的值複製一份賦予到另外一個變數上, 那這樣的作法就是兩邊沒有任何關聯性

但物件就不同了, 物件在被宣告時, 所賦予的變數會存有一個記憶體位置, 而這個記憶體位置會指到另外一塊記憶體上, 這一塊記憶體才會存有實際物件所有的屬性和值, 之後該變數被賦予到另外一個變數時, 所賦予的值為記憶體位置, 並不會將實際的物件內容複製一份賦值給另一個變數, 這個稱為傳參考, 而如果是傳參考的話, `person` 和 `newPerson` 指到的是同一個記憶體位置, 操作的內容會是一樣的

來看幾個有趣的例子

以下這個範例因為 a.y 指向自己本身, 所以會造成無限循環
``` JavaScript
var a = {
    x: 1
}
a.y = a;
```

以下這個範例, 因為 `.` 運算子的優先序比較高, 所以 a.y 會先被執行, 然後會執行 `a = { x: 2 }` 的賦值, 而這個時候 a 已經指向到 { x: 2 } 這一塊記憶體, 跟原先的 { x: 1 } 已經沒有關聯, 最後將這個賦值的表達式回傳的 { x: 2 } 賦予到 a.y 上, 連帶的影響到 b 所以最後印出的結果是 undefined 和 { x: 1, y: { x: 2 } }
``` JavaScript
var a = { x: 1 };
var b = a;
a.y = a = { x: 2 };
console.log(a.y, b);
```