---
title: 模板字符串及其实现
copyright: true
permalink: 1
top: 0
date: 2016-06-03 18:47:40
categories:
- 前端开发
- 标准
- ECMA
- ES6
- 3.模板字符串及其实现
tags:
- ES6
- 模板字符串
---
## es6模板字符串 特殊的字符串
// 模板字符串取代了原有的字符串拼接功能
let name ='zfpx'
let age=9
//拼接到一起
let str= 'hello~\''+name+'今年'+age+'岁了'

//使用es6的模板字符串
let str2= `hello~${name}今年${age}岁了`

//拼写标签
let ul =`
        <ul>
            <li>${name}</li>
            <li>${age}</li>
        </ul>
        `
//好处：可以换行 可以支持取值

//？如何自己实现一个类模板字符串功能
let name2 ='zfpx'
let age2=9
let str2= 'hello${name2}今年${age2}岁了'
str2=str2.replace(/\$\{([^}]*)\}/g,function(){
    return eval(arguments[1])
})
console.log(str2)

//带标签的模板字符 自定义模板字符串的实现
//hello*zfpx*今年*9*岁了