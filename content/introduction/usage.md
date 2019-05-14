---
title: 基础使用
weight: 3
---

## 安装 IPFS

如果你还没这样做，那么你的第一步是**安装 IPFS**！大多数人喜欢安装预编译的软件包
——你可以在 [IPFS 发行页](https://dist.ipfs.io/#go-ipfs) 点击 “Download go-ipfs” <!-- 现在页面上是这样的 -->按钮
（我们用 Go 语言编写的参考实现），然后按照说明 [安装预编译软件包](../install/#安装预编译软件包)。

<a class="button button-primary" href="https://dist.ipfs.io/#go-ipfs" role="button">
  下载你平台的 IPFS&nbsp;&nbsp;<i class="fa fa-download" aria-hidden="true"></i>
</a>

想要其他选择，比如从 *源代码构建* ，或者遇到麻烦？检查 [安装页面](../install) 获得更多选项和疑难解答。
在这个教程中，如果你遇到任何问题或者卡住了，请随时在 [https://discuss.ipfs.io/](https://discuss.ipfs.io/)
或 [#ipfs on chat.freenode.net](irc://chat.freenode.net/%23ipfs) 寻求帮助。

## 初始化仓库

`ipfs` 把它所有的设置和内部数据保存在一个叫做 *仓库（repository）* 的目录下。在第一次使用 IPFS 之前，你需要使用 `ipfs init` 命令初始化仓库：

```sh
> ipfs init
initializing ipfs node at /Users/jbenet/.go-ipfs
generating 2048-bit RSA keypair...done
peer identity: Qmcpo2iLBikrdf1d6QU6vXuNb6P7hwrbNPW9kLAH8eG67z
to get started, enter:

  ipfs cat /ipfs/QmYwAPJzv5CZsnA625s3Xf2nemtYgPpHdWEz79ojWnPbdG/readme

```

<div class="alert alert-warning">
    <p>
        如果你在数据中心的服务器上运行，应该使用服务器的配置文件初始化 IPFS。这会阻止 IPFS 为了发现本地节点而产生大量数据中心内部的流量：
    </p>

    <pre><code class="language-sh">&gt; ipfs init --profile server</code></pre>

    <p>
        还有一大堆其他你可能想要修改的选项——查看 <a href="https://github.com/ipfs/go-ipfs/blob/v0.4.15/docs/config.md">参考列表</a> 了解更多。
    </p>
</div>

<div class="alert alert-info">
    <code>peer identity: </code> 之后的散列是你的节点的 ID，会与上面的不一样。网络中的其它节点用它来寻找和连接你。
    你可以在需要时运行 <code>ipfs id</code> 来再次查看它。
</div>

现在，试着运行 ipfs init 输出中建议的命令，他们看起来像是 `ipfs cat /ipfs/<散列>/readme`。

你会看到这样的输出：

```
Hello and Welcome to IPFS!

██╗██████╗ ███████╗███████╗
██║██╔══██╗██╔════╝██╔════╝
██║██████╔╝█████╗  ███████╗
██║██╔═══╝ ██╔══╝  ╚════██║
██║██║     ██║     ███████║
╚═╝╚═╝     ╚═╝     ╚══════╝

If you're seeing this, you have successfully installed
IPFS and are now interfacing with the ipfs merkledag!

 -------------------------------------------------------
| Warning:                                              |
|   This is alpha software. use at your own discretion! |
|   Much is missing or lacking polish. There are bugs.  |
|   Not yet secure. Read the security notes for more.   |
 -------------------------------------------------------

Check out some of the other files in this directory:

  ./about
  ./help
  ./quick-start     <-- usage examples
  ./readme          <-- this file
  ./security-notes

```

你可以探索里面的其它条目，尤其是 `quick-start`：


```sh
ipfs cat /ipfs/QmYwAPJzv5CZsnA625s3Xf2nemtYgPpHdWEz79ojWnPbdG/quick-start
```

Which will walk you through several interesting examples.

## 启动

当你准备好启动时，在另一个终端中运行守护程序：

```sh
> ipfs daemon
Initializing daemon...
API server listening on /ip4/127.0.0.1/tcp/5001
Gateway server listening on /ip4/127.0.0.1/tcp/8080
```

等到这三行都出现。

<div class="alert alert-info">
注意你得到的 tcp 端口号，如果他们和示例不一样，在下面的命令里使用你获得的。
</div>

现在，回到之前的终端。如果你已经连接到网络，你应该能在运行下面命令后看到你连接的节点的 ipfs 地址：

```sh
> ipfs swarm peers
/ip4/104.131.131.82/tcp/4001/ipfs/QmaCpDMGvV2BGHeYERUEnRQAwe3N8SzbUtfsmvsqQLuvuJ
/ip4/104.236.151.122/tcp/4001/ipfs/QmSoLju6m7xTh3DuokvT3886QRYqxAzb1kShaanJgW36yx
/ip4/134.121.64.93/tcp/1035/ipfs/QmWHyrPWQnsz1wxHR219ooJDYTvxJPyZuDUPSDpdsAovN5
/ip4/178.62.8.190/tcp/4002/ipfs/QmdXzZ25cyzSF99csCQmmPZ1NTbWTe8qtKFaZKpZQPdTFB
```

它们是一组 `<传输地址>/ipfs/<公钥的散列>`。

现在你应该能从网络中获取对象。试试：

```
ipfs cat /ipfs/QmW2WQi7j6c7UgJTarActp7tDNikE4B2qXtFCfLPdsgaTQ/cat.jpg >cat.jpg
open cat.jpg
```

而且你应该能向网络传递对象。试着添加一个，然后在你喜欢的浏览器里查看它。
在这个例子里，我们拿 curl 当浏览器，但你也可以在浏览器里打开这个 IPFS 地址：

```
> hash=`echo "I <3 IPFS -$(whoami)" | ipfs add -q`
> curl "https://ipfs.io/ipfs/$hash"
I <3 IPFS -<你的用户名>
```

很酷，是吧？那个网关从 _你的电脑_ 提供了一个文件。那个网关查询了 DHT，找到了你的机器，请求了那个文件，
你的机器把文件发给网关，然后网关把文件发给你的浏览器。

<div class="alert alert-warning">
    注意：取决于网络状态，`curl` 可能会花一些时间。那个公共网关也可能过载或者难以连接到你。
</div>

你也可以在自己的本地网关尝试：

```
> curl "http://127.0.0.1:8080/ipfs/$hash"
I <3 IPFS -<你的用户名>
```

你的网关默认不向外部世界开放，它只对本地工作。

## 优秀的网页控制台

我们也有一个网页控制台，可以用来检查你的节点的状态。打开你喜欢的浏览器，前往：

<pre><code><a href="http://localhost:5001/webui">http://localhost:5001/webui</a></code></pre>

它会显示一个像这样的控制台：

<figure>
    <img class="screenshot" alt="Web console connection view" src="../assets/webui-connection.png">
</figure>

## 浏览器伴侣插件

此外，[IPFS 伴侣](https://github.com/ipfs-shipyard/ipfs-companion#ipfs-companion)
是一个简化对 IPFS 资源的访问、添加 IPFS 协议支持的浏览器插件。

<div class="alert alert-info">
它会自动重定向对 IPFS 网关的请求到你本地的守护程序，这样你就无需依赖，或者说信任远程网关。
</div>

他可以在 Firefox（桌面版和安卓版）和许多像 Google Chrome 或 Brave 这样基于 Chromium 的浏览器中运行。
查看 [它的功能](https://github.com/ipfs-shipyard/ipfs-companion#features)
并且立即 [**安装它**](https://github.com/ipfs-shipyard/ipfs-companion#install)！

| <img src="../assets/firefox_16x16.png" widgth="16" height="16"> [Firefox](https://www.mozilla.org/firefox/new/) / <img src="../assets/firefox_16x16.png" widgth="16" height="16"> [Firefox for Android](https://play.google.com/store/apps/details?id=org.mozilla.firefox) | <img src="../assets/chrome_16x16.png" width="16" height="16"> [Chrome](https://www.google.com/chrome/) / <img src="../assets/brave_16x16.png" width="16" height="16"> [Brave](https://brave.com/)
|------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [![Install From Firefox Add-ons](../assets/get-the-firefox-add-on.png)](https://addons.mozilla.org/firefox/addon/ipfs-companion/) | [![Install from Chrome Store](../assets/chrome-web-store.png)](https://chrome.google.com/webstore/detail/ipfs-companion/nibjojkomfdiaoajekhjakgkdhaomnch) |


现在，你已经准备好：

<a class="button button-primary" href="{{< ref "/guides/examples" >}}" role="button">
  查看更多例子&nbsp;&nbsp;<i class="fa fa-arrow-right"></i>
</a>
