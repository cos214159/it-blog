---
title: TypeScript 系列筆記 (三) - TypeScript 各種型別
date: 2019-08-06 00:52:58
tags: 
  - TypeScript
  - JavaScript
categories: 前端
---

那麼可以來進入到 TypeScript 的撰寫方式了，回顧一下之前講的，TypeScript 會出現是因為 JavaScript 的動態型別，我們不希望在指定一個整數給變數之後，又可以將字串指派給該變數，TypeScript 可以在編譯階段抓出這個寫法並提示這不合法。

## 指派不一樣的型別給同一個變數
用以下程式碼當作範例
``` JavaScript
let a = 10;
a = 'Hello World';
```

首先先指派整數 10 給變數 a，然後再把一個字串指派給 a 之後再進行編譯，這個時候就會出錯。

![TypeScript 編譯錯誤](https://firebasestorage.googleapis.com/v0/b/it-blog-a274d.appspot.com/o/tsc%20error.PNG?alt=media&token=6a3ff377-f726-481c-bd9c-4e74665779b1)

## TypeScript 針對各個型別的檢查

### 字串型別
``` JavaScript
let name = 'Tom';
name = 123; // error !
```
像這個例子就會有錯誤，因為我們把數字指派給一個存有字串的變數。

### 數字型別
``` JavaScript
let a = 10;
a = 1.1;
```
TypeScript 他不會特別區分整數和浮點數，因此上面這個設定是合法的。

### Boolean
``` JavaScript
let a = true;
a = false; // 正確  
a = 1; // 錯誤
```
布林在撰寫的時候要注意，當一開始把 Boolean 指派給變數之後，再把數字指派給他在 TypeScript 編譯時是不合法的。

### Arrays
陣列在 TypeScript 的檢查方式有點不一樣

``` JavaScript
let a = [1, 2, 3];
a = [4, 5]; // 正確
a = ['1', '2'] // 錯誤
```
以這個例子來說，TypeScript 編譯時會出現錯誤，而錯誤的原因是因為在一開始把一個陣列的值都設定成整數，而這個時候 TypeScript 會將這個陣列視為整數陣列，假如我們之後把一個字串陣列指派給 a 的話編譯時會出錯，但假使真的想要指派任意型別給變數的話，後面會介紹 `any` 就是把這個變數指定為任意型別。

但是假如一開始把陣列裡面的元素都是不同型別的話，那就相當於是 `任意型別`，之後重新指派新的陣列時，就不用符合原先的型別順序，如以下範例。

``` JavaScript
let arr = [1, 'Hello World', true];
arr = ["Hello World", false, 20]; // 正確
```

### any 任意型別
假如我們想要賦予一個變數任意型別的值，可以使用任意型別，用法也很簡單

``` JavaScript
let test: any = 1;
test = 'Hello World'; // 正確

let arr: any[] = [1, 2, 3];
arr = ['1', '2', '3']; // 正確
```

之前的陣列範例，當一個陣列的值型都一樣時，之後如果去指派一個字串陣列那編譯後會出錯，但是假如是想要做這個事情的話，只要在後面指定他是一個任意型別的陣列就可以 `any[]`。

### 明確指定變數型別
如果有學過 Java 或 C 的話，在宣告變數時，會有一些型別關鍵字，像

``` C
int a;
char b;
double c;
```

而在 TypeScript 也可以在宣告變數時明確指定該變數的型別，宣告的方式如下
``` JavaScript
let a: number = 1;
a = 10;

let str: string = 'Hello World';
str = 'test'
```

### tuple 數組
這個蠻特別的，之前所講的陣列，假如說是這樣寫並不會出錯，原因是陣列裡面所存的值型別並不相同。
``` JavaScript
let arr = [1, 'Hello World', true];
arr = ["Hello World", false, 20]; // 正確
```

但是假如我們會想要讓後續在指派新的陣列時，也要依照一開始所定義的型別順序的話，就可以在為每一個元素指定型別，範例如下

``` JavaScript
let arr: [number, string, boolean] = [1, 'Hello World', true];
arr = ["Hello World", false, 20]; // 編譯錯誤
```

這樣寫的話編譯時就會發生錯誤，原因就是因為新的陣列的值型別順序並沒有依照一開始給定的 `[number, string, boolean]` 去做排列。

### 列舉 (enum)
列舉在像 C、C# 或是 Java 都有這樣的用法，而 TypeScript 提供這樣的語法特性可以讓我們使用。

``` JavaScript
enum Week {
  Monday,
  Tuesday,
  Wednesday,
  Thursday,
  Friday,
  Saturday,
  Sunday
}

let day: Week = Week.Monday; // 0
```

用法上每一個列舉的項目的第一個值是從 0 開始，往後依序加一遞增，從 Monday 到 Sunday 的值依序為 0 ~ 6，但我們也可以改變這個數值順序

``` TypeScript
enum Week {
  Monday = 1,
  Tuesday,
  Wednesday,
  Thursday = 10,
  Friday,
  Saturday,
  Sunday
}
let Monday: Week = Week.Monday; // 1
let Tuesday: Week = Week.Tuesday; // 2
let Wednesday: Week = Week.Wednesday; // 3
let Thursday: Week = Week.Thursday; // 10
let Friday: Week = Week.Friday; // 11
let Saturday: Week = Week.Saturday; // 12
let Sunday: Week = Week.Sunday; // 13
```

這樣的編排順序只要其中一個列舉的值是有經過指定的，那之後就會從這個指定的值往上遞增。

### union type 聯合類型
假如一個變數我們想要讓他可以接受多個型別的值，可以使用 union type

``` JavaScript
let test: number | string = '123';
test = 123; // 正確
test = true // 錯誤
```

如以上範例，test 在宣告時使用聯合型別讓他可以接受數字和字串的值，但是這兩個型別以外的值沒有辦法通過編譯。

### never type
這個型別代表
  * 沒有被賦予任何值的變數
  * 會拋出異常不會有返回值的函數
 
``` JavaScript
let a: never;

function error(message: string): never {
  throw new Error(message);
}
```

如以上範例，變數 `a` 並沒有被賦予任何值，這個時候將型別註記為 never 是可以的，而 function error 內部有拋出例外，所以這個 function 不會有回傳值，那他也是可以註記為 never 型別

### null type
這個是在 TypeScript 2.0 提供的型別，在之前我們使用以下語法會是正確的

``` JavaScript
let a = 10;
a = null;
```

在 2.0 提供一個設定，只要在 `tsconfig.json` 打開這個設定 `"strictNullChecks": true,  `，就可以增加 null 的檢查，會在編譯時把 null 當成一個獨立的型別看待，這個時候再執行上面的程式會錯誤，因為不可以把 null 指派給存有數字型別的變數，如果想要讓編譯時通過那可以使用聯合型別。

``` JavaScript
let a: number | null = 10;
a = null; // 正確
```

有趣的是如果沒有增加這個設定 `"strictNullChecks": true,`，null 會被當成一個任意型別，假如一開始指派 null 給 a 的話，那 a 還是可以被指派為任何型別的值

``` JavaScript
let a = null;
a = 10; // 正確
a = '10' // 正確
a = function hello() { // 正確
  return 'hello world'
}
```

