---
title: webpack-watch的用法
copyright: true
permalink: 1
top: 0
date: 2019-03-17 11:07:31
categories:
- webpack
tags:
- webpack
- watch
---

## 配置

```
watch: true,
//监控选项
watchOptions: {
poll: 1000, //每秒 问我 1000次
aggregateTimeout: 500, //防抖 我一直输入代码
ignored: /node_modules/ //忽略监控的文件夹
},
```

[完整代码](https://github.com/zhoubichuan/frontend-note/blob/master/3.dev/3.scaffolding/1.webpack/2.config/3.watch/)
