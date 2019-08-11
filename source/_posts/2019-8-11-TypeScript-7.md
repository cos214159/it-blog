---
title: TypeScript 系列筆記 (七) - TypeScript 和 ES6
date: 2019-08-11 07:48:11
tags: 
  - TypeScript
  - JavaScript
categories: 前端
---

TypeScript 可以讓我們撰寫強型別的變數並且在編譯階段檢查型別是否正確，而 TypeScript 因為是 ES6 的超級 (superset)，他也可以幫我們編譯 ES6 的語法。

## ES6 語法
### let & const
在 ES6 裡面定義變數的關鍵字從 ES5 的 `var` 變成 `let` 和 `const`，而他們的差別在於作用域的不同，`var` 屬於 function scope 而 `let` 和 `const` 屬於 block scope，這個語法可以撰寫在 TypeScript 裡面，而編譯時會自動幫我們轉成 ES5 的語法，比方說以下範例在編譯時會出錯

``` JavaScript
{
  let a = 10;
}
console.log(a); // Cannot find name 'a'.
```
![let & const 編譯錯誤](https://firebasestorage.googleapis.com/v0/b/it-blog-a274d.appspot.com/o/ES6%20let%20const.PNG?alt=media&token=7f83a4a1-c2fd-4b9f-83ec-53d150662a76)

而編譯成功之後就跟使用 babel 幫我們編譯的結果是一樣的，以下為合法的程式碼

----> 編譯前
``` JavaScript
let str = '123'
function hello() {
  let str = 'hello'
}

hello();
console.log(str)
```
----> 編譯後
``` JavaScript
"use strict";
var str = '123';
function hello() {
    var str = 'hello';
}
hello();
console.log(str);
```



### arrow function
箭頭函式是 ES6 的新語法，而與 ES5 的最大不同就是 `this` 指向的位置不同，詳細的內容網路上有很多文章可以參閱。

----> 編譯前
``` JavaScript
const hello = (name: string) => 'hello' + name;
hello('JACK'); 
```

-----> 編譯後
``` JavaScript
"use strict";
var hello = function (name) { return 'hello' + name; };
hello('JACK');
```

可以看到箭頭函式被編譯成了傳統函式了。

### 函式預設參數 (default parameter)
函式預設參數 允許沒有值傳入或是傳入值為 undefined 的情況下，參數能以指定的預設值初始化。
以下範例在 function 的參數註記型別之後，可以再直接指派一個預設值，這是當呼叫此 function 的時候如果沒有帶參數那就會使用這個預設值。

``` JavaScript
const greeting = (name: string = 'JAMES') => {
  return 'hello ' + name
}
greeting();
```
### 展開運算子 (spread operator) 與 其餘運算子 (rest operator)
展開運算子與其餘運算子的寫法都是 `...`，但應用的地方不同先來看看展開運算子

#### 展開運算子 (spread operator)

展開運算子的作用是展開一個陣列當中的值，而這種特性可以幫助我們在開發上可以更方便

##### 拆解陣列或字串
``` JavaScript
let arr = [1, 2, 3];
console.log(...arr); // 1, 2, 3
```

我們也可以把一個字串拆解開來
``` JavaScript
let str = "hello world";
console.log(...str); // h e l l o   w o r l d
```

##### 陣列的合併
``` JavaScript
let arr1 = ['a', 'b', 'c'];
let arr2 = ['d', 'e', 'f'];

// 舊的寫法
let group = arr1.concat(arr2);

// 新的寫法
let newGroup = [...arr1, ...arr2];
```

##### shallow copy 淺複製
因為 JavaScript 的物件賦予到另外一個值上的時候，不會另外配置一塊新的記憶體，而是用傳參考的方式傳遞，因此可能會被別的操作而影響到自己本身的內容，例如

``` JavaScript
let userData = {
  name: 'BOB'
}

let newUserData = userData;
newUserData.name = 'TOM';

console.log(userData.name); // TOM
console.log(newUserData.name); // TOM
```

而我們可以利用 `Object.assign` 或是 `...` 展開運算子做淺複製，這樣就能保證針對複製過後的內容作操作不會影響到原始物件的內容。

``` JavaScript
let userData = {
  name: 'BOB'
}

let newUserData = Object.assign(userData);
newUserData.name = 'TOM';

let newUserData1 = { ...userData };
newUserData1.name = 'LARRY';

console.log(userData.name); // BOB
console.log(newUserData.name); // TOM
console.log(newUserData1.name); // LARRY
```

#### 其餘運算子 (rest operator)
其餘運算子雖然寫法跟展開運算子一樣，但是情境是使用在**把 function 當中的參數變成陣列的形式**，比方說

``` JavaScript
function sum(...arg: number[]) {
  let sum = 0;
  arg.forEach(function(item) {
    sum += item;
  })
  console.log(sum); // 15
}

sum(1, 2, 3, 4, 5);
```

或者也可以接收不一樣的參數並且轉成陣列，比方說
``` JavaScript
function foo(...arg: [number, boolean]) {
  console.log(arg[0]); // 10
  console.log(arg[1]); // false
}

foo(10, false);
```

### 解構賦值

解構賦值的特色在於把右邊的值鏡射到左邊的變數上，而我們可以針對陣列或物件做解構賦值。
#### 陣列解構賦值
``` JavaScript
const skill = ['HTML', 'CSS', 'JavaScript'];
const [skill1, skill2, skill3] = skill;
console.log(skill1, skill2, skill3);
```
#### 物件解構賦值
``` JavaScript
const userData = {
  name: 'BOB',
  height: 170
}
const { name: newName, height: newHeight } = userData; 
console.log(newName, newHeight);
```

### template literal 模版字符串
在過去 ES5 的時代，假如我們想要組多行的字串的話，要使用 斷行符號 `\n` 和 `+` 號把字串組起來，像以下範例。

``` JavaScript
const content = '<div>\n' + 
                  '<header>header</header>\n' + 
                  '<main>main</main>\n' +
                "</div>";
console.log(content);
```

但若是改成 template literal 寫法就簡單很多
``` JavaScript
const content = `
                  <div>
                    <header>header</header>
                    <main>main</main>
                  </div>
                `
console.log(content);
```



