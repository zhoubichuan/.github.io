---
title: vue中常见的指令
copyright: true
permalink: 1
top: 0
date: 2019-04-07 21:11:59
categories:
- 前端开发
- 基础知识
- 框架
- vue
tags:
- vue
- 常见的指令
---
## 安装
```
yarn add vue
```
## vue中常用的指令
- v-once
```
<div v-once>{{msg}}</div>
```
- v-html
```
<div v-html="d"></div>
```
- v-if控制的是dom，支持template
```
<template v-if="flag">
    <div>123</div>
</template>
<div v-else>321</div>
```
- v-show控制的是样式，不支持template
```
<div v-show="flag">hello</div>
```
- vm.$mount();
- vm.$nextTick();
