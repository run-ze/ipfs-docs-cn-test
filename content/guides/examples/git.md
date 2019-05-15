---
title: 更加分布式的 Git
---

你有没有对自己说过：“伙计，我的 git 服务器不够分布式”或“我期望有一个简单的方法来提供全世界可用的静态 git 仓库”。
不用再期望了，我为你找到了解决方法！

在本文中，我将讨论如何使用 ipfs 网络提供 git 仓库服务。
最终的结果将是一个 ipfs 支撑的可以使用 `git clone` 的地址！

首先，选择你想要托管的那个 git 仓库，然后做一个它的裸克隆：
```
$ git clone --bare git@myhost.io/myrepo
```

如果你不了解 git，一个裸仓库表示它没有工作目录、可以作为服务器。
它的格式与一般的 git 仓库略有不同。

现在，为了让它准备好被克隆，你需要做以下操作：
```
$ cd myrepo
$ git update-server-info
```

可选地，你可以解压所有 git 对象：
```
$ cp objects/pack/*.pack .
$ git unpack-objects < ./*.pack
$ rm ./*.pack
```

这样做会把 git 的大型打包文件分解为独立的对象。
如果你添加了这个存储库的多个版本，这会允许 ipfs 去除重复的对象。

当你做完后，那个仓库已经准备好提供服务了。
剩下要做的就是把它添加到 ipfs：
```
$ pwd
/code/myrepo
$ ipfs add -r .
...
...
...
added QmX679gmfyaRkKMvPA4WGNWXj9PtpvKWGPgtXaF18etC95 .
```

现在可以尝试克隆它了：
```
$ cd /tmp
$ git clone http://localhost:8080/ipfs/QmX679gmfyaRkKMvPA4WGNWXj9PtpvKWGPgtXaF18etC95 myrepo
```

注意：把那个散列改成你的。

现在，你可能会问：“如果一个 git 仓库不能更改，那么它有什么用处呢？”
让我告诉你一个很棒的用例！
我喜欢用一种叫 Go 的语言编程，如果你不了解它，go 使用版本控制路径进行导入，比如：
```go
import (
	"github.com/whyrusleeping/mycoollibrary"
)
```

这是一个很棒的特性，解决了很多问题。但有时我会遇到问题，我使用了某人的库，但是他们改变了 API，这就影响了我的代码。
使用我们上面做的，你可以克隆这个库，把它添加到 ipfs，这样你的导入路径看起来像下面这样：
```go
import (
	mylib "gateway.ipfs.io/ipfs/QmX679gmfyaRkKMvPA4WGNWXj9PtpvKWGPgtXaF18etC95"
)
```

这样你就能确保每次都获取到相同的代码！

Note: Since go doesn't allow the usage of localhost for import paths, we use the
public http gateways. This provides no security guarantees as a man in the
middle attack could ship you bad code. You could use a domain name that redirects
to the localhost instead to avoid the issue.
注意：因为 go 不允许使用 localhost 作为导入路径，所以我们使用了公共 http 网关。
这没有提供安全保证，因为可以通过中间人攻击可以发给你有问题的代码。
你可以使用重定向到 localhost 的域名来避免这个问题。

By [whyrusleeping](http://github.com/whyrusleeping)
