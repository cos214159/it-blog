---
title: JavaScript 筆記 - 單執行緒與非同步
date:  2019-09-01 21:42:14
tags: 
  - JavaScript
categories: 前端
---

JavaScript 本身是單執行緒，在執行程式時若是遇到非同步的程式比方說 `settimeout`、`setInterval`，會把這些非同步的行為往一個事件隊列裡面放

而這個事件隊列會有一個 event loop 不斷的去檢查裡面是否有 function 可以執行，但是執行的時間會是等主執行緒的功能全部都執行完才會執行裡面的 function。

``` JavaScript
console.log('start');
setTimeout(() => {
  console.log('process1');
}, 0);
console.log('end');
```

以上述程式來說，執行的結果會是
``` JavaScript
start
end
process1
```

在執行到 setTimeout 的 function 時，會先被放置到一個事件隊列裡面，會等到主執行緒的程式都執行完之後才會執行這個事件隊列裡面的內容。