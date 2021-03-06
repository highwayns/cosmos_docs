# 特定于应用程序的区块链

本文档解释了什么是特定于应用程序的区块链，以及为什么开发人员想要构建一个而不是编写智能合约。 {概要}

## 什么是特定于应用程序的区块链？

特定于应用程序的区块链是为操作单个应用程序而定制的区块链。 开发人员不是在像以太坊这样的底层区块链之上构建分散式应用程序，而是从头开始构建自己的区块链。 这意味着构建一个全节点客户端、一个轻客户端，以及与节点交互的所有必要接口(CLI、REST 等)。

```
                ^  +-------------------------------+  ^
                |  |                               |  |   Built with Cosmos SDK
                |  |  State-machine = Application  |  |
                |  |                               |  v
                |  +-------------------------------+
                |  |                               |  ^
Blockchain node |  |           Consensus           |  |
                |  |                               |  |
                |  +-------------------------------+  |   Tendermint Core
                |  |                               |  |
                |  |           Networking          |  |
                |  |                               |  |
                v  +-------------------------------+  v
```

## 智能合约的缺点是什么？

早在 2014 年，以太坊等虚拟机区块链就满足了对更多可编程性的需求。当时，可用于构建去中心化应用程序的选项非常有限。大多数开发人员会建立在复杂且有限的比特币脚本语言之上，或者分叉难以使用和定制的比特币代码库。

虚拟机区块链带来了新的价值主张。他们的状态机包含一个虚拟机，能够解释称为智能合约的图灵完备程序。这些智能合约非常适合一次性事件(例如 ICO)等用例，但它们可能无法构建复杂的去中心化平台。原因如下:

- 智能合约通常是用特定的编程语言开发的，可以被底层虚拟机解释。这些编程语言通常不成熟，并且本质上受到虚拟机本身的限制。例如，以太坊虚拟机不允许开发者实现代码的自动执行。开发者也受限于 EVM 的基于账户的系统，他们只能从有限的一组功能中进行选择以进行加密操作。这些只是示例，但它们暗示智能合约环境通常缺乏**灵活性**。
- 智能合约都由同一个虚拟机运行。这意味着它们在争夺资源，这会严重制约**性能**。即使状态机被分成多个子集(例如通过分片)，智能合约仍然需要由虚拟机进行交互，与在状态机级别实现的本机应用程序相比，这将限制性能(我们的基准测试显示，删除虚拟机后，性能提高了 10 倍)。
- 智能合约共享相同底层环境的另一个问题是由此导致的**主权**限制。去中心化应用程序是一个涉及多个参与者的生态系统。如果应用程序建立在通用虚拟机区块链上，利益相关者对其应用程序的主权非常有限，最终会被底层区块链的治理所取代。如果应用程序中存在错误，则几乎无能为力。

特定于应用程序的区块链旨在解决这些缺点。

## 特定于应用程序的区块链的好处

### 灵活性

特定于应用程序的区块链为开发人员提供了最大的灵活性:

- 在 Cosmos 区块链中，状态机通常通过称为 [ABCI](https://docs.tendermint.com/v0.34/spec/abci/) 的接口连接到底层共识引擎。这个接口可以用任何编程语言包装，这意味着开发人员可以用他们选择的编程语言构建他们的状态机。

- 开发人员可以在多个框架中进行选择来构建他们的状态机。目前使用最广泛的是 Cosmos SDK，但也有其他的(例如 [Lotion](https://github.com/nomic-io/lotion)、[Weave](https://github.com/iov-one/编织)，...)。通常，将根据他们想要使用的编程语言做出选择(Cosmos SDK 和 Weave 使用 Golang，Lotion 使用 Javascript，......)。
- ABCI 还允许开发人员交换其特定于应用程序的区块链的共识引擎。今天，只有 Tendermint 可以投入生产，但未来预计会出现其他共识引擎。
- 即使他们满足于框架和共识引擎，如果他们的原始形式不能完全符合他们的要求，开发人员仍然可以自由地调整它们。
- 开发人员可以自由探索各种权衡(例如，验证器数量与交易吞吐量、安全性与异步可用性等)和设计选择(用于存储的 DB 或 IAVL 树、UTXO 或帐户模型等) .
- 开发者可以实现代码的自动执行。在 Cosmos SDK 中，可以在每个块的开头和结尾自动触发逻辑。他们还可以自由选择在他们的应用程序中使用的加密库，而不是在虚拟机区块链的情况下受到底层环境提供的内容的限制。

上面的列表包含一些示例，显示了特定于应用程序的区块链为开发人员提供了多少灵活性。 Cosmos 和 Cosmos SDK 的目标是使开发人员工具尽可能通用和可组合，以便堆栈的每个部分都可以在不失去兼容性的情况下进行分叉、调整和改进。随着社区的发展，每个核心构建块的更多替代方案将会出现，为开发人员提供更多选择。

### 表现 

使用智能合约构建的去中心化应用程序本质上受到底层环境的性能限制。对于去中心化应用程序优化性能，它需要构建为特定于应用程序的区块链。接下来是特定于应用程序的区块链在性能方面带来的一些好处:

- 特定应用区块链的开发人员可以选择使用一种新颖的共识引擎，例如 Tendermint BFT。与工作量证明(当今大多数虚拟机区块链使用)相比，它在吞吐量方面提供了显着的收益。
- 特定于应用程序的区块链仅运行单个应用程序，因此该应用程序不会与其他应用程序竞争计算和存储。这与当今大多数非分片虚拟机区块链相反，智能合约都在竞争计算和存储。
- 即使虚拟机区块链提供基于应用程序的分片以及高效的共识算法，性能仍然会受到虚拟机本身的限制。真正的吞吐量瓶颈是状态机，并且需要由虚拟机解释交易显着增加了处理它们的计算复杂性。

### 安全

安全性难以量化，并且因平台而异。也就是说，特定于应用程序的区块链在安全性方面可以带来一些重要的好处:

- 开发人员可以在构建特定于应用程序的区块链时选择 Golang 等经过验证的编程语言，而不是通常不成熟的智能合约编程语言。
- 开发人员不受底层虚拟机提供的加密功能的限制。他们可以使用自己的自定义密码学，并依赖经过充分审核的密码库。
- 开发人员不必担心底层虚拟机中的潜在错误或可利用机制，从而更容易推理应用程序的安全性。

### 主权

特定于应用程序的区块链的主要好处之一是主权。去中心化应用程序是一个涉及许多参与者的生态系统:用户、开发人员、第三方服务等。当开发者建立在许多去中心化应用并存的虚拟机区块链上时，应用的社区不同于底层区块链的社区，后者在治理过程中取代了前者。如果存在错误或需要新功能，则应用程序的利益相关者几乎没有升级代码的余地。如果底层区块链的社区拒绝采取行动，则什么都不会发生。

这里的根本问题是应用程序的治理和网络的治理不一致​​。这个问题由特定于应用程序的区块链解决。由于特定于应用程序的区块链专门运行单个应用程序，因此应用程序的利益相关者可以完全控制整个链。这确保了社区在发现错误时不会被卡住，并且可以自由选择如何发展。

## 下一个 {hide}

详细了解 Cosmos SDK 应用程序的 [高级架构](./sdk-app-architecture.md) {hide} 