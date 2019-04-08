---
title: 你真的了解es6的let吗
copyright: true
permalink: 1
top: 0
date: 2019-04-06 18:46:41
categories:
- 前端开发
- 标准
- ECMA
- ES6
tags:
- ES6
- let
---
## let

### 首先看一段代码
```
for (var i = 0; i < 3; i++) {
  console.log(i);
}
console.log(i)
console.log(window.i)
```
在浏览器上的结果：
```
0
1
2
3
3
```
其中最后一个3是window.i

var不支持封闭作用域，如果不是在函数里声明的变量会声明到全局作用域（window）上

### 如果不想声明到全局上可以写一个自执行函数将i包在里面
```
(function() {
  for (var i = 0; i < 3; i++) {
    console.log(i);
  }
})();
console.log(i);
console.log(window.i);
```
在浏览器上查看结果：
```
0
1
2
Uncaught ReferenceError: i is not defined
    at <anonymous>:6:13
```
这是i也拿不到，因为已经封闭到自执行函数里面了
### 3
```
for (var i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000);
}
```
结果：
```
3
3
3
```
### 4.
```
for (var i = 0; i < 3; i++) {
  (function(i){
    setTimeout(function() {
        console.log(i);
      }, 1000);
  })(i)
}
```
结果：
```
0
1
2
```
- es6中提供新的let 和 const替代var

### 5
代码
```
for (let i = 0; i < 3; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000);
}
console.log(i)
```
结果：
```
Uncaught ReferenceError: i is not defined
    at <anonymous>:7:13
(anonymous) @ VM1874:7
0
1
2
```
先执行同步代码，i报错，然后执行异步代码，由于是块级作用域，let i=0会保存在{}中

let支持块级作用域声明的变量只会声明在当前的作用域内

let 可以解决作用域污染的问题 和局部作用域的问题