---
title: TypeScript 系列筆記 (十一) - 泛型
date:  2019-08-22 07:46:42
tags: 
  - TypeScript
  - JavaScript
categories: 前端
---

如果是在寫 JavaScript 的話，基本上是不需要泛型的，然而因為 TypeScript 是強型別的關係，在編譯階段會需要檢查型別，有一些情形就顯得沒有動態型別這麼方便，比方說

``` JavaScript
function identity(arg: number): number {
    return arg;
}

identity(10);

```

在以上範例我們會希望傳入的參數與回傳值的型別是一樣的，但是上面的 function 在定義時已經限制了型別為 `number`，如果要支援其他型別的傳遞，就會需要重複宣告很多次重複的 function，比如說

``` JavaScript
function identity1(arg: string): string {
  return arg;
}

function identity2(arg: boolean): boolean {
  return arg;
}
```

這樣當然就會非常不方便，那當然最方便的做法就是將傳入值以及回傳值都宣告成 `any`，而這個也是一個泛型，但是這樣的話就不能保證回傳值一定跟傳入的值的型別是一致的

``` JavaScript
function identity(arg: any): any {
  return arg;
}
```

這個時候就可以使用泛型 (generic)

``` JavaScript
function identity<T>(data: T): T {
  return data
}

identity(20)
identity('Alice').length
identity<boolean>(false)
```

泛型的寫法在於使用這個記號 `<T>`，裡面的 T 是一個型別的代號，這個例子就是根據傳入的值，T 會自動帶入該值的型別，所以如果傳入數字，那 T 就會被替換成 `number`，如果傳入字串那 T 就會被替換成 `string`，而上面的例子，其中 `identity<boolean>(false)`，也可以自己去指定泛型的型別


