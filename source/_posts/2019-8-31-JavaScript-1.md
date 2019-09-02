---
title: JavaScript 筆記 - JavaScript 的運作方式
date:  2019-08-31 09:33:11
tags: 
  - JavaScript
categories: 前端
---

JavaScript 是屬於**直譯式語言**與 C 或 Java 這類的 **編譯式語言** 不一樣，而兩者的差別就在於，編譯式語言會有一個編譯階段將我們的原始碼進行 compiler 然後在交給執行環境運行，而直譯式語言就是在執行階段一行一行的去解析程式碼，所以在執行階段編譯式語言會比直譯式語言還要再快一點。

## JavaScript 直譯器轉換過程
語法基本單元化 (Tokenize) -> 抽象結構樹 AST (Abstract Syntax Tree) -> 代碼生成

而要看到這一系列的資料，可以在這個網站上觀察  https://esprima.org/demo/parse.html# 
在這個網站上假如我們宣告以下這個變數

``` JavaScript
var greet = 'Hello World';
```

而這一串變數會先進行語法基本單元化，就像這一張圖

![Alt text](https://firebasestorage.googleapis.com/v0/b/it-blog-a274d.appspot.com/o/tokenize.PNG?alt=media&token=22adf8d7-c497-43b3-8311-608ef25d263e)


而之後就會進一步解析成 AST 
![Alt text](https://firebasestorage.googleapis.com/v0/b/it-blog-a274d.appspot.com/o/AST.PNG?alt=media&token=d46ff16e-4244-4e7f-b590-30d328518e23)

最後再進行代碼生成，程式真正被執行是在代碼生成之後

## LHS RHS
RHS 全名叫 right hand side 意思是把右側變數的值取出來
LHS 全名叫 left hand side 意思是把值賦予到左側的變數上

而左側如果不是一個變數便沒有辦法被賦予值，比方說

``` JavaScript
'hello world' = 10; // Uncaught ReferenceError: Invalid left-hand side in assignment
```

因為左側並不是一個變數那就沒有辦法被賦予值

而 RHS 的話就是像這樣，底下的 `str` 被宣告出來之後，會先被 RHS 取出，並且透過 LHS 賦予到右側的 `greet` 變數上。

``` JavaScript
var str = 'Hello World';
var greet = str; 
```

