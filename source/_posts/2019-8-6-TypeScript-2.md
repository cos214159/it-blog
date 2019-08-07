---
title: TypeScript 系列筆記 (二) - TypeScript 基本環境
date: 2019-08-06 00:52:37
tags: 
  - TypeScript
  - JavaScript
categories: 前端
---

因為 TypeScript 並不能運行在瀏覽器上，我們必須要安裝相對應的工具來進行編譯，我們可以使用 `NPM` 來安裝。

## 安裝 TypeScript
首先先使用 NPM 將 TypeScript 編譯工具安裝到全域環境下

``` 
npm install -g typescript
```

### 建置一個基本環境
![TypeScript 資料夾結構](https://firebasestorage.googleapis.com/v0/b/it-blog-a274d.appspot.com/o/ts-category.PNG?alt=media&token=5126d37c-91c8-4be4-9462-de8cf381def1)

首先我們建立一個 `.ts` 檔和一個 `.html` 檔，HTML 以及腳本的結構如下

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
  <script src="./all.js"></script>
</body>
</html>
```

``` JavaScript
function sum(a: number, b: number) {
  return a + b;
}

alert(sum(1, 2)); 
```

很重要的一點是 TypeScript 目前並沒有辦法在瀏覽器上運行，必須依賴工具將 `all.ts` 轉成 JavaScript 檔案才可以使用，而我們在先前已經用 NPM 將編譯工具裝到全域環境下，那我們只需要打以下指令

![TypeScript 編譯指令](https://firebasestorage.googleapis.com/v0/b/it-blog-a274d.appspot.com/o/tsc.PNG?alt=media&token=efb34742-4cb9-4175-99cd-bf21a9b02324)

之後就會幫我們編譯出來一個 JavaScript 檔案囉 !

![TypeScript 編譯後的檔案](https://firebasestorage.googleapis.com/v0/b/it-blog-a274d.appspot.com/o/tsc%20result.PNG?alt=media&token=25c3ede5-9d52-4d84-91e4-cf735b8b32b1)

## 自動化編譯
剛剛使用 tsc 去編譯檔案，如果檔案有非常多個的話，要用 tsc 一個一個去指定 `.ts` 檔進行編譯，如果要比較方便的話，可以產生一個設定檔，之後可以利用 `tsc` 指令去編譯專案底下所有的 `.ts` 檔案。

### 產生 tsconfig.json
首先我們先建立設定檔，要建立設定檔也很簡單，只要打以下指令就可以幫我們建立一個 `tsconfig.json` 檔。

![建立 TypeScript 設定檔](https://firebasestorage.googleapis.com/v0/b/it-blog-a274d.appspot.com/o/tsc%20--init.PNG?alt=media&token=4454b94c-5dfa-429d-9ba4-3e28333beca8)

### 執行 tsc 指令
接下來我們只需要執行以下指令，在專案目錄下所有的 `.ts` 檔都會被編譯成 JavaScript 的檔案。
``` hash
tsc
```
![tsc 編譯結果](https://firebasestorage.googleapis.com/v0/b/it-blog-a274d.appspot.com/o/ts%20%E7%B7%A8%E8%AD%AF.PNG?alt=media&token=54a295d1-e855-48c7-bd27-3d4ce131cb5d)

