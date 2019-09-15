---
title: JavaScript 筆記 - 寬鬆相等, 嚴格相等以及隱含轉型
date: 2019-09-15 11:59:12
tags: 
  - JavaScript
categories: 前端
---

JavaScript 的比對分成嚴格相等和寬鬆相等, 來分別做介紹

## 嚴格相等
嚴格相等會比較兩側的運算元的值以及型別，若是值和型別都是一致的，那就會回傳 true, 比方說

``` JavaScript
console.log(1 === '1'); // false
```

但是也是有例外, 比方說 NaN 與 NaN 比較結果為 false

``` JavaScript
console.log(NaN === NaN); // false
```

## 寬鬆相等
在做寬鬆相等時, 字串或布林會被轉為數字

在這個範例中, 字串 `'1'` 會被轉為數字與 1 比較, 所以結果為 true
``` JavaScript
console.log(1 == '1');
```

而在寬鬆相等下我們也可以比對 16 進制格式的字串
``` JavaScript
console.log(11 == '0x0B')
```

再來看幾個範例

這個範例左側的 'true' 會被轉成數字 Number('true') -> NaN 而右側的布林會被轉成數字 Number(true) 這比對過後結果是不一致的
``` JavaScript
console.log('true' == true); // false
console.log(true == '1') // true
console.log(false == '0') // true
```

這個範例比較特殊, 因為 `!0` 會是 true 然後在比對時會被轉為數字 1, 所以比對後結果為 true
``` JavaScript
console.log(1 == !0);
```

## null 與 undefined
這兩個型別比較會比較特殊, 首先先來看一下這兩個型別被轉為數字之後的值
``` JavaScript
console.log(Number(null)); // 0
console.log(Number(undefined)); // NaN
```

然後在比較時 null 和 undefined 是不會被轉成數字型別, 所以用 null 跟 0 比較會是 false

但是把 null 跟 undefined 去比較結果會是為 true
``` JavaScript
console.log(null == undefined); // true
console.log(null == 0); // false
```

## 物件與非物件的比對
物件與非物件的比對上是使用包裹物件來作轉換, 比如以下範例

在做比較時會使用包裹物件來作轉換, Number([10]) -> 10, String['B'] -> 'B'
``` JavaScript
console.log([10] == 10)
console.log(['B'] == 'B')
```

而如果是以下範例
物件在經過包裹物件轉換之後會變成 '[object Object]' 在與 [object Object] 為 true 不過一般來說是不會這樣比較
``` JavaScript
console.log(String({ 'name': 'Alice' })); // [object Object]
console.log('[object Object]' == { 'name': 'Alice' }); // true
```

## 物件與物件的比較
物件與物件比較時比對的是參考位置

``` JavaScript
console.log({} == {}); // false
console.log([] == []); // false

var a = {};
var b = a;
console.log(a == b); // true
```
