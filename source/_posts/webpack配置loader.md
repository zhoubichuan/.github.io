---
title: 使用 webpack 打包 css 文件
date: 2018-12-29 23:23:09
categories:
- webpack
tags:
- webpack
- 入门
---

# 使用 webpack 打包 css 文件

## 1.安装 css-loader

```
npm i css-loader -D
```

## 2.安装 style-loader

```
npm i style-loader -D
```

## 3.配置 css-loader 和 style-loader

* 因为 css 并不是 js 模块

- 配置 module

```
module: {

  },
```

* 转换后

```
module: {
    rules: [
      {
        //转换文件的匹配正则
        test: /\.css$/,
        //css-loader用来解析处理css文件中的url路径
        //style-loader可以把css文件变成style标签插入header中
        //多个loader是有顺序的，从右往左写，因为转换是从右向左转换
        loader: ["style-loader", "css-loader"]
      }
    ]
  },
```

## 4.打包文件

```
npm run build
```

## 5.启动一个静态服务器

### 1.安装

```
npm i webpack-dev-server -D
```

### 2.配置

* package.json

```
"scripts": {
    "build": "webpack --mode development",
  },
```

--> 转换

```
"scripts": {
    "build": "webpack --mode development",
    "dev": "webpack-dev-server --open --mode development"
  },
```

### 3.webpack 配置

* webpack.config.js

```
devServer: {
    contentBase: "./dist",
    host: "localhost",
    port: 8080,
    //服务器返回给浏览器的时候是否启动gzip压缩
    compress: true
  }
```
