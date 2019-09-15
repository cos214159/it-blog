---
title: JavaScript 筆記 - promise、async await
date:  2019-09-01 22:44:26
tags: 
  - JavaScript
categories: 前端
---

在講 promise 之前先來看一下什麼叫做回調地獄 (callback hell)

## 回調地獄 (callback hell)
假如說今天我們要做很多事情，比方說洗澡後煮飯，煮飯後洗衣服，洗衣服後吃飯，以程式來說可能會寫成以下這樣，這樣的寫法很充分利用了 JavaScript function 一等公民的做法，將 function 當作參數傳入 function 可以讓 function 內部進行調用，但缺點就是當我們要實作的功能很多時，會導致程式過於巢狀。

``` JavaScript
let runProcess = function(callback) {
  callback();
}

runProcess(() => {
  console.log('洗澡')
  runProcess(() => {
    console.log('煮飯')
    runProcess(() => {
      console.log('洗衣服');
      runProcess(() => {
        console.log('吃飯');
      })
    })
  })
})
```

而要解決上述的巢狀結構問題，就可以使用 promise

## promise
promise 為 ES6 提供用來解決非同步的解決方案

一個 promise 會處於以下幾種狀態 : 

擱置（pending）：初始狀態，不是 fulfilled 與 rejected。
實現（fulfilled）：表示操作成功地完成。
拒絕（rejected）：表示操作失敗了。

promise 成功可以調用 `resolve` 回傳成功錯誤訊息，失敗的話可以調用 `reject` 

而成功的訊息可以使用 then 去接，失敗可則用 catch 去接

上面的函式基於 promise 可以改成這樣

``` JavaScript
let runProcess = function(message, status) {
  return new Promise((resolve, reject) => {
    if (status === true) {
      resolve(message + ' 成功');
    } else {
      reject(message + '失敗');
    }
  })
}

runProcess('洗澡', true).then((message) => {
  console.log(message);
  return runProcess('煮飯', true)
}).then((message) => {
  console.log(message);
  return runProcess('洗衣服', true)
}).then((message) => {
  console.log(message);
  return runProcess('吃飯', false)
}).catch((message) => {
  console.log(message);
})
```

## Macrotasks 和 Microtasks
JavaScript 是單執行緒, 如果有非同步的程式碼 JavaScript 會把這些工作加到 queue 裡面, 而依據不同的 api 可以細分為兩種不同的 工作, Macrotasks 和 Microtasks, 可以細分為以下的兩個部分
