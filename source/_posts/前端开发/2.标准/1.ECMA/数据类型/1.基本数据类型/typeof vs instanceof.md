---
title: typeof vs instanceof
date: 2018-12-27 17:38:51
categories:
  - 前端开发
  - 2.标准
  - ECMA
  - 数据类型
  - 基本数据类型
  - 4.typeof vs instanceof
tags:
  - 数据类型
  - 基本数据类型
  - typeof vs instanceof
---

对于原始类型来说，直接用 instanceof 来判断类型是不行的，我们可以改造

```js
class PrimitiveString {
  static [Symbol.hasInstance](x) {
    return typeof x === "string";
  }
}
console.log("hhhh" instanceof PrimitiveString);
```

你可能不知道 Symbol.hasInstance 是什么东西，其实就是一个能
让我们自定义 instanceof 行为的东西，以上代码等同于 typeof 'hhh' === 'string',所以结果自然是 true 了
