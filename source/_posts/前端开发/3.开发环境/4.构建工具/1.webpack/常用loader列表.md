---
title: 常用loader列表
copyright: true
permalink: 1
top: 0
date: 2019-03-08 22:31:24
categories:
- 前端开发
- 3.开发环境
- 4.构建工具
- 常用loader列表
tags:
- webpace
- less
- sass
- 配置
---
## 常用loader列表
webpack可以使用laoder来预处理文件。这允许你打包除JavaScript之外的任何静态资源。你可以使用Node.js来很简单地编写自己的loader。
### 文件
- raw-loader加载文件原始内容（utf-8）
- val-loader将代码作为模块执行，并将exports转为JS代码
- url-loader向file-loader一样工作，但如果文件小于限制，可以返回data URL
- file-loader将文件发送到输出文件夹，并返回（相对）URL
### JSON
- json-loader加载JSON文件（默认包含）
- json5-loader加载和转译JSON5文件
- cson-loader加载和转移CSON文件
### 转换编译
- script-loader 在全局上下文总执行一次JavaScript文件（如在script标签），不需要解析
- babel-loader 加载ES2015+代码，然后使用Babel转译为ES5
- buble-loader使用Buble加载ES2015+代码，并且将代码转译为ES5