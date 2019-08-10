---
title: TypeScript 系列筆記 (五) - TypeScript 的 Object 型別
date: 2019-08-09 15:50:45
tags: 
  - TypeScript
  - JavaScript
categories: 前端
---

在 TypeScript 定義 Object 時，一開始所指派的物件，後來在指派值的時候，是只能指派同樣的物件結構，比方說在一開始我們將 data assign 一個物件，之後只能在 assign 一個同樣結構的物件，像以下我們指派一個 空物件就會編譯錯誤，連屬性的名稱都要一樣才會正確

``` JavaScript
let data = {
  name: 'JAMES',
  sex: 'male',
  height: 170
}

data = {} // 錯誤
data = { // 錯誤
  name: 'Marry',
  sex: 'female',
  h: 160
}
data = { // 正確
  name: 'Marry',
  sex: 'female',
  height: 160
}
```

此外也可以在變數宣告時，註記物件型別

``` JavaScript
let data: { name: string, sex: string, height: number } = {
  name: 'JAMES',
  sex: 'male',
  height: 170
}
```

這樣就是在編譯時會檢查每個屬性是否有符合對應的型別才可以編譯成功。