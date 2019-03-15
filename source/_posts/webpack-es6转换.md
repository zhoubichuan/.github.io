---
title: webpack-es6转换
copyright: true
permalink: 1
top: 0
date: 2019-03-14 22:28:48
categories:
- webpack
tags:
- webpack
- es6
- babel
---

## 安装

```
yarn add babel babel-loader @babel/core @babel/preset-env -D
```

## webpack.config.js

```
{
    test: /\.js$/,
    use: {
        loader: "babel-loader",
        options: {
        presets: ["@babel/preset-env"]
        }
    }
}
```

## index.js

```
let fn = () => {
  console.log("log");
};
fn();
```

## 安装

```
yarn add @babel/plugin-proposal-class-properties -D
```

## webpack.config.js

```
{
    test: /\.js$/,
    use: {
        loader: "babel-loader",
        options: {
        presets: ["@babel/preset-env"],
        plugins: ["@babel/plugin-proposal-class-properties"]
        }
    }
}
```

## 安装

```
yarn add @babel/plugin-proposal-decoreators -D
```
