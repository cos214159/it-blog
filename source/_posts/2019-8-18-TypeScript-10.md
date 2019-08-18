---
title: TypeScript 系列筆記 (十) - module
date: 2019-08-18 13:45:03
tags: 
  - TypeScript
  - JavaScript
categories: 前端
---

module 的使用方式跟 ES6 的 `import export` 差不多，但是要在瀏覽器上運行的話會需要額外的 module loader，

比如以下作法

**circle.js**
``` JavaScript
export const PI = 3.14

// 計算圓形面積
export function calculateRoundArea(diameter: number) {
  return PI * diameter * diameter
}
```

**rectangle.js**
``` JavaScript
// 計算矩形面積
export function calcalateRect(width: number, height: number): number {
  return width * height
}
```

**app.js**
``` JavaScript
import { PI, calculateRoundArea } from './circle'

console.log(PI)
console.log(calculateRoundArea(10))
```

這樣的結構在進行編譯之後產生的 `app.js` 程式碼如下
``` JavaScript
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
var circle_1 = require("./circle");
console.log(circle_1.PI);
console.log(circle_1.calculateRoundArea(10));
```

我們會發現裡面的關鍵字像 require、exports 瀏覽器目前不能運行這些語法，而我們可以使用以下的函式庫 `systemjs`，幫我們去載入這些 模組 

`https://www.npmjs.com/package/systemjs`

``` HTML
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <script src="node_modules/systemjs/dist/system.js"></script>
  <script>
      SystemJS.config({
        baseURL: '/',
        packages: {
          '/': {
            defaultExtension: 'js'
          }
        }
      });
      System.import('app.js');
  </script>
</body>
</html>
```

但這種寫法應該在業界上不常使用，應該還是會搭配 webpack 進行打包，所以這個單純只是做個紀錄，可以這樣使用。