---
title: JavaScript 筆記 - 物件淺層複製與深層複製
date: 2019-09-18 23:51:22
tags: 
  - JavaScript
categories: 前端
---

JavaScript 的物件是以傳參考的方式將本身的參考位置進行賦值，但有時候我們會想要複製一個完整的物件，不想要在操作物件內容時還會影響到原物件，有分為兩種複製方式

## 淺層複製 (shallow copy)
淺層複製只能做到第一層的複製，第二層還是以傳參考的方式進行傳遞

### 方法 1
``` JavaScript
var family = {
  name: 'Alice',
  members: {
    father: 'Bob',
    mom: 'Julia',
    sister: 'Jane'
  }
}

var newFamily = {}
for (var key in family) {
  newFamily[key] = family[key]
}

console.log(newFamily, family, newFamily === family);
```

### 方法 2
使用 ES6 的 Object.assign
``` JavaScript
var family = {
  name: 'Alice',
  members: {
    father: 'Bob',
    mom: 'Julia',
    sister: 'Jane'
  }
}

var newFamily = {}
newFamily = Object.assign({}, family)

console.log(newFamily, family, newFamily === family);
```

## 深層複製
深層複製就可以連同第二層以下的結構進行複製

### 方法 1
``` JavaScript
var family = {
  name: 'Alice',
  members: {
    father: 'Bob',
    mom: 'Julia',
    sister: 'Jane'
  }
}

var newFamily = {}
newFamily = JSON.parse(JSON.stringify(family));

console.log(newFamily, family, newFamily === family);
```

## 方法 2 
使用 lodash

``` JavaScript
var family = {
  name: 'Alice',
  members: {
    father: 'Bob',
    mom: 'Julia',
    sister: 'Jane'
  }
}

var newFamily = {}
newFamily = _.cloneDeep(family);
console.log(newFamily, family, newFamily === family);
```