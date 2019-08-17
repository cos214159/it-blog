---
title: TypeScript 系列筆記 (八) - class
date: 2019-08-12 22:49:43
tags: 
  - TypeScript
  - JavaScript
categories: 前端
---

TypeScript 在 ES6 提供的 class 語法提供了很多物件導向的特性，比方說私有公有變數、繼承、抽象類別的特性，讓我們可以更好的去導入設計樣式的概念。

我們來看一個例子，比方說我們建立一個個人的基本資料類別

``` JavaScript
class Person {
  _name: string;
  private _id: string;
  protected _age: number;

  constructor(name: string, id: string, age: number) {
    this._name = name;
    this._id = id;
    this._age = age;
  }

  getName() {
    return this._name
  }

  setName(name) {
    this._name = name
  }

  protected getAge() {
    return this._age
  }

  private getId() {
    return this._id
  }
}

const person = new Person("JAMES", "A123456789", 12);
```
在這個範例裡面，一個人的基本資料有 `姓名`、`身分證`、`年齡`，宣告欄位的方法有三種，而除此之外也可以宣告私有以及公有方法，預設如果沒有特別指定，不管是變數或方法都是 public。

* public : 意思也就是在 class 外面也可以自由使用
* private : 只能在 class 內部使用
* protected : 跟 private 一樣，但是在繼承過後的 class 可以直接使用他

### 宣告變數的 style
一個 class 內部的變數，在宣告時最前面最好加上一個下底線，單純只是為了區分外部變數和 class 內部的變數。

以上述範例來說姓名是可以從外部直接使用的
``` JavaScript
person.name = 'JAMES1' // 正確
```

但是 `id` 和 `age` 是沒有辦法透過物件直接存取
``` JavaScript
console.log(person.id); // 錯誤
console.log(person.age); // 錯誤
```

當然方法也是一樣，只有公有方法可以直接呼叫，其餘都要從內部呼叫才合法。

``` JavaScript
console.log(person.getName()); // 正確
console.log(person.getAge()); // 錯誤
console.log(person.getId()); // 錯誤
```

## 繼承
TypeScript 也有把繼承的特性加入，如果有寫過 C 或 Java 的話，對以下的程式結構應該不陌生，`Staff` 繼承了 `Person` 而繼承類別必須在建構式中呼叫 上一層的建構式，`super()`。

``` JavaScript
class Staff extends Person {
  constructor(name: string) {
    super(name, 'Azyz', 18)
  }
}

const bob = new Staff("Bob")
console.log(bob)
```

在繼承類別中不可以取用父類別的私有變數，但是卻可以直接使用保護變數。

``` JavaScript
class Staff extends Person {
  constructor(name: string) {
    super(name, 'Azyz', 18)
    console.log(this.id) // Property 'id' is private and only accessible within class 'Person'.
    console.log(this.age) // 正確
  }
}
```

## 使用 get, set
在上面的範例針對 `name` 的操作，我們會增加 `getName` 和 `setName` 給外部呼叫，而還有另外的方式可以更方便的去撰寫這兩種函式

``` JavaScript
class Person {
  _name: string;
  private _id: string;
  protected _age: number;

  constructor(name: string, id: string, age: number) {
    this._name = name;
    this._id = id;
    this._age = age;
  }

  get name() {
    return this._name
  }

  set name(name: string) {
    this._name = name
  }
}

const person = new Person("James", "A123456789", 12);
person.name = 'Jason'

```




