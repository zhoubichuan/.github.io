---
title: mongodb安装教程
date: 2018-12-30 23:34:05
tags:
---
# Mongodb 安装

## 1.安装包

Windows 官方安装指南

mongodb32 位安装版 链接: https://pan.baidu.com/s/1SHJ1vre_CQOE3u-W0zniqQ 密码: chan

MongoDB64 位绿色版 链接: https://pan.baidu.com/s/1EkAB2SrcU1mfMfff_WDxtA 密码: w913

mongo 客户端 链接: https://pan.baidu.com/s/1YFxLZ-55D-WFR8os2fXN0A 密码: 61qd

## 2.安装可视化工具

### Robomongo

https://robomongo.org/?tdsourcetag=s_pcqq_aiomsg

## 3.mongodb 启动与连接

### 1.找到 mongodb 安装目录，一般是 C:\Program Files\MongoDB 2.6 Standard\bin

### 2.按下 Shift+鼠标右键，选择在此处打开命令窗口

### 3.在除 C 盘外的盘符新建一个空目录，如 D:\Mongodb\data

### 4.在命令行中输入 mongod --dbpath=刚创建的空目录，如

```
mongod --dbpath=D:\Mongodb\data
```

### 5.再按回车键

* 如果出现 waiting for connections on port 27017 就表示启动成功，已经在 27017 端口上监听了客户端的请求
* 注意：--dbpath 后的值表示数据库文件的存储路径，而且后面的路径必须事先创建好，必须已经存在，否则服务开启失败
* 注意：这个命令窗体绝对不能不能关，关闭这个窗口就相当于停止了 mongodb 服务
