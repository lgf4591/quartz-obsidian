---
aliases: [Promise核心 ]
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:55:10
tags: [learn/program/javascript]
title: Promise核心 
---
# Promise核心
## 起步构建

> 向军大叔每晚八点在 [抖音 (opens new window)](https://live.douyin.com/houdunren) 和 [bilibli (opens new window)](https://space.bilibili.com/282190994) 直播

本章来自己开发一个 Promise 实现，提升异步编程的能力。

首先声明定义类并声明 Promise 状态与值，有以下几个细节需要注意。

-   executor 为执行者
-   当执行者出现异常时触发**拒绝**状态
-   使用静态属性保存状态值
-   状态只能改变一次，所以在 resolve 与 reject 添加条件判断
-   因为 `resolve`或`rejected`方法在 executor 中调用，作用域也是 executor 作用域，这会造成 this 指向 window，现在我们使用的是 class 定义，this 为 undefined。

下面测试一下状态改变

## THEN

现在添加 then 方法来处理状态的改变，有以下几点说明

1.  then 可以有两个参数，即成功和错误时的回调函数
2.  then 的函数参数都不是必须的，所以需要设置默认值为函数，用于处理当没有传递时情况
3.  当执行 then 传递的函数发生异常时，统一交给 onRejected 来处理错误

### 基础构建

下面来测试 then 方法的，结果正常输出`后盾人`

### 异步任务

但上面的代码产生的 Promise 并不是异步的，使用 setTimeout 来将 onFulfilled 与 onRejected 做为异步宏任务执行

现在再执行代码，已经有异步效果了，先输出了`houdunren.com`

### PENDING 状态

目前 then 方法无法处理 promise 为 pending 时的状态

为了处理以下情况，需要进行几点改动

1.  在构造函数中添加 callbacks 来保存 pending 状态时处理函数，当状态改变时循环调用
    
2.  将 then 方法的回调函数添加到 callbacks 数组中，用于异步执行
    
3.  resovle 与 reject 中添加处理 callback 方法的代码
    

### PENDING 异步

执行以下代码发现并不是异步操作，应该先输出 `大叔视频` 然后是\`后盾人

解决以上问题，只需要将 resolve 与 reject 执行通过 setTimeout 定义为异步任务

## 链式操作

Promise 中的 then 是链式调用执行的，所以 then 也要返回 Promise 才能实现

1.  then 的 onReject 函数是对前面 Promise 的 rejected 的处理
2.  但该 Promise 返回状态要为 fulfilled，所以在调用 onRejected 后改变当前 promise 为 fulfilled 状态

下面执行测试后，链式操作已经有效了

## 返回类型

如果 then 返回的是 Promise 呢？所以我们需要判断分别处理返回值为 Promise 与普通值的情况

### 基本实现

下面来实现不同类型不同处理机制

### 代码复用

现在发现 pendding、fulfilled、rejected 状态的代码非常相似，所以可以提取出方法 Parse 来复用

### 返回约束

then 的返回的 promise 不能是 then 相同的 Promise，下面是原生 Promise 的示例将产生错误

解决上面的问题来完善代码，添加当前 promise 做为 parse 的第一个参数与函数结果比对

现在进行测试也可以得到原生一样效果了

## RESOLVE

下面来实现 Promise 的 resolve 方法

使用普通值的测试

使用状态为 fulfilled 的 promise 值测试

使用状态为 rejected 的 Promise 测试

## REJEDCT

下面定义 Promise 的 rejecte 方法

使用测试

## ALL

下面来实现 Promise 的 all 方法

来对所有 Promise 状态为 fulfilled 的测试

使用我们写的 resolve 进行测试

其中一个 Promise 为 rejected 时的效果

## RACE

下面实现 Promise 的 race 方法

我们来进行测试

使用延迟 Promise 后的效果
