---
title: JavaScript 筆記 - ASI
date: 2019-09-10 00:18:37
tags: 
  - JavaScript
categories: 前端
---

ASI 全名叫 Automatic Semicolon Insertion : 當 JavaScript 語句沒有加上分號時，會受到自動插入分號的規則影響。

比方說以下範例

``` JavaScript
function sayHi() {
  return 
    'Hi 你好'
}

sayHi(); // undefined
```

在 return 之後空一行會讓 ASI 在 return 關鍵字後自動加入分號，上面的範例會被自動調整成，`return` 後面會自動補上一個分號。
 
``` JavaScript
function sayHi() {
  return; 
    'Hi 你好';
}

sayHi(); // undefined
```

再來看一個例子，這個例子中在判斷式的後方都沒有加上分號，但 ASI 有正確的自動加入，所以執行上並沒有問題

``` JavaScript
if (2 > 5) console.log('2 大於 5')
else console.log('2 小於 5')
console.log('end')
```

## 立即函式不會自動加入分號
預設立即函式的後方不會加入分號，以下的程式執行時會發生錯誤，如果要排除這個錯誤就要在每個立即函式後加入分號
``` JavaScript
(function(){
   console.log('1')
})()

(function(){
   console.log('2')
})()
```