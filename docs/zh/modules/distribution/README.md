# `分布`

## 概述

这个 _simple_ 分发机制描述了一种被动的功能方式
在验证者和委托者之间分配奖励。请注意，此机制确实
不像积极的奖励分配机制那样精确地分配资金，并且
因此，将来会升级。

该机制如下操作。收集到的奖励在全球范围内汇集，
被动地分配给验证者和委托者。每个验证器都有
有机会就收集的奖励向委托人收取佣金
代表代表。费用直接收集到全球奖励池中
和验证者提议者奖励池。由于被动会计的性质，
每当影响奖励分配率的参数发生变化时
发生，也必须发生奖励的撤回。

- 每次提款时，必须提取他们有权提取的最高金额
   到，在池中什么也不留下。
- 每当绑定、解除绑定或将代币重新委托给现有帐户时，
   必须完全撤回奖励(作为懒惰会计的规则)
   改变)。
- 每当验证者选择更改奖励佣金时，所有累积
   佣金奖励必须同时提取。

`hooks.md` 中涵盖了上述场景。

此处概述的分发机制用于延迟分发
验证者和相关委托人之间的以下奖励:

- 社会分配的多代币费用
- 提议者奖励池
- 膨胀的原子规定
- 其委托人权益所获得的所有奖励的验证者佣金

费用集中在一个全局池中，以及特定于验证器的
提议者奖励池。所使用的机制允许验证者和委托者
独立和懒惰地收回他们的奖励。

## 缺点

作为惰性计算的一部分，每个委托人都持有一个累加项
特定于每个验证器，用于估计它们的近似值
全球费用池中持有的相当一部分代币归他们所有。

``
权利=委托人累积/所有委托人累积
``

在来料流量恒定且均等的情况下
每个区块奖励代币，这种分配机制将等于
主动分配(每个区块单独分配给所有委托人)。
然而，这是不现实的，因此与主动分布的偏差将
基于传入奖励代币的波动以及发生的时间
其他委托人的奖励提现。

如果您碰巧知道收到的奖励即将大幅增加，
您被激励在此事件之后才退出，从而增加
您现有的 _accum_ 的价值。见 [#2764](https://github.com/cosmos/cosmos-sdk/issues/2764)
了解更多详情。

## 对 Staking 的影响

对 Atom 条款收取佣金，同时也允许 Atom 条款
自动绑定(直接分配给验证者绑定的股份)是
BPoS 中存在问题。从根本上说，这两种机制是相互的
独家的。如果委托和自动绑定机制同时存在
应用于质押代币，然后质押代币在
任何验证者及其委托人都会随着每个区块的变化而变化。这然后
需要计算每个区块的每个委托记录 -
这被认为在计算上是昂贵的。

总之，我们只能有原子委托和未键合原子
没有 Atom 佣金的条款或键合原子条款，我们选择
实施前者。希望重新绑定其条款的利益相关者可以选择
设置脚本以定期提取和重新绑定奖励。

## 内容

1. **[概念](01_concepts.md)**
    - [F1费用分配中的引用计数](01_concepts.md#reference-counting-in-f1-fee-distribution)
2. **[状态](02_state.md)**
3. **[开始区块](03_begin_block.md)**
4. **[消息](04_messages.md)**
    - [MsgSetWithdrawAddress](04_messages.md#msgsetwithdrawaddress)
    - [MsgWithdrawDelegatorReward](04_messages.md#msgwithdrawdelegatorreward)
        - [全部提现验证者奖励](04_messages.md#withdraw-validator-rewards-all)
    - [常用计算](04_messages.md#common-calculations-)
5. **[钩子](05_hooks.md)**
    - [创建或修改委托分发](05_hooks.md#create-or-modify-delegation-distribution)
    - [佣金率变化](05_hooks.md#commission-rate-change)
    - [验证器状态的变化](05_hooks.md#change-in-validator-state)
6. **[事件](06_events.md)**
    - [BeginBlocker](06_events.md#beginblocker)
    - [处理程序](06_events.md#handlers)
7. **[参数](07_params.md)**
8. **[参数](07_params.md)**
    - [CLI](08_client.md#cli)
    - [gRPC](08_client.md#grpc) 