---
title: TypeScript 系列筆記 (十二) - 泛型
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

這種作法還有一個好處就是可以針對傳入的型別去做檢查，如果指定型別是數字但傳入的是字串，這樣編譯時會發生錯誤，而如果傳入的型別是數字，會沒有 `length` 這個屬性，這個錯誤也會檢查出來。

``` JavaScript
identity<number>('Bob'); // 錯誤
identity<number>(50).length // 錯誤
```

## 陣列泛型

陣列泛型主要是可以接受一個型別，而這個型別會規範這個陣列所有的元素型別，如以下範例如果 push 一個字串進去的話，那編譯時會出錯。

``` JavaScript
const arr:Array<number> = [1.2, 2.3]
arr.push(10);
arr.push('Hello'); // 錯誤
```

## function 中定義泛型陣列

在 宣告 function 時如同前面的範例，我們可以定義一個泛型，然後我們可以用這個型別去定義陣列參數，如果不符合定義的規範，在檢查時會出錯。

``` JavaScript
function printData<T>(arr: T[]) {
  arr.forEach(item => {
    console.log(item)
  });
}

printData<number>(["Alice", 123]);
```

## generic type

這種泛型型別可以應用的很廣泛，如以下範例在定義變數時，將該變數規範成 `<T>(data: T) => T` 這樣的型別

``` JavaScript
function printData<T>(arr: T[]) {
  arr.forEach(item => {
    console.log(item)
  });
}

const foo: <T>(data: T) => T = identity
console.log(foo('Hello'))
```

## generic class
我們也可以在定義類別時加上泛型

``` JavaScript
class MathHelper {
  _number1: number;
  _number2: number;
  constructor(number1, number2) {
    this._number1 = number1;
    this._number2 = number2;
  }

  sum() {
    return this._number1 + this._number2;
  }
}
```

如果是這個例子，當我們在定義該類別時會必須在每個變數加上型別檢查，但若是變數想要可以動態綁定數字或字串的話，就可以加上泛型定義

``` JavaScript
class MathHelper<T> {
  _number1: T;
  _number2: T;
  constructor(number1: T, number2: T) {
    this._number1 = number1;
    this._number2 = number2;
  }

  sum(): number {
    return +this._number1 + +this._number2;
  }
}
```

### Generic Constraints
在泛型中我們可以使用 `extends` 關鍵字讓型別限縮在 extend 的型別下，比如說像上面例子

``` JavaScript
class MathHelper<T extends number | string> {
  _number1: T;
  _number2: T;
  constructor(number1: T, number2: T) {
    this._number1 = number1;
    this._number2 = number2;
  }

  sum(): number {
    return +this._number1 + +this._number2;
  }
}

const mathHelper = new MathHelper<number>(10, 20);
const booleanMathHelper = new MathHelper<boolean>(true, false); // 錯誤
```

可以讓 class 只接受數字和字串的型別，其餘的型別傳入的話 編譯時會出錯

### 宣告類別時，定義多種的 generic type
如果我們想要在定義類別時，讓他可以接受數字以及字串的型別，那就可以使用多個泛型，如下所示，第一個泛型 `T` 接受數字或字串，第二個泛型 `U` 接受數字或字串，這樣宣告之後在變數上加上不同的泛型，在 new 出物件時參數給定就會比較彈性。

``` JavaScript
class MathHelper<T extends number | string, U extends number | string> {
  _number1: T;
  _number2: U;
  constructor(number1: T, number2: U) {
    this._number1 = number1;
    this._number2 = number2;
  }

  sum(): number {
    return +this._number1 + +this._number2;
  }
}

const mathHelper = new MathHelper<number, string>(10, "20");
console.log(mathHelper.sum());
```


