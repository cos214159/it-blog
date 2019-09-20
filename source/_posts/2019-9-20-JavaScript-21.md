---
title: JavaScript 筆記 - JSON
date: 2019-09-20 06:06:02
tags: 
  - JavaScript
categories: 前端
---

JSON 為 JavaScript 的一個子集，他們的寫法非常的像，但是不盡相同，來看下以下範例

``` JavaScript
var family = {
  name: 'Alice',
  members: {
    father: 'Bob',
    mom: 'Julia',
    sister: 'Jane'
  }
}
console.log(JSON.stringify(family));

```

上面範例印出來的結果如下
{"name":"Alice","members":{"father":"Bob","mom":"Julia","sister":"Jane"}}

可以發現 JSON 的 key 值一定是用雙引號括起來的，而且一定是一個字串，如果用單引號或把雙引號拿掉都是不符合 JSON 規則的