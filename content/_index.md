---
type: index
title: IPFS 文档
---

欢迎来到 IPFS 文档入口！无论你是刚学习 IPFS 还是正在寻找详细的参考信息，都可以从这里开始。你可能注意到 IPFS 是一个范围很广的项目——包含 *许多* 不同的工具、网站和代码。

## 介绍

前往[介绍](/introduction)部分以获取 IPFS 的基础知识和安装、运行指南。


## 指南

指南部分概述了 IPFS 的主要概念、指南及演示 IPFS 多种用途的示例项目。

特别是 IPFS 是一个复杂的系统，旨在改变我们使用互联网的方式，所以它自然带来了许多新的概念。查看[概念](/guides/concepts)部分可以了解更多关于 IPFS 的主要架构以及与分布式文件系统相关的术语和概念。

如果你更喜欢动手操作，可以试试 [ProtoSchool](https://proto.school) 的交互式教程，在那里你可以通过解决代码挑战来了解分布式网络。


## 参考

### 命令与 API

如果你通过[命令行](/reference/api/cli)使用 IPFS 或以编程方式[与正在运行的 IPFS 节点交互](/reference/api/http)，那么你将使用 IPFS 的命令行 API。IPFS 的 Go 和 JavaScript 版本都实现了它。


### Go 和 JavaScript 实现

IPFS 基本上是一组分布式文件系统的通信协议，但是这些协议是通过 [Go](/reference/go/overview) 和 [JavaScript](/reference/js/overview) 的参考实现来实现的。Go 实现更加成熟，实现了更多的 IPFS 协议，但 JavaScript 实现可以用于更广泛的需求（包括在浏览器中）。


### 规范与计划

虽然 IPFS 有两个参考实现（使用 Go 和 JavaScript），但它基本上是一组分布式文件系统的格式和通信协议。你可以在“规范与计划”部分中找到这些协议、白皮书和有关 RFC（请求更改）过程的规范。


## 社区

与 IPFS 社区的其他成员保持联系，他们正在 IPFS 之上构建工具，甚至帮助构建 IPFS 本身！你可以提问题、讨论新想法、或者在 [https://discuss.ipfs.io](https://discuss.ipfs.io) 得到帮助，也可以在 [IRC](/community/irc) 上聊天。

会议、活动、人们正在构建的应用程序等相关信息请参见社区部分的其他链接。

这里还有为 IPFS 及社区中的其他软件项目做贡献的信息。

### 应用

IPFS 的 Go 和 JavaScript 实现都是同时作为库和功能相对有限的命令行程序编写。我们正在开发利用 IPFS 的各种其它应用程序，比如 GUI 应用、浏览器扩展以及用于管理大型数据档案的集群工具。你可以在这里找到更多相关信息。

### 参与进来

IPFS 是一个开源的社区项目。虽然 [Protocol Labs](https://protocol.ai) 能够支持一些相关工作，但是大部分的设计、代码和成就都是由志愿者和像你这样的社区成员贡献的。如果你有兴趣帮助改进 IPFS，请从[如何帮助](/community/contribute/how_to_help)指南开始。

如果你打算贡献新代码，请先阅读[贡献指南](https://github.com/ipfs/community/blob/master/CONTRIBUTING.md)和你用的语言的风格指南（[Go](https://github.com/ipfs/community/blob/master/CONTRIBUTING_GO.md)，[JavaScript](https://github.com/ipfs/community/blob/master/CONTRIBUTING_JS.md)）。

### 相关项目

随着时间的推移，我们已经将 IPFS 的一些主要部分分割成单独的项目——它们虽然仍旧是 IPFS 的关键组成部分，但也可以在其它各种情况下发挥作用。检查它们的独立网站以获取具体的信息和参考：

- [Libp2p](https://libp2p.io) 负责 IPFS 的点对点网络连接。
- [Multiformats](https://multiformats.io) 是一套 *自我描述* 的数据格式。
- [IPLD](https://ipld.io) 是一组用于描述像 IPFS 文件、Git 提交、以太坊区块这样的内容寻址数据之间链接的工具。
