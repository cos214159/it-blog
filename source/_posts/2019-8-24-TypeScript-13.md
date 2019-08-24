---
title: TypeScript 系列筆記 (十三) - 常用的 TypeScript 設定
date:  2019-08-24 20:23:47
tags: 
  - TypeScript
  - JavaScript
categories: 前端
---

在編譯 TypeScript 檔案的時候我們可以使用以下指令去做編譯

``` 
tsc
```
但是這個方法在編譯之後假如檔案發生改變還要再運行一次這個指令，那麼可以使用一個 `-w`option 這樣就會持續的去監聽這個檔案，發生改變時自動進行編譯。

```
tsc -w
```