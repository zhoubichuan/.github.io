---
title: 包管理工具与配置项
copyright: true
permalink: 1
top: 0
date: 2019-03-10 23:07:23
categories:
  - 前端开发
  - 1.基础知识
  - 框架
  - 1.Vue
  - 1.入门
  - 2.包管理工具与配置项
tags:
  - vue
  - 包管理工具与配置项
---

## 1.常用的命令（项目构建和开发阶段）

```
# 生成 package.json 文件（需要手动选择配置）
npm init

# 生成 package.json 文件（使用默认配置）
npm init -y

# 一键安装 package.json 下的依赖包
npm i

# 在项目中安装包名为 xxx 的依赖包（配置在 dependencies 下）
npm i xxx

# 在项目中安装包名为 xxx 的依赖包（配置在 dependencies 下）
npm i xxx --save

# 在项目中安装包名为 xxx 的依赖包（配置在 devDependencies 下）
npm i xxx --save-dev

# 全局安装包名为 xxx 的依赖包
npm i -g xxx

# 运行 package.json 中 scripts 下的命令
npm run xxx

# 更新全部依赖
npm update

# 更新单个依赖
npm update <name>

# 查找全局安装过的包
npm ls -g

# 安装 npm-check
npm install -g npm-check

# npm 全局更新包
npm-check -u -g

# npm 更新单个全局包
npm update <name> -g

# npm 更新项目生产环境依赖包
npm update <name> --save

# npm 更新项目开发环境依赖包
npm update <name> --save-dev

```

比较陌生但实用的有：

```
# 打开 xxx 包的主页
npm home xxx

# 打开 xxx 包的代码仓库
npm repo xxx

# 将当前模块发布到 npmjs.com，需要先登录
npm publish
```

### 
