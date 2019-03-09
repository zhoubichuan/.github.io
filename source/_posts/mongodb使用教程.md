---
title: mongodb使用教程
date: 2018-12-30 23:34:22
tags:
---
# Mongodb 使用

## 1.启动 mongodb

```
mongod --config D:\MongoDB\etc\mongodb.conf
```

```

```

## 2.数据库操作

### 1.使用数据库

```
use database_name
```

* 切换到 school 数据库下

```
use school
```

### 2.查看所有数据库

```
show dbs
```

### 3.查看当前使用的数据库

```
db
```

### 4.删除数据库

```
db.dropDatabase()
```

## 3.集合操作

### 1.查看集合帮助

```
db.students.help()
```

### 2.查看数据库下的集合

```
show collections
```

### 3.创建一个空集合

```
db.createCollection(collection_Name)
```

### 4.创建集合并插入一个文档

```
db.collection_Name.insert(document)
```

## 4.插入文档

### 1.insert

```
db.collection_name.insert(document)
```

### 2.save

```
db.collection_name.save(document)
```

## 5.更新文档

### 1.语法

```
db.collection.update(
    <query>,
    <updateObj>,
    {
        upsert:<boolean>,
        multi:<boolean>
    }
)
```

### 2.参数

* query 查询条件，指定要更新符合哪些条件的文档
* update 更新后的对象或指定一些更新的操作符
  * $set 直接指定更新后的值
  * $inc 在原基础上累加

- upsert 可选，这个参数的意思是，如果不存在符合条件的记录时是否插入 updateObj 默认是 false，不插入。
- multi 可选，mongodb 默认只更新查找到的第一条记录，如果这个参数为 true，就更新所有符合条件的记录。

### 3.upsert


### 4.multi

## 6.更新操作符

### 1.$set

### 2.$inc

### 3.$unset

### 4.$push

### 5.$ne

### 6.$addToSet

### 7.$each

### 8.$pop

### 9.修改指定索引元素

## 7.文档删除

## 8.查询文档

## 9.与和或

## 10.分页查询

## 11.执行脚本

## 12.权限

## 13.索引
