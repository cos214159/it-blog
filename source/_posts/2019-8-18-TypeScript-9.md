---
title: TypeScript 系列筆記 (九) - namespace
date: 2019-08-18 09:51:24
tags: 
  - TypeScript
  - JavaScript
categories: 前端
---

namespace 命名空間跟 C# 是一樣的概念，我們可以把一些相關的函式放在這個命名空間裡面，避免與其他的函式命名有所衝突。

``` JavaScript
namespace MathHelper {
  const PI = 3.14

  // 計算圓形面積
  export function calculateRoundArea(diameter: number) {
    return PI * diameter * diameter
  }

  // 計算矩形面積
  export function calcalateRect(width: number, height: number): number {
    return width * height
  }
}

console.log(MathHelper.calcalateRect(10, 20))
console.log(MathHelper.calculateRoundArea(10))
```

如以上範例命名空間的關鍵字是 `namespace`，如果要使用該命名空間裡面的函式那我們就只要 `MathHelper.calcalateRect(10, 20)` 這樣便可以調用。

而也許我們會需要將檔案拆開來好方便我們管理，而拆開的功能如果要整合在一支檔案裏面，這個時候的做法有兩種

### 使用指令方式將所有的檔案匯集在一起
比方說現在有三支檔案

**circle.ts**
``` JavaScript
namespace MathHelper {
  const PI = 3.14

  // 計算圓形面積
  export function calculateRoundArea(diameter: number) {
    return PI * diameter * diameter
  }
}
```

**rectangle.ts**
``` JavaScript
namespace MathHelper {
  // 計算矩形面積
  export function calcalateRect(width: number, height: number): number {
    return width * height
  }
}
```

**app.ts**
``` JavaScript
console.log(MathHelper.calcalateRect(10, 20))
console.log(MathHelper.calculateRoundArea(10))
```

這個時候可以發現到在 app.ts 裡面並沒有 MathHelper 而他實際定義在其他兩支檔案中，我們可以使用以下指令將這三支檔案的內容全部輸出到一支檔案，這樣在執行 `app.js` 時就可以正常使用

``` bash
tsc --outFile app.js circle.ts rectangle.ts app.ts
```

### 使用 TypeScript 匯入 namespace 的方式
``` TypeScript
/// <reference path="circle.ts" />
/// <reference path="rectangle.ts" />


console.log(MathHelper.calcalateRect(10, 20))
console.log(MathHelper.calculateRoundArea(10))
```

這個做法比較特殊，我們會使用 `/// <reference path="circle.ts" />` 這種語法將外部的命名空間檔案匯入，而引入之後還要執行以下指令，檔案內容就會全部都會出到 app.js 裡面

``` bash
tsc --outFile app.js app.ts
```

## 巢狀的命名空間
如果要將命名空間拆分的更細的話，可以使用這種語法，在內層的命名空間要加上 `export` 將該命名空間匯出

``` JavaScript
namespace MathHelper {
  const PI = 3.14

  export namespace Circle {
    // 計算圓形面積
    export function calculateRoundArea(diameter: number) {
      return PI * diameter * diameter
    }
  }
}
```
然後使用內部方法時就從最外層的命名空間，一路往內用 `.` 去調用
``` JavaScript
console.log(MathHelper.Circle.calculateRoundArea(10))
```

