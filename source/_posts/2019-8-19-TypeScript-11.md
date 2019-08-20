---
title: TypeScript 系列筆記 (十一) - interface
date: 2019-08-19 06:08:05
tags: 
  - TypeScript
  - JavaScript
categories: 前端
---

TypeScript 的 interface 跟物件導向的 interface 有點雷同，也比之前所介紹到的 type 功能更多。

物件導向的 interface 在於定義一組尚未實作的方法，並實作他，而 TypeScript 的介面來看一下他是怎麼一回事

``` JavaScript
function sayHello(person: { name: string }) {
  console.log(person.name + ' Hello');
}

const person = {
  name: 'Jason',
  height: 180
}

sayHello(person);
```

假設說在上面的範例中，呼叫 `sayHello` 時傳入的物件，在編譯時會檢查是否有 name 這一個 key 值，如果沒有編譯會出錯。

而之前有介紹到的 type 在於可以將這個檢查的型別抽出來共用，像下面這個範例
``` JavaScript
type Person = {
  name: string
}

function sayHello(person: Person) {
  console.log(person.name + ' Hello');
}

function setName(person: Person) {
  person.name = 'Alice';
}

const person = {
  name: 'Jason',
  height: 180
}

sayHello(person);
setName(person);
```

除此之外 TypeScript 還有提供 interface 的功能，可以做到一樣的事情，這樣寫的話一樣是會在呼叫 function 時檢查傳入的參數是否有跟介面定義的內容相符合。

``` JavaScript
interface Person {
  name: string
}

function sayHello(person: Person) {
  console.log(person.name + ' Hello');
}

function setName(person: Person) {
  person.name = 'Alice';
}

const person = {
  name: 'Jason',
  height: 180
}

sayHello(person);
```

而介面有幾個用法

## 檢查傳入的參數內容

根據傳入參數的方式，TypeScript 會做不一樣的檢查

### 先定義物件傳入
這種檢查會比較沒有那麼嚴格，在預先定義的物件當中，有多了 `height` 這個屬性，而這個屬性並沒有定義在 介面中，當以這種方式將物件傳入，編譯時並不會特別檢查出 height 沒有定義在介面中。

``` JavaScript
interface Person {
  name: string
}

function sayHello(person: Person) {
  console.log(person.name + ' Hello');
}

const person = {
  name: 'Jason',
  height: 180
}

sayHello({ name: 'Jason'});
```

### 直接傳入物件
用這種方式傳入物件，在編譯時會去檢查傳入的物件中所有的屬性是否跟介面定義的一致，如果沒有的話就會出錯
``` JavaScript
interface Person {
  name: string
}

function sayHello(person: Person) {
  console.log(person.name + ' Hello');
}

sayHello({ name: 'Jason', height: 180}); // Object literal may only specify known properties, and 'height' does not exist in type 'Person
```

那假如說我們想要直接傳入物件又想要通過檢查的話

第一種當然就是在 interface 多定義 height 這個屬性

``` JavaScript
interface Person {
  name: string,
  height: number
}

function sayHello(person: Person) {
  console.log(person.name + ' Hello');
}

sayHello({ name: 'Jason', height: 180 });
```

但是這樣寫就會是每次傳入物件時都一定要多帶 height 這個屬性，那 TypeScript 有給我們這個彈性是可傳可不傳，就只要在屬性旁邊加上一個 `?` 就好

``` JavaScript
interface Person {
  name: string,
  height?: number
}

function sayHello(person: Person) {
  console.log(person.name + ' Hello');
}

sayHello({ name: 'Jason' }); // 通過
```

那如果一開始並不知道資料的屬性欄位，那有一個額外的功能是可以定義一個變數，可以接受任意的屬性 `[propName: string]: any`

``` JavaScript
interface Person {
  name: string,
  height?: number,
  [propName: string]: any
}

function sayHello(person: Person) {
  console.log(person.name + ' Hello');
}

sayHello({ name: 'Jason', sex: 'male' }); // 通過
```

## 對方法進行檢查
如果定義的物件沒有符合 interface 的欄位，那在編譯時會出錯｀
``` JavaScript
interface Person {
    firstName: string;
    height?: number;
    [propName: string]: any;
    greet(lastName: string): void;
}

const person: Person = {
    firstName: 'Jason',
    height: 170,
    greet(lastName: string) {
        console.log('Hello ' + this.firstName + ' ' + lastName);
    }
}

function sayHello(person: Person) {
    console.log(person.name + ' Hello');
}

sayHello(person)
```

## 實作 implements
這個有點像 C 或 java, 可以使用 class 去實作一個介面

``` JavaScript
interface Person {
    firstName: string;
    height?: number;
    [propName: string]: any;
    greet(lastName: string): void;
}

class Teacher implements Person {
    firstName: string;
    
    constructor(firstName: string) {
        this.firstName = firstName;
    }

    greet(lastName: string) {
        console.log(lastName)
    }
}

```

## 用於 function type
這個作法在於定義一個 function 的介面, 會於編譯時檢查 function 的參數和回傳值是否有跟這個介面是一樣的

``` JavaScript
interface Sum {
    (number1: number, number2: number): number
}

const sum: Sum = function(number1, number2) {
    return number1 + number2
}

console.log(sum(10, 30))

```

## interface inheritance

介面繼承就是當宣告一個 interface 時可以藉由繼承去擴充或複寫該介面定義的欄位以及方法

``` JavaScript

interface Shape {
    width: number;
    height: number;
}

interface Circle extends Shape {
    diameter: number
}

const shape: Shape = {
    width: 100,
    height: 100
}

const circle: Circle = {
    width: 100,
    height: 100,
    diameter: 10
}
```