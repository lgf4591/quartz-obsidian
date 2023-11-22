---
aliases: []
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:32:48
tags: [learn/program/algorithm/labuladong/common]
title: 用消息队列制作一款多人在线游戏  labuladong的算法小抄
---
# 用消息队列制作一款多人在线游戏 Labuladong的算法小抄
## 用消息队列制作一款多人在线游戏

[![](https://labuladong.gitee.io/algo/images/souyisou1.png)](https://labuladong.gitee.io/algo/images/souyisou1.png)

**通知： [数据结构精品课](https://aep.h5.xeknow.com/s/1XJHEO) 已更新到 V2.0； [第 13 期刷题打卡](https://mp.weixin.qq.com/s/eUG2OOzY3k_ZTz-CFvtv5Q) 最后几天报名！**

**———–**

上篇文章我讲了两种常用的随机算法，本文就把这些算法运用出来，做一个多人在线小游戏。

我小时候特别喜欢在 4399 玩一款叫做 Q 版泡泡堂的游戏：

[![](https://labuladong.gitee.io/algo/images/%e7%82%b8%e5%bc%b9%e4%ba%ba/0.jpg)](https://labuladong.gitee.io/algo/images/%e7%82%b8%e5%bc%b9%e4%ba%ba/0.jpg)

游戏里玩家可以操控一个机器人放炸弹，炸开障碍物能够获取随机道具，玩家消灭所有其他机器人则闯关成功，如果被其他机器人消灭，则闯关失败。

这个游戏中其他机器人都是电脑控制的，说实话有些蠢，我玩 Hard 难度一个小时就通关了。所以我在想，是否能够把这类炸弹人游戏做成多人在线的游戏，让几个好朋友联机 PK 呢？

### 分析

我对多人在线游戏的技术点并不了解，但是根据自己的游戏经验简单分析一下，我总结出来以下几个关键点：

1、需要「房间」的概念，在相同房间里的玩家才能一起对战，不同房间之间不能互相影响。

2、多人在线游戏肯定需要有一个后端服务供所有玩家连接，但由于这只是个小游戏，所以希望开发尽可能简单，后端最好不要有代码逻辑，所有逻辑都写在前端（游戏客户端）。

3、炸弹人游戏的初始地图会随机生成一些障碍物以增加游戏的难度和趣味性，但我希望随着游戏的进行，每隔一分钟就能重新生成一个新的随机地图。

4、最重要的，所有玩家的操作必须同步，或者说要保证「一致性」。比如你玩王者荣耀，如果你拆掉一座塔，那么要保证局内所有玩家都知道这座塔被拆了，不能因为某些玩家网络卡顿导致他还能看到这座塔，否则的话玩家们的视图就不同步了。

**其实用一个消息队列就可以满足上述要求**：

我们可以把消息队列的每个 topic 作为一个房间，然后把每个玩家的操作抽象成不同的 `Event`，由游戏客户端作为生产者将 `Event` 发到房间的 topic，游戏客户端同时也是消费者，从房间 topic 中读取并执行 `Event` 序列。这样一来，游戏房间的概念有了，而且所有游戏客户端展现的事件顺序就是消息队列中消息的顺序，能够保证不同玩家的操作都是同步的。

不过这里还有个问题，怎么做到每隔 1 min 随机生成新的地图呢？这个需求其实有点难办，你可以把生成新地图也抽象成一个 `Event`，但问题是这个 `Event` 该由谁发送呢？

显然你不能让每个客户端都持有一个 1 min 的计时器，所以我们可能需要在多个客户端之间进行「选主」的逻辑，保证只有一个 leader 客户端持有更新地图的权限，然后让这个客户端定时发出更新地图的 `Event`。

当然，如果这个 leader 客户端下线了，其他客户端应该能感知到，并确定一个新的客户端成为 leader，承担更新地图的任务。

### 设计思路

**首先需要一个游戏框架，我选择了 Go 语言的一款 2D 游戏框架，叫做 Ebitengine，官网如下**：

[https://ebitengine.org/](https://ebitengine.org/)

之所以选择这款 Go 语言的框架，主要是两个原因：

1、比较简单，适合快速上手写 2D 小游戏。

2、支持编译成 WebAssembly，如果需要的话可以直接编译到网页上运行。

这个库的使用原理特别简单，只要你实现这个 `Game` 接口的这两个核心方法就可以：

```
type Game interface {
    // 在 Update 函数里填写数据更新的逻辑
Update() error

    // 在 Draw 函数里填写图像渲染的逻辑
Draw(screen *Image)

    // ...
}
```

我们知道显示器能够显示动态影像的原理其实就是快速的刷新一帧一帧的图像，肉眼看起来就好像是动态影像了。

在每一帧图像刷新之前，这个游戏框架会先调用 `Update` 方法更新游戏数据，再调用 `Draw` 方法渲染出每一帧图像，这样就能够制作出简单的 2D 小游戏了。

**另外我们还需要一款消息队列作为后端，我选择 Apache Pulsar，官网如下**：

[https://pulsar.apache.org/](https://pulsar.apache.org/)

我在前文 [Apache Pulsar 的架构设计](https://labuladong.gitee.io/algo/5/43/) 介绍了 Pulsar 的一些原理，但并没有介绍它的基本用法，本文就来实践一下。

首先，Pulsar 支持多租户、多命名空间的企业级特性，也就是说一个 topic 的全名实际上是 tenant/namespace/topic。不过我们不用管这些，如果我们不指定租户名称和 namespace 名称创建一个名为 `room1` 的 topic，则会使用默认的租户名 public 和默认 namespace 名 default，创建一个全名是 `public/default/room1` 的 topic。

另外，我们说每个游戏客户端同时是生产者和消费者，Pulsar 的生产者只需要指定 topic 名字即可。但 Pulsar 的消费者这边抽象了一个 Subscription 的概念，有点类似 Kafka 的消费者组，但是更加灵活。

具体来说，Subscription 有三种模式，分别是 `Shared, Exclusive, Failover, Key_Shared`，下面贴一张官网的图：

[![](https://labuladong.gitee.io/algo/images/%e7%82%b8%e5%bc%b9%e4%ba%ba/4.png)](https://labuladong.gitee.io/algo/images/%e7%82%b8%e5%bc%b9%e4%ba%ba/4.png)

`Shared` 模式的 Subscription 可以被任意数量的 consumer 订阅，对应 topic 的消息会被负载均衡算法分发给多个 consumer。

`Key_Shared` 模式类似 `Shared` 模式，区别是能够根据消息的 key 进行负载均衡。

`Exclusive` 模式的 Subscription 只允许一个 consumer 独占连接，其他试图连接该 Subscription 的 consumer 将会被拒绝。

`Failover` 模式有些类似 `Exclusive` 模式，也是只能有第一个 consumer 能独占该 Subscription，但是后续试图连接该 Subscription 的 consumer 会作为备用，如果独占的 consumer 挂了，备用 consumer 能立刻补上。

在我们这个游戏的场景中，可以把玩家名称作为 Subscription 的名字，且把这个 Subscription 设置为 `Exclusive` 模式，这样如果有两个玩家用了同一个昵称，可以报错提示玩家重新设置：

```
roomName := inputRoomName()
playerName := inputPlayerName()

// 创建 pulsar client
client, err := pulsar.NewClient(pulsar.ClientOptions{
    URL:            "your-pulsar-cluster-url",
})
// topic 名称就是房间名
topicName := roomName + "-topic"
// 玩家的名称就是 subscription 名
subscriptionName := playerName + "-sub"

// 创建 pulsar consumer
consumer, err := client.Subscribe(pulsar.ConsumerOptions{
    Topic:            topicName,
    SubscriptionName: subscriptionName,
    // 使用 Exclusive 模式订阅
    Type:             pulsar.Exclusive,
    // 保证玩家第一次登录时，从最新的消息开始消费
    SubscriptionInitialPosition: pulsar.SubscriptionPositionLatest,
})

if err != nil {
    log.Fatal("玩家昵称" + playerName + "已经被其他人用过了，请换一个")
}

// 保证玩家再次登录时，从最新的消息开始读
err = consumer.SeekByTime(time.Now())
```

代码中的 `SubscriptionInitialPosition` 用来设置创建这个 Subscription 时开始消费消息的位置，我们设置为 `Latest` 的意思是忽略之前的消息，从最新的消息开始消费。

但为什么需要调用 `SeekByTime` 方法呢，这需要解释一下 Pulsar 中 Subscription 的机制。

在 Pulsar 中，一个 Subscription 就好像是一个指向某个消息的命名指针，一旦创建之后就会持久化在 broker 端。也就是说这个 `SubscriptionPositionLatest` 只能设置 Subscription 创建时指向最新的消息，如果再次使用这个 Subscription 的话，并不能保证指向最新的消息。

具体到我们的游戏中是以下场景：

1、玩家**首次**使用用昵称 `player1` 进入房间 `room1`，此时相当于在 Pulsar 中新建了一个名为 `room1-topic` 的 topic，然后新建了一个名为 `player1-sub` 的 Subscription 去消费 `room1-topic` 中的消息。由于设置 `SubscriptionInitialPosition` 为 `SubscriptionPositionLatest`，所以 `player1-sub` 指向 `room1-topic` 中的最新消息。

2、玩家 `player1` 退出游戏，consumer 断开和 Pulsar 的连接，但此时 Pulsar 中已经保存了名为 `player1-sub` 的 Subscription，指向 `player1` 退出时最后消费的那条消息，我们假设是 `msg-X`。

3、虽然玩家 `player1` 退出了，但房间 `room1` 中还有其他玩家在向 `room1-topic` 发送事件消息。

4、过了一段时间，玩家再次使用昵称 `player1` 进入房间 `room1`，此时 Pulsar broker 发现 `room1-topic` 中已经有名为 `player1-sub` 的 Subscription，且该 Subscription 将从 `msg-X` 开始消费，所以 `player1` 将会看到类似放电影的场景：自己下线后所有其他玩家的操作都会重放一遍。

所以为了避免这种「放电影」的情景出现，我们需要手动调用 `SeekByTime` 方法，让重新登录的玩家也从最新的消息开始消费，投入战斗。

> PS：回想一下，我们在玩 MOBA 游戏时，如果由于网络原因短暂卡顿重连，也会出现类似放快速放电影的情况。所以我猜测真实的多人在线游戏可能真的是通过类似消息队列的机制来保证玩家之间同步的。

当然这里有一个潜在的 bug：**对于一个分布式消息系统来说，考虑到网络延迟、系统时钟的差异，时间戳的语义是不明确的，我们其实不应该依赖消息的时间戳**。

所以更好的一个方式是在玩家退出时调用 `Unsubscribe` 方法，相当于手动删除存储在 Pulsar broker 里的 Subscription：

```
func Close() {
    consumer.Unsubscribe()
consumer.Close()
    // ...
}
```

再考虑随机生成地图的功能，如何在地图中随机生成障碍物可以使用前文 [水塘抽样算法](https://labuladong.gitee.io/algo/4/32/113/) 来实现。关键是我们需要在多个游戏客户端之间进行类似「选主」的操作，可以利用一个 `Exclusive` 模式的 Subscription 来达到目的：

```
// 这个函数每分钟调用一次，试图向后端发送更新地图的事件
func trySendUpdateEvent(client pulsar.Client) {
    // 所有房间内的玩家都有相同的房间名，所以他们的 mapSubscriptionName 都相同
    mapTopicName := inputRoomName() + "-map-topic"
    mapSubscriptionName := inputRoomName() + "-map-sub"
    // 抢占这个 Subscription，抢到的那个客户端才能发起更新地图的请求
    _, err := client.Subscribe(pulsar.ConsumerOptions{
        Topic:            mapTopicName,
        // Exclusive 模式下只有第一个 consumer 能连接成功
        Type:             pulsar.Exclusive,
        SubscriptionName: mapSubscriptionName,
    })
    
    if err != nil {
        // 已经有别的 consumer 抢到这个 Subscription 了
        // 让他们更新地图吧
        return
    }
    // 我抢到了这个 Subscription，我负责来更新随机地图
    producer, err := c.client.CreateProducer(pulsar.ProducerOptions{
        Topic:           mapTopicName,
    })
    // 生成新的随机地图 event，发送到 pulsar 中
    payload := getUpdateMapEventData()
    producer.Send(context.Background(), &pulsar.ProducerMessage{Payload: payload})
}
```

假设游戏房间名称是 `room1`，那么玩家的动作将会发送到名为 `room1-topic` 的 topic 中，而地图的更新操作将会发送到名为 `room1-map-topic` 的 topic 中。

有的读者可能好奇，为什么要给地图更新单独建立一个 topic 呢？直接把更新地图的 `Event` 也直接发到 `room1-topic` 里面不行吗？其实是不行的。

根据我们前面的代码，玩家登录后会从最新的消息开始消费，那么玩家大概率收不到这个更新地图的 `Event`，也就无法初始化地图，只下一次更新地图的时才能完成地图的初始化。

而如果把地图的更新事件放在另一个专用的 topic 中，玩家登录后只需从这个 topic 读取最新的消息，就可以得到初始化地图了。

想要读取 topic 中最新的那条消息，可以用 Pulsar 提供的 `Reader` 接口：

```
reader, err := c.client.CreateReader(pulsar.ReaderOptions{
    Topic: mapTopicName,
    // 指向最新的那一条消息
    StartMessageID: pulsar.LatestMessageID(),
    StartMessageIDInclusive: true,
})

if reader.HasNext() {
    // 读取最新的地图消息，初始化地图
    msg, err := reader.Next(context.Background())
    updateMap(msg)
}
```

Pulsar 的 `Reader` 接口就好比一个迭代器，可以通过 `HasNext` 和 `Next` 方法一条一条读取消息，不过在这里我们仅仅使用它来读取地图 topic 中最新的消息，其他时候还是用 consumer 读取消息。

上述代码演示了使用 Pulsar 实现多人游戏的核心逻辑，下面再介绍一些关键的代码实现

### 关键代码实现

根据前文的内容，每个游戏客户端需要持有一个 producer，用来把玩家的操作事件发送到操作事件对应的 topic 中，其中有一个客户端需要一个额外的 producer，定期将新的随机地图发到地图更新事件的 topic 中。另外，每个游戏客户端需要持有两个 consumer，分别订阅操作事件的 topic 和地图更新的 topic。

所以我就使用 go 语言的 channel 来处理所有事件的输入和输出：

```
type Game struct {
    // 从 pulsar 中接收事件
receiveCh chan Event
// 向 pulsar 中发送事件
sendCh chan Event

    // 地图、其他玩家的坐标、炸弹坐标等等游戏数据
    // ...
}
```

所有需要发往 Pulsar 的事件只要塞到 `sendCh`，Pulsar 的 producer 实例就会把事件发往对应的 topic；其他玩家产生的操作事件都会被发到 `receiveCh` 中，只要渲染这些事件即可在当前玩家的屏幕上显示出其他玩家的操作。

这个 `Event` 是我自己实现的一个接口，该接口声明了一个 `handle` 方法：

```
type Event interface {
    // 传入 Game 结构，可以修改游戏数据
handle(game *Game)
}
```

这样，只要我们把用户的操作抽象成不同的 `Event`，然后实现对应的 `handle` 方法即可。比如我列举几个关键的事件：

```
// 放置炸弹的事件
type SetBombEvent struct {
bombName string
pos      Position
}

func (e *SetBombEvent) handle(game *Game) {
    // ...
    go func() {
        // 3 秒后炸弹爆炸
        explodeTimer := time.NewTimer(3 * time.Second)
        <-explodeTimer.C
        // 发送炸弹爆炸的事件
        game.sendSync(&ExplodeEvent{
            bombName: bombName,
        })
    }()
}


// 炸弹爆炸的事件
type ExplodeEvent struct {
bombName string
pos      Position
}

func (e *ExplodeEvent) handle(game *Game) {
// ...
    go func() {
        // 2 秒后炸弹爆炸的火焰消失
        undoTimer := time.NewTimer(2 * time.Second)
        <-undoTimer.C
        // 发送爆炸结束的事件
        game.sendSync(&UndoExplodeEvent{
            pos: bomb.pos,
        })
    }()
}


// 爆炸结束的事件
type UndoExplodeEvent struct {
pos Position
}

func (e *UndoExplodeEvent) handle(game *Game) {
// ...
}


// 玩家移动的事件
type UserMoveEvent struct {
    // ...
}

func (a *UserMoveEvent) handle(g *Game) {
// ...
}
```

有了这个 `Event` 接口，结合 Ebitengine 游戏框架的使用，我们可以这样实现关键的 `Update` 和 `Draw` 方法：

```
// 这个函数会在每一帧显示前调用，用于更新游戏数据
func (g *Game) Update() error {
// 1、非阻塞地接收并处理一个事件，更新游戏数据
select {
case event := <-g.eventCh:
event.handle(g)
default:
}

    // 2、监听玩家的键盘操作，发给后端的 pulsar
var event = listenPlayerKeyboardEvent()
    g.sendSync(event)
    // ...

    return nil
}

// 这个函数会 Update 后调用，用于显示游戏界面
func (g *Game) Draw(screen *ebiten.Image) {
    // 画出地图和障碍物
for pos, _ := range g.obstacleMap {
ebitenutil.DrawRect(screen, float64(pos.X*gridSize), float64(pos.Y*gridSize), gridSize, gridSize, obstacleColor)
}

    // 画出炸弹
for pos, _ := range g.posToBombs {
ebitenutil.DrawRect(screen, float64(pos.X*gridSize), float64(pos.Y*gridSize), gridSize, gridSize, bombColor)
}

    // 画出每个玩家的位置
for _, player := range g.nameToPlayers {
// ...
}

    // 画出炸弹爆炸后的火焰
for pos, val := range g.flameMap {
// ...
}
}
```

**当然，本文中的代码是大幅简化过的，省略了诸如错误处理的细节，不过现在整个游戏的关键逻辑应该已经理清了**。

我们还可以给游戏添加有趣的新特性，比如道具系统、爆炸效果不同的炸弹、允许玩家推动炸弹、计分系统等，目前我实现了一部分新特性。

### 运行游戏

首先，我们需要一个 Pulsar 集群作为后端系统，且需要你和你的朋友连接同一套 Pulsar 集群才能一起游戏。

你可以在 Apache Pulsar 的官网查看文档自己搭建服务器部署一套：

[https://pulsar.apache.org/](https://pulsar.apache.org/)

也可以在 StreamNative Cloud 平台上建立一个免费 Pulsar 集群：

[https://console.streamnative.cloud/](https://console.streamnative.cloud/)

首先，集群需要用 OAuth 的方式连接认证，所以需要先在 Service Account 中新建一个秘钥，然后把秘钥文件下载到本地：

[![](https://labuladong.gitee.io/algo/images/%e7%82%b8%e5%bc%b9%e4%ba%ba/2.jpg)](https://labuladong.gitee.io/algo/images/%e7%82%b8%e5%bc%b9%e4%ba%ba/2.jpg)

然后新建一个免费的集群（注意免费集群拥有的资源很少，且一段时间后会自动回收集群资源）：

[![](https://labuladong.gitee.io/algo/images/%e7%82%b8%e5%bc%b9%e4%ba%ba/1.jpg)](https://labuladong.gitee.io/algo/images/%e7%82%b8%e5%bc%b9%e4%ba%ba/1.jpg)

新建了 Instance 之后可以查看 Pulsar Cluster 的信息，包括连接集群的地址：

[![](https://labuladong.gitee.io/algo/images/%e7%82%b8%e5%bc%b9%e4%ba%ba/3.jpg)](https://labuladong.gitee.io/algo/images/%e7%82%b8%e5%bc%b9%e4%ba%ba/3.jpg)

现在 Pulsar 集群就建好了，可以从我的仓库 clone 游戏代码：

[https://github.com/labuladong/pulsar-bomb-game](https://github.com/labuladong/pulsar-bomb-game)

下载依赖后修改 `main.go` 文件中的 `privateKeyPath` 为秘钥文件的路径，修改 `pulsarUrl` 为 Pulsar 集群的地址，最后运行程序 `go run *.go` 即可启动游戏。

多个玩家只要连接同一个集群并且输入相同的房间号，即可一起游戏：

[![](https://labuladong.gitee.io/algo/images/%e7%82%b8%e5%bc%b9%e4%ba%ba/%e6%b8%b8%e6%88%8f%e5%bd%95%e5%88%b6.gif)](https://labuladong.gitee.io/algo/images/%e7%82%b8%e5%bc%b9%e4%ba%ba/%e6%b8%b8%e6%88%8f%e5%bd%95%e5%88%b6.gif)

为了增加难度，我让地图里随机生成炸弹以提高难度，但如果玩家被炸死，还可以按 R 键复活继续游戏。

详细的代码实现可以看我的代码仓库，本文就到这里，主要带大家实操一下 Apache Pulsar 的使用，后续我还会分享更多消息系统相关的技术，敬请期待。

___

**引用本文的文章**

-   [拥抱开源，告别 CRUD](https://labuladong.gitee.io/algo/5/46/)

___

**＿＿＿＿＿＿＿＿＿＿＿＿＿**

**《labuladong 的算法小抄》已经出版，关注公众号查看详情；后台回复关键词「**进群**」可加入算法群；回复「**全家桶**」可下载配套 PDF 和刷题全家桶**：

[![](https://labuladong.gitee.io/algo/images/souyisou2.png)](https://labuladong.gitee.io/algo/images/souyisou2.png)