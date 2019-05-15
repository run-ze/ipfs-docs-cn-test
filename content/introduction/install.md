---
title: 安装 IPFS
weight: 2
---

<!--
基于现存的安装文档 https://github.com/ipfs/website/blob/714fa4f3fc469d81b94dc190f1335b9556ad90e1/content/docs/install.md

Note there are pending PRs for that document that will need to be included here:
- https://github.com/ipfs/website/pull/260 / https://github.com/ipfs/website/pull/228
- https://github.com/ipfs/website/pull/258

-->

有许多把 IPFS 安装到你的系统的方法。我们一般推荐 [安装预编译软件包](#安装预编译软件包)，但还有一些其他支持的选择：

* [安装预编译软件包](#安装预编译软件包)（推荐）
* [使用 ipfs-update 安装](#使用-ipfs-update-安装)
* [从源代码构建](#从源代码构建)
* [升级 IPFS](#升级-ipfs)
* [疑难解答](#疑难解答)

请注意这些说明使用**命令行**。我们用 `$` 表示命令提示符—以它开头的行表示需要输入的命令，不以它开头则表示命令的输出。

---

## 安装预编译软件包

首先，下载你的平台对应版本的 IPFS：

<a class="button button-primary" href="https://dist.ipfs.io/#go-ipfs" role="button">
  下载你平台的 IPFS&nbsp;&nbsp;<i class="fa fa-download" aria-hidden="true"></i>
</a>

### Mac OS X 和 Linux

<!-- macOS 和 Linux 平台的 install.sh 好像都只会移动到 /usr/local/bin 或 /usr/bin，并不会检查 $PATH -->
下载完成之后，解压档案，然后用 `install.sh` 把 `ipfs` 可执行文件移动到 `$PATH` 环境变量里的路径下：

```sh
$ tar xvfz go-ipfs.tar.gz
$ cd go-ipfs
$ ./install.sh
```

测试一下：

```sh
$ ipfs help
USAGE:

    ipfs - Global p2p merkle-dag filesystem.
...
```

恭喜！你的电脑已经安装好 IPFS 了。

<a class="button button-primary" href="{{< relref "usage.md" >}}" role="button">
  日常使用&nbsp;&nbsp;<i class="fa fa-arrow-right"></i>
</a>

### Windows

下载完之后，解压档案，然后把 `ipfs.exe` 移动到 `%PATH%` 环境变量里的路径下。

测试一下：

```sh
$ ipfs help
USAGE:

    ipfs - Global p2p merkle-dag filesystem.
...
```

恭喜！你的电脑已经安装好 IPFS 了。

<a class="button button-primary" href="{{< relref "usage.md" >}}" role="button">
  日常使用&nbsp;&nbsp;<i class="fa fa-arrow-right"></i>
</a>


---

## 使用 ipfs-update 安装

`ipfs-update` 是一个用于安装和升级 `ipfs` 二进制文件的命令行工具。

### 获取 ipfs-update

可以从 https://dist.ipfs.io/#ipfs-update 下载你平台的 `ipfs-update`。

如果你有可用的 Go 语言环境（>=1.8），你也可以这样安装它：
```
$ go get -u github.com/ipfs/ipfs-update
```

在安装新版本 `ipfs` 或升级前确保你在使用最新版本的 `ipfs-update`。

### 使用 ipfs-update 安装 ipfs

`ipfs-update versions` 会显示所有可以下载的 `ipfs` 版本：

```
$ ipfs-update versions
v0.3.2
v0.3.4
v0.3.5
v0.3.6
v0.3.7
v0.3.8
v0.3.9
v0.3.10
v0.3.11
v0.4.0
v0.4.1
v0.4.2
v0.4.3
v0.4.4
v0.4.5
v0.4.6
v0.4.7-rc1
```


`ipfs-update install latest` 会安装可用的最新版本：

```
$ ipfs-update install latest
fetching go-ipfs version v0.4.7-rc1
binary downloaded, verifying...
success!
stashing old binary
installing new binary to /home/hector/go/bin/ipfs
checking if repo migration is needed...
Installation complete!
```

请留意可用的最新版本可能不是稳定版（比如：发布候选版会显示为 `vX.X.X-rcX`）。
所以建议指明你想要安装的版本，例如 `ipfs-update install v0.4.6`。

---

## 从源代码构建

<div class="message mb">
  <strong>警告：</strong> 过去你可以使用 <code>go get</code> 安装 IPFS。现在不能再这样了！
</div>

如果你想，可以从源代码构建 IPFS。
如果你使用 Mac OS X 或 Linux，安装介绍在 [这个说明](https://github.com/ipfs/go-ipfs#build-from-source)。
如果你使用 Windows，看一下 [这个文档](https://github.com/ipfs/go-ipfs/blob/master/docs/windows.md)。

---

## 升级 IPFS

`ipfs` 的升级（还有降级）可能需要使用 [fs-repo-migrations](https://dist.ipfs.io/#fs-repo-migrations) 工具进行仓库的升级。

### 使用 ipfs-update 升级

在安装新版或旧版 `ipfs` 过程中（如上所述），`ipfs-update install` 会在需要时下载并运行 `fs-repo-migrations`。
这是最简单的升级方法。

<div class="message mb">
  <strong>警告：</strong> 确保升级时 ipfs 守护程序没在运行。
</div>


### 手动升级

要手动升级 `ipfs`，你需要手动执行仓库迁移。步骤如下：

* 停止 `ipfs` 守护程序，如果它正在运行
* 可以备份你的 `ipfs` 数据目录（例如：`cp -aL ~/.ipfs ~/.ipfs.bk`）
* 从 [https://dist.ipfs.io/#go-ipfs](https://dist.ipfs.io/#go-ipfs) 下载并安装最新版 `ipfs`
* 运行 `ipfs daemon`。

如果需要迁移仓库，`ipfs` 会提示用户，下载并安装 `fs-repo-migrations` 并进行升级。
如果你想要自动执行这个流程，在启动守护程序时附加 `--migrate` 参数。

也可以从 [https://dist.ipfs.io/#fs-repo-migrations](https://dist.ipfs.io/#fs-repo-migrations)
下载最新版 `fs-repo-migrations` 并
[遵照这些说明](https://github.com/ipfs/fs-repo-migrations/blob/master/run.md) 来手动执行迁移。

---

## 疑难解答

### 帮帮我！

如果你遇到任何问题，可以通过 [#ipfs](/#community)
或 [邮件列表](/#community) 得到帮助。

### 检查 Go 语言版本

IPFS 可以在 Go 1.7.0 或以上版本运行。
要检查你安装的 Go 版本，执行 `go version`。
我得到的是：

```sh
$ go version
go version go1.7 linux/amd64
```

如果你需要升级，建议安装 [官方 Go 安装包](https://golang.org/doc/install)。
软件包管理器附带的 Go 版本往往是过时的。

### 安装 FUSE

要了解更多安装 FUSE （这样你就可以把 IPFS 作为文件系统挂载了）的说明，看看 [github.com/ipfs/go-ipfs/blob/master/docs/fuse.md](https://github.com/ipfs/go-ipfs/blob/master/docs/fuse.md)。
