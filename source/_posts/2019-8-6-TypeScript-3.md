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

而在 TypeScript 也可以在宣告變數時明確指定該變數的型別




