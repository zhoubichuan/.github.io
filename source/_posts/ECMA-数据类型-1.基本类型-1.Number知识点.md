---
title: Number 数据类型知识点
date: 2018-12-24 12:38:51
categories:
- 前端开发
- 标准
- ECMA
- 数据类型
- 基本数据类型
- number
tags:
- 数据类型
- 基本数据类型
- number
---

# number 数据类型知识点

## 1.js 中等号的三种情况

### 赋值

```
=
```

### 判断左右两边的值是否相等

```
==
```

### 判断左右两边的值是否觉得相等（包含数据类型）

```
===
```

## 2.number 包含正数、负数、零、小数、NaN

### NaN 不是一个数，但是属于 number 数据类型

```
typeof NaN
//"number"
```

## 3.把其他数据类型转换成 number

### 1.严格转换：Number()

```
Number(true)
//1
```

```
Number(false)
//0
```

```
Number(null)
//0
```

```
Number("")
//0
```

* undefined 是不能转换为有效数字

```
Number(undefined)
//NaN
```

```
Number('12')
//12
```

* 如果是把一个字符串转换为数字，只有字符串中的每一个字符都是有效数字字符，才可以正常转换，相反只要有一个不是有效数字字符(除了空格符)，最后结构就是 NaN

```
Number('12px')
//NaN
```

```
Number('  1 ')
//1
```

### 2.非严格转换

### parseInt/parseFloat 从左到右一次查找，把有效的数字字符转换为数字，但是一旦遇到一个非有效的数字字符，立即停止查找（不管后面是否还有都不再查找了）

```
parseInt('12px')
//12
```

```
parseInt('12px13')
//12
```

```
parseInt('px12')
//NaN
```

```
parseInt('12.5px')
//12.5
```

```
parseFloat('12.5px')
//12.5
```

## 4.数字常用的方法

### toFixed(n):保留小数点后面 n 位

```
var num=3.1415
num.toFixed(2)
//3.14
```

## 5.检测是否为有效数字

### isNaN()用来检测是否为有效数字的方法，如果当前的值是有效数字返回的结果是 false,如果不是有效数字，返回的结果是 true

```
isNaN(12)
//false
```

```
isNaN('qwer')
//true
```

* 如果检测的值不是 number 类型的，浏览器会默认先通过 Number 把它强制转换为 number 类型，然后再检测是否为有效数字

```
isNaN('12')
//false
```
