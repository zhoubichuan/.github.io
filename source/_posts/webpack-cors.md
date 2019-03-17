---
title: webpack-cors
copyright: true
permalink: 1
top: 0
date: 2019-03-17 18:27:05
categories:
- webpack
tags:
- webapck
- cors
- 跨域
- webpack跨域
---

## 安装

## 文件

### src/index.html

```
let xhr = new XMLHttpRequest();

xhr.open("GET", "/api/user", true);

xhr.onload = function() {
  console.log(xhr.response);
};

xhr.send();
```

### server.js

```
let express = require("express");

let app = express();

app.get("/user", (req, res) => {
  res.json({ name: "这是我" });
});
app.listen(3000);
```

## 配置

### webpack.config.js

第一种

```
devServer: {
    proxy: {
      "/api": {
        target: "http://localhost:3000",
        pathRewrite: { "/api": "" }
      }
    }
  }
```

第二种：模拟数据

```
before(app) {
    app.get("/api/user", (req, res) => {
    res.json({ name: "before" });
    });
}
```

第三中：修改 index.js 中请求地址

```
let xhr = new XMLHttpRequest();

xhr.open("GET", "/user", true);

xhr.onload = function() {
  console.log(xhr.response);
};

xhr.send();
```

打开http://localhost:3000/

[完整代码](https://github.com/zhoubichuan/frontend-note/tree/master/3.dev/3.scaffolding/1.webpack/2.config/5.cors)
