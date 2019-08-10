---
title: TypeScript 系列筆記 (四) - TypeScript 的 function 型別
date: 2019-08-09 15:44:45
tags: 
  - TypeScript
  - JavaScript
categories: 前端
---

呼叫 function 時，可以使用 TypeScript 幫我們限制傳入的參數型別，以及回傳值的型別。

## 限制回傳的型別
比如說像這樣的寫法，就是強制回傳的時候一定要回傳字串
``` JavaScript
function hello(): string {
  return 'Hello World'
}

console.log(hello()); 
```

## function 無回傳值
如果要限制 function 無回傳值得話，可以使用 `void` 的 type
``` JavaScript
function hello(): void {
  return 'Hello World' // 錯誤
} 
```

在使用 void 的情況下，如果回傳值時會跳出警告

## function 參數加上強型別
以下的程式碼在做兩數相加的時候，雖然可以傳入非數字的值，但是回傳的結果並不會是我們預期的，而我們就可以加上強型別限制傳入的只能是數字。

``` JavaScript
function sum(a: number, b: number): number {
  return a + b;
}
sum(1, 2); // 正確
sum('1', '2'); // 錯誤
```

## function type

這個用法我覺得比較特殊，在將 function 傳給一個變數時，以下的作法是合法的
``` JavaScript
let fun;
fun = function sum(a: number, b: number): number {
  return a + b;
}

fun = function hello(name: string): string {
  return name
}
```

但是假如我們會希望指派的 function 跟一開始定義的結構一致的話，可以使用以下作法

``` JavaScript
let foo: (num1: number, num2: number) => number;
foo = function sum(a: number, b: number) {
  return a + b;
}

// 錯誤
foo = function hello(name: string): string {
  return name
}
```

