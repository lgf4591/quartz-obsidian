---
aliases: [Promise]
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:55:06
tags: [learn/program/javascript]
title: Promise
---
# Promise

> 向军大叔每晚八点在 [抖音 (opens new window)](https://live.douyin.com/houdunren) 和 [bilibli (opens new window)](https://space.bilibili.com/282190994) 直播

`JavaScript` 中存在很多异步操作,`Promise` 将异步操作队列化，按照期望的顺序执行，返回符合预期的结果。可以通过链式调用多个 `Promise` 达到我们的目的。

Promise 在各种开源库中已经实现，现在标准化后被浏览器默认支持。

> promise 是一个拥有 `then` 方法的对象或函数

## 问题探讨

下面通过多个示例来感受一下不使用 `promise` 时，处理相应问题的不易，及生成了不便阅读的代码。

### 定时嵌套

下面是一个定时器执行结束后，执行另一个定时器，这种嵌套造成代码不易阅读

### 图片加载

下面是图片后设置图片边框，也需要使用回调函数处理，代码嵌套较复杂

### 加载文件

下面是异步加载外部`JS`文件，需要使用回调函数执行，并设置的错误处理的回调函数

实例中用到的 `hd.js` 与 `houdunren.js` 内容如下

如果要加载多个脚本时需要嵌套使用，下面`houdunren.js` 依赖 `hd.js`，需要先加载`hd.js` 后加载`houdunren.js`

> 不断的回调函数操作将产生回调地狱，使代码很难维护

### 异步请求

使用传统的异步请求也会产生回调嵌套的问题，下在是获取向军的成绩，需要经过以下两步

1.  根据用户名取得 `向军` 的编号
2.  根据编号获取成绩

> 示例中用到的 php 文件请在 [版本库 (opens new window)](https://gitee.com/houdunren/code) 中查看

启动 PHP 服务器命令 `php -S localhost:8080`

### 肯德基

下面是模拟肯德基吃饭的事情，使用 `promise` 操作异步的方式每个阶段会很清楚

而使用以往的回调方式，就会让人苦不堪言

## 异步状态

Promise 可以理解为承诺，就像我们去 KFC 点餐服务员给我们一引取餐票，这就是承诺。如果餐做好了叫我们这就是成功，如果没有办法给我们做出食物这就是拒绝。

-   一个 `promise` 必须有一个 `then` 方法用于处理状态改变

### 状态说明

Promise 包含`pending`、`fulfilled`、`rejected`三种状态

-   `pending` 指初始等待状态，初始化 `promise` 时的状态
-   `resolve` 指已经解决，将 `promise` 状态设置为`fulfilled`
-   `reject` 指拒绝处理，将 `promise` 状态设置为`rejected`
-   `promise` 是生产者，通过 `resolve` 与 `reject` 函数告之结果
-   `promise` 非常适合需要一定执行时间的异步任务
-   状态一旦改变将不可更改

promise 是队列状态，就像体育中的接力赛，或多米诺骨牌游戏，状态一直向后传递，当然其中的任何一个 promise 也可以改变状态。

![image-20191224100431808](https://doc.houdunren.com/assets/img/image-20191224100431808.4e777bac.png)

promise 没有使用 `resolve` 或 `reject` 更改状态时，状态为 `pending`

当更改状态后

`promise` 创建时即立即执行即同步任务，`then` 会放在异步微任务中执行，需要等同步任务执行后才执行。

`promise` 操作都是在其他代码后执行，下面会先输出 `houdunren.com` 再弹出 `success`

-   `promise` 的 then、catch、finally 的方法都是异步任务
-   程序需要将主任务执行完成才会执行异步队列任务

下例在三秒后将 `Promise` 状态设置为 `fulfilled` ，然后执行 `then` 方法

状态被改变后就不能再修改了，下面先通过`resolve` 改变为成功状态，表示`promise` 状态已经完成，就不能使用 `reject` 更改状态了

### 动态改变

下例中 p2 返回了 p1 所以此时 p2 的状态已经无意义了，后面的 then 是对 p1 状态的处理。

如果 `resolve` 参数是一个 `promise` ，将会改变`promise`状态。

下例中 `p1` 的状态将被改变为 `p2` 的状态

当 promise 做为参数传递时，需要等待 promise 执行完才可以继承，下面的 p2 需要等待 p1 执行完成。

-   因为`p2` 的`resolve` 返回了 `p1` 的 promise，所以此时`p2` 的`then` 方法已经是`p1` 的了
-   正因为以上原因 `then` 的第一个函数输出了 `p1` 的 `resolve` 的参数

## Then

一个 promise 需要提供一个 then 方法访问 promise 结果，`then` 用于定义当 `promise` 状态发生改变时的处理，即`promise`处理异步操作，`then` 用于结果。

`promise` 就像 `kfc` 中的厨房，`then` 就是我们用户，如果餐做好了即 `fulfilled` ，做不了拒绝即`rejected` 状态。那么 then 就要对不同状态处理。

-   then 方法必须返回 promise，用户返回或系统自动返回
-   第一个函数在`resolved` 状态时执行，即执行`resolve`时执行`then`第一个函数处理成功状态
-   第二个函数在`rejected`状态时执行，即执行`reject` 时执行第二个函数处理失败状态，该函数是可选的
-   两个函数都接收 `promise` 传出的值做为参数
-   也可以使用`catch` 来处理失败的状态
-   如果 `then` 返回 `promise` ，下一个`then` 会在当前`promise` 状态改变后执行

### 语法说明

then 的语法如下，onFulfilled 函数处理 `fulfilled` 状态， onRejected 函数处理 `rejected` 状态

-   onFulfilled 或 onRejected 不是函数将被忽略
-   两个函数只会被调用一次
-   onFulfilled 在 promise 执行成功时调用
-   onRejected 在 promise 执行拒绝时调用

### 基础知识

`then` 会在 `promise` 执行完成后执行，`then` 第一个函数在 `resolve`成功状态执行

`then` 中第二个参数在失败状态执行

如果只关心成功则不需要传递 `then` 的第二个参数

如果只关心失败时状态，`then` 的第一个参数传递 `null`

promise 传向 then 的传递值，如果 then 没有可处理函数，会一直向后传递

如果 onFulfilled 不是函数且 promise 执行成功, p2 执行成功并返回相同值

如果 onRejected 不是函数且 promise 拒绝执行，p2 拒绝执行并返回相同值

### 链式调用

每次的 `then` 都是一个全新的 `promise`，默认 then 返回的 promise 状态是 fulfilled

每次的 `then` 都是一个全新的 `promise`，不要认为上一个 promise 状态会影响以后 then 返回的状态

`then` 是对上个 promise 的`rejected` 的处理，每个 `then` 会是一个新的 promise，默认传递 `fulfilled`状态

如果内部返回 `promise` 时将使用该 `promise`

如果 `then` 返回`promise` 时，后面的`then` 就是对返回的 `promise` 的处理，需要等待该 promise 变更状态后执行。

如果`then`返回 `promise` 时，返回的`promise` 后面的`then` 就是处理这个`promise` 的

> 如果不 `return` 情况就不是这样了，即外层的 `then` 的`promise` 和内部的`promise` 是独立的两个 promise

这是对上面代码的优化，把内部的 `then` 提取出来

### 其它类型

Promise 解决过程是一个抽象的操作，其需输入一个 `promise` 和一个值，我们表示为 `[[Resolve]](promise, x)`，如果 `x` 有 `then` 方法且看上去像一个 Promise ，解决程序即尝试使 `promise` 接受 `x` 的状态；否则其用 `x` 的值来执行 `promise` 。

#### 循环调用

如果 `then` 返回与 `promise` 相同将禁止执行

#### Promise

如果返加值是 `promise` 对象，则需要更新状态后，才可以继承执行后面的`promise`

#### Thenables

包含 `then` 方法的对象就是一个 `promise` ，系统将传递 resolvePromise 与 rejectPromise 做为函数参数

下例中使用 `resolve` 或在`then` 方法中返回了具有 `then`方法的对象

-   该对象即为 `promise` 要先执行，并在方法内部更改状态
-   如果不更改状态，后面的 `then` promise 都为等待状态

包含 `then` 方法的对象可以当作 promise 来使用

当然也可以是类

如果对象中的 then 不是函数，则将对象做为值传递

## Catch

下面使用未定义的变量同样会触发失败状态

如果 onFulfilled 或 onRejected 抛出异常，则 p2 拒绝执行并返回拒因

catch 用于失败状态的处理函数，等同于 `then(null,reject){}`

-   建议使用 `catch` 处理错误
-   将 `catch` 放在最后面用于统一处理前面发生的错误

`catch` 可以捕获之前所有 `promise` 的错误，所以建议将 `catch` 放在最后。下例中 `catch` 也可以捕获到了第一个 `then` 返回 的 `promise` 的错误。

错误是冒泡的操作的，下面没有任何一个`then` 定义第二个函数，将一直冒泡到 `catch` 处理错误

`catch` 也可以捕获对 `then` 抛出的错误处理

`catch` 也可以捕获其他错误，下面在 `then` 中使用了未定义的变量，将会把错误抛出到 `catch`

### 使用建议

建议将错误要交给`catch`处理而不是在`then`中完成，不建议使用下面的方式管理错误

### 处理机制

在 `promise` 中抛出的错误也会被`catch` 捕获

可以将上面的理解为如下代码，可以理解为内部自动执行 `try...catch`

但像下面的在异步中 `throw` 将不会触发 `catch`，而使用系统错误处理

下面在`then` 方法中使用了没有定义的`hd`函数，也会抛除到 `catch` 执行，可以理解为内部自动执行 `try...catch`

在 `catch` 中发生的错误也会抛给最近的错误处理

### 定制错误

可以根据不同的错误类型进行定制操作，下面将参数错误与 404 错误分别进行了处理

### 事件处理

**unhandledrejection**事件用于捕获到未处理的 Promise 错误，下面的 then 产生了错误，但没有`catch` 处理，这时就会触发事件。该事件有可能在以后被废除，处理方式是对没有处理的错误直接终止。

## Finally

无论状态是`resolve` 或 `reject` 都会执行此动作，`finally` 与状态无关。

下面使用 `finally` 处理加载状态，当请求完成时移除加载图标。请在后台 php 文件中添加 `sleep(2);` 设置延迟响应

## 实例操作

### 异步请求

下面是将 `ajax` 修改为 `promise` 后，代码结构清晰了很多

### 图片加载

下面是异步加载图片示例

### 定时器

下面是封装的`timeout` 函数，使用定时器操作更加方便

封闭 `setInterval` 定时器并实现动画效果

## 链式操作

-   第个 `then` 都是一个 promise
-   如果 `then` 返回 promse，只当`promise` 结束后，才会继承执行下一个 `then`

### 语法介绍

下面是对同一个 `promise` 的多个 `then` ，每个`then` 都得到了同一个 promise 结果，这不是链式操作，实际使用意义不大。

![image-20191223201319405](https://doc.houdunren.com/assets/img/image-20191223201319405.03e9ad3b.png)

第一个 `then` 也是一个 promise，当没接受到结果是状态为 `pending`

`promise` 中的 `then` 方法可以链接执行，`then` 方法的返回值会传递到下一个`then` 方法。

-   `then` 会返回一个`promise` ，所以如果有多个`then` 时会连续执行
-   `then` 返回的值会做为当前`promise` 的结果

下面是链式操作的 `then`，即始没有 `return` 也是会执行，因为每个`then` 会返回`promise`

`then` 方法可以返回一个`promise` 对象，等`promise` 执行结束后，才会继承执行后面的 `then`。后面的`then` 方法就是对新返回的`promise` 状态的处理

### 链式加载

使用`promise` 链式操作重构前面章节中的文件加载，使用代码会变得更清晰

### 操作元素

下面使用 `promise` 对元素事件进行处理

### 异步请求

下面是使用链式操作获取学生成绩

## 扩展接口

### Resolve

使用 `promise.resolve` 方法可以快速的返回一个 promise 对象

根据值返加 `promise`

下面将请求结果缓存，如果再次请求时直接返回带值的 `promise`

-   为了演示使用了定时器，你也可以在后台设置延迟响应

如果是 `thenable` 对象，会将对象包装成 promise 处理，这与其他 promise 处理方式一样的

### Reject

和 `Promise.resolve` 类似，`reject` 生成一个失败的`promise`

下面使用 `Project.reject` 设置状态

### All

使用`Promise.all` 方法可以同时执行多个并行异步操作，比如页面加载时同进获取课程列表与推荐课程。

-   任何一个 `Promise` 执行失败就会调用 `catch`方法
-   适用于一次发送多个异步操作
-   参数必须是可迭代类型，如 Array/Set
-   成功后返回 `promise` 结果的有序数组

下例中当 `hdcms、houdunren` 两个 Promise 状态都为 `fulfilled` 时，hd 状态才为`fulfilled`。

根据用户名获取用户，有任何一个用户获取不到时 `promise.all` 状态失败，执行 `catch` 方法

可以将其他非`promise` 数据添加到 `all` 中，它将被处理成 `Promise.resolve`

如果某一个`promise`没有 catch 处理，将使用`promise.all` 的 catch 处理

### allSettled

`allSettled` 用于处理多个`promise` ，只关注执行完成，不关注是否全部执行成功，`allSettled` 状态只会是`fulfilled`。

下面的 p2 返回状态为 `rejected` ，但`promise.allSettled` 不关心，它始终将状态设置为 `fulfilled` 。

下面是获取用户信息，但不关注某个用户是否获取不成功

### Race

使用`Promise.race()` 处理容错异步，和`race`单词一样哪个 Promise 快用哪个，哪个先返回用哪个。

-   以最快返回的 promise 为准
-   如果最快返加的状态为`rejected` 那整个`promise`为`rejected`执行 cache
-   如果参数不是 promise，内部将自动转为 promise

下面将第一次请求的异步时间调整为两秒，这时第二个先返回就用第二人。

获取用户资料，如果两秒内没有结果 `promise.race` 状态失败，执行`catch` 方法

## 任务队列

### 实现原理

如果 `then` 返回`promise` 时，后面的`then` 就是对返回的 `promise` 的处理

下面使用 `map` 构建的队列，有以下几点需要说明

-   `then` 内部返回的 `promise` 更改外部的 `promise` 变量
-   为了让任务继承，执行完任务需要将 `promise` 状态修改为 `fulfilled`

下面再来通过 `reduce` 来实现队列

### 队列请求

下面是异步加载用户并渲染到视图中的队列实例

-   请在后台添加延迟脚本，以观察队列执行过程
-   也可以在任何`promise` 中添加定时器观察

### 高可用封装

上例中处理是在队列中完成，不方便业务定制，下面将 Promise 处理在剥离到外部

**后台请求处理类**

**队列处理类**

**后台脚本**

**使用队列**

## async/await

使用 `async/await` 是 promise 的语法糖，可以让编写 promise 更清晰易懂，也是推荐编写 promise 的方式。

-   `async/await` 本质还是 promise，只是更简洁的语法糖书写
-   `async/await` 使用更清晰的 promise 来替换 promise.then/catch 的方式

### Async

下面在 `hd` 函数前加上 async，函数将返回 promise，我们就可以像使用标准 Promise 一样使用了。

如果有多个 await 需要排队执行完成，我们可以很方便的处理多个异步队列

### Await

使用 `await` 关键词后会等待 promise 完

-   `await` 后面一般是 promise，如果不是直接返回
-   `await` 必须放在 async 定义的函数中使用
-   `await` 用于替代 `then` 使编码更优雅

下例会在 await 这行暂停执行，直到等待 promise 返回结果后才继执行。

一般 await 后面是外部其它的 promise 对象

下面是请求后台获取用户课程成绩的示例

也可以将操作放在立即执行函数中完成

下面是使用 async 设置定时器，并间隔时间来输出内容

### 加载进度

下面是请求后台加载用户并通过进度条展示的效果

### 类中使用

和 promise 一样，await 也可以操作`thenables` 对象

类方法也可以通过 `async` 与 `await` 来操作 promise

### 其他声明

函数声明

函数表达式

对象方法声明

立即执行函数

类方法中的使用

### 错误处理

async 内部发生的错误，会将必变 promise 对象为 rejected 状态，所以可以使用`catch` 来处理

下面是异步请求数据不存在时的错误处理

如果`promise` 被拒绝将抛出异常，可以使用 `try...catch` 处理错误

多个 await 时当前面的出现失败，后面的将不可以执行

如果对前一个错误进行了处理，后面的 await 可以继续执行

也可以使用 `try...catch` 特性忽略不必要的错误

也可以将多个 await 放在 try...catch 中统一处理错误

### 并发执行

有时需要多个 await 同时执行，有以下几种方法处理，下面多个 await 将产生等待

使用 `Promise.all()` 处理多个 promise 并行执行

让 promise 先执行后再使用 await 处理结果
