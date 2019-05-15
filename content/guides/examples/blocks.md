---
title: 操作分块
---

`ipfs add` 命令会用你指定的文件中的数据创建一个默克有向无环图（Merkle DAG）。 <!-- 后面还是用原名比较好，译名看不懂 -->
它在执行时遵循 [unixfs 数据格式](https://github.com/ipfs/go-unixfs/blob/master/pb/unixfs.proto)。
这意味着你的文件会被分割成块，然后使用链接节点（link nodes）把它们以类似于树的结构组合起来。
给定文件的散列其实是 DAG 的根节点（顶部节点）的散列。
你可以使用 `ipfs ls` 轻易地查看一个给定 DAG 的子块。

例如：
```
# 确保这个文件大于 256k
ipfs add alargefile
ipfs ls thathash
```

上面的命令应该打印出类似如下内容：
```
ipfs@earth ~> ipfs ls qms2hjwx8qejwm4nmwu7ze6ndam2sfums3x6idwz5myzbn
qmv8ndh7ageh9b24zngaextmuhj7aiuw3scc8hkczvjkww 7866189
qmuvjja4s4cgyqyppozttssquvgcv2n2v8mae3gnkrxmol 7866189
qmrgjmlhlddhvxuieveuuwkeci4ygx8z7ujunikzpfzjuk 7866189
qmrolalcquyo5vu5v8bvqmgjcpzow16wukq3s3vrll2tdk 7866189
qmwk51jygpchgwr3srdnmhyerheqd22qw3vvyamb3emhuw 5244129
```

这将显示文件的所有子块，以及它们及其子块在磁盘上的大小。

### 如何操作分块？
如果你喜欢探索，你可以从这些不同的块中得到很多不同的信息。
你可以使用子块的散列作为 `ipfs cat` 的输入，只查看给定子树（那个块和它的子块）的数据。
若要只查看给定块的数据而不查看其子块，使用 `ipfs block get`。
但是要小心，因为直接对某个块使用 `ipfs block get` 会在屏幕上打印出 DAG 结构的原始二进制数据。

`ipfs block stat` 会告诉你一个给定块（不包括它的子块）的实际大小，`ipfs refs` 会告诉你一个块的所有子块。
类似地，`ipfs ls` 或 `ipfs object links` 会显示所有子块及它们的大小。
要编写在给定对象的每个子块上运行的脚本，`ipfs refs` 是一个更合适的命令。

### 块 vs 对象
在 IPFS 中，块是由它的键（散列）标识的单个数据单元，
一个块可以是任何种类的数据，不一定要有任何类型的格式。
另一面，对象是一个遵循 Merkle DAG protobuf 数据格式的块，
可以用 `ipfs object` 解析和操作它。
任何给定的散列都可以表示一个对象或块。

### 从头创建一个块
创建自己的块很容易！简单地把你的数据放到一个文件中，然后运行 `ipfs block put <你的文件>`。
或者也可以把你的文件数据传递给 `ipfs block put`，像这样：

```
$ echo "This is some data" | ipfs block put
QmfQ5QAjvg4GtA3wg3adpnDJug8ktA1BxurVqBD8rtgVjM
$ ipfs block get QmfQ5QAjvg4GtA3wg3adpnDJug8ktA1BxurVqBD8rtgVjM
This is some data
```
注意：在创建你自己的块数据时，不能使用 `ipfs cat` 来读取它们。
这是因为输入的原始数据没有使用 unixfs 数据格式。
要读取这些原始块，使用 `ipfs block get`，如上所示。

By [whyrusleeping](http://github.com/whyrusleeping)
