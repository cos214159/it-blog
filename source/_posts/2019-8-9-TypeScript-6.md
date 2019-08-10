---
title: TypeScript 系列筆記 (六) - 自訂 Type
date: 2019-08-09 19:03:09
tags: 
  - TypeScript
  - JavaScript
categories: 前端
---

我們已經知道在定義物件的時候可以加上屬性型別，如果是比較複雜的型別結構

``` JavaScript
let person: { name: string, height: number, hello: (name: string) => string } = {
  name: 'JAMES',
  height: 160,
  hello: function(name) {
    return name
  }
}
```

假使這個物件的型別我們想要重複使用的話，就要把上面的結構重新再複製一次，這樣的結構非常不容易維護而且難以閱讀

``` JavaScript
let person: { name: string, height: number, hello: (name: string) => string } = {
  name: 'JAMES',
  height: 160,
  hello: function(name) {
    return name
  }
}

let person1: { name: string, height: number, hello: (name: string) => string } = {
  name: 'JAMES1',
  height: 170,
  hello: function(name) {
    return name
  }
}
```

而 TypeScript 有給我們一個 type 的用法，可以把一些比較複雜的型別事先寫好，並且可以重複利用，如以下做法

``` JavaScript
type Person = {
  name: string, 
  height: number, 
  hello: (name: string) => string 
}

let person: Person = {
  name: 'JAMES',
  height: 160,
  hello: function(name) {
    return name
  }
}

let person1: Person = {
  name: 'JAMES1',
  height: 170,
  hello: function(name) {
    return name
  }
}

```

這樣的寫法自然會比較容易閱讀，可以清楚知道一個 person 的物件結構每一個欄位的型別是甚麼。