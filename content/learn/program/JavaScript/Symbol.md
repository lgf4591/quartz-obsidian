---
aliases: [Symbol ]
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:55:18
tags: [learn/program/javascript]
title: Symbol 
---
# Symbol
Symbol用于防止属性名冲突而产生的，比如向第三方对象中添加属性时。

Symbol 的值是唯一的，独一无二的不会重复的

## 基础知识

### Symbol

Symbol 不可以添加属性

### 描述参数

可传入字符串用于描述Symbol，方便在控制台分辨Symbol

传入相同参数Symbol也是独立唯一的，因为参数只是描述而已，但使用 `Symbol.for`则不会

使用`description`可以获取传入的描述参数

### Symbol.for

根据描述获取Symbol，如果不存在则新建一个Symbol

-   使用Symbol.for会在系统中将Symbol登记
-   使用Symbol则不会登记

### Symbol.keyFor

`Symbol.keyFor` 根据使用`Symbol.for`登记的Symbol返回描述，如果找不到返回undefined 。

### 对象属性

Symbol 是独一无二的所以可以保证对象属性的唯一。

-   Symbol 声明和访问使用 `[]`（变量）形式操作
    
-   也不能使用 `.` 语法因为 `.`语法是操作字符串属性的。
    

下面写法是错误的，会将`symbol` 当成字符串`symbol`处理

正确写法是以`[]` 变量形式声明和访问

## 实例操作

### 缓存操作

使用`Symbol`可以解决在保存数据时由于名称相同造成的耦合覆盖问题。

### 遍历属性

Symbol 不能使用 `for/in`、`for/of` 遍历操作

可以使用 `Object.getOwnPropertySymbols` 获取所有`Symbol`属性

也可以使用 `Reflect.ownKeys(obj)` 获取所有属性包括`Symbol`

如果对象属性不想被遍历，可以使用`Symbol`保护
