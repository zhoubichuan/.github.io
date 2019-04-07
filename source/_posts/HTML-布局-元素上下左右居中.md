---
title: 元素上下左右居中
date: 2018-12-23 21:35:29
categories:
- 前端开发
- 标准
- W3C
- HTML
tags:
- 静态页面
- css
- html
- 布局
---

# 元素上下左右居中

## 1.弹性盒子

### html

```
<div class="father">
    <div class="children"></div>
</div>
```

### css

```
.father {
    width: 200px;
    height: 200px;
    border: 1px solid black;
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
}

.children {
    height: 50px;
    width: 50px;
    background-color: red;
}
```

[查看代码](./弹性盒子.html)

## 2.translate

### html

```
<div class="father">
    <div class="children"></div>
</div>
```

### css

```
.father {
    width: 200px;
    height: 200px;
    border: 1px solid black;
    position: relative;
}

.children {
    height: 50px;
    width: 50px;
    background-color: red;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

[查看代码](./translate.html)

## 3.table-cell

### html

```
<div class="father">
    <div class="children"></div>
</div>
```

### css

```
.father {
    width: 200px;
    height: 200px;
    border: 1px solid black;
    display: table-cell;
    text-align: center;
    vertical-align: middle;
}

.children {
    height: 50px;
    width: 50px;
    background-color: red;
    display: inline-block;
}
```

[查看代码](./table-cell.html)

## 4.绝对定位

### html

```
<div class="father">
    <div class="children"></div>
</div>
```

### css

```
.father {
    width: 200px;
    height: 200px;
    border: 1px solid black;
    position: relative;
}

.children {
    height: 50px;
    width: 50px;
    background-color: red;
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
}
```

[查看代码](./绝对定位.html)

## 5.行内块元素

### html

```
<div class="father">
    <div class="children"></div>
</div>
```

### css

```
.father {
    width: 200px;
    height: 200px;
    border: 1px solid black;
    text-align: center;
    overflow: auto;
}

.father:after {
    content: "";
    display: inline-block;
    vertical-align: middle;
    height: 100%;
    margin-left: 0.25em;
}
```

[查看代码](./行内块元素.html)

## 6.负外边距

### html

```
<div class="father">
    <div class="middle">
        <div class="children"></div>
    </div>
</div>
```

### css

```
.father {
    width: 200px;
    height: 200px;
    border: 1px solid black;
}

.middle {
    width: fit-content;
    margin: 0 auto;
}

.children {
    height: 50px;
    width: 50px;
    background-color: red;
}
```

[查看代码](./负外边距.html)
