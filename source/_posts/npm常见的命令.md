---
title: npm常见的命令
copyright: true
permalink: 1
top: 0
date: 2019-03-10 23:07:23
categories:
- npm
tags:
- npm
- 命令
---

## 更新全部依赖

```
npm update
```

## 更新单个依赖

```
npm update <name>
```

## 查找全局安装过的包

```
npm ls -g
```

## npm 更新包

### 安装 npm-check

```
npm install -g npm-check
```

### npm 全局更新包

```
npm-check -u -g
```

### npm 更新单个全局包

```
npm update <name> -g
```

### npm 更新项目生产环境依赖包

```
npm update <name> --save
```

### npm 更新项目开发环境依赖包

```
npm update <name> --save-dev
```
