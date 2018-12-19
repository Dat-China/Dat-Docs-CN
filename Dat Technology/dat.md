[dat_clone_img]: https://raw.githubusercontent.com/datproject/docs/master/assets/cli-clone.gif
[dat_share_img]: https://raw.githubusercontent.com/datproject/docs/master/assets/cli-share.gif
[build_failing]: https://travis-ci.org/datproject/dat.svg?branch=master
[build_passing]: https://camo.githubusercontent.com/f5d09f0805c9a618d57e87fe683f281ad0371f16/68747470733a2f2f63692e6170707665796f722e636f6d2f6170692f70726f6a656374732f7374617475732f6769746875622f64617470726f6a6563742f6461743f6272616e63683d6d6173746572267376673d74727565
[mac_build]: https://travis-ci.org/datproject/dat
[windows_build]: https://ci.appveyor.com/project/joehand/dat/branch/master
[view_demo_online]: https://datbase.org/778f8d955175c92e4ced5e4f5563f69bfec0c86cc6f670352c457943666fe639
[datbase.org]: https://datbase.org
[registry_server]: https://github.com/datproject/datbase
[dat_keys_import_issue]: https://github.com/datproject/dat/issues/983
[open_an_issue]: https://github.com/datproject/dat/issues/new
[chat_room]: https://gitter.im/datproject/discussions

# Dat

    npm install -g dat
    
一个分布式的数据社区。Dat是一个非盈利组织支持的社区，也是一个创造未来的开放协议。

使用Dat命令行工具在分享文件的同时完成版本控制、将数据备份到服务器、按需远程浏览文件、并自动对数据进行长期保存。

如果还有问题请通过IRC或Gitter加入我们：

[![irc channel # dat on free node](https://img.shields.io/badge/irc%20channel-%23dat%20on%20freenode-blue.svg)](https://webchat.freenode.net/?channels=dat)
[![gitter](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/datproject/discussions)

## 目录

* [安装][installation]
* [开始][getting_started]
* [使用Dat][using_dat]
* [疑难解答][troubleshooting]
* [JavaScript API][javascript_api]
* [致开发者][for_developers]

## 安装

Dat可以被用作命令行工具或JavaScript库：

* 在命令行中安装dat
* [require('dat')][require_dat] - dat-node,一个在JavaScript应用中分享和下载dat archive的库。

### 在命令行中安装`$ dat`

我们推荐在命令行中使用npm安装Dat：

    npm intall -g dat
    
首先，你要确保你已经安装了node和npm。如果米有安装的话，请参考下面的准备环节。我们推荐使用npm，因为这样可以在dat发布新版本的时候及时获取最新版本。

当你使用npm安装dat后，你应该可以运行`$ dat`命令了。如果不能运行的话，你可以到疑难解答中找一下解决方案。

### 其他安装方法

如果你没办法使用npm安装dat，你也可以下载一个二进制文件，然后利用下面的一行命令进行安装。二进制文件包含一个node和一个打包好的dat，所以你只需要下载一个文件就可以了，不需要在你的系统上配置其他依赖。
    
    wget -qO- https://raw.githubusercontent.com/datproject/dat/master/download.sh | bash

### NPM 准备环节

* **Node:**在你安装Dat之前，你需要先安装Nodejs。Dat需要4.0及以上版本的Node来运行并且需要使用npm来安装。你可以运行`node -v`来查看你的版本。

* **npm:**npm会自动随着node一起安装。你可以运行一下`npm -v`来检查它是否已经安装好了。

当你安装好npm以后，使用`npm install -g dat`来安装dat。

# #开始

###什么是Dat？
分享、备份并发布你的文件。你可以将你电脑上任何文件夹变成一个dat。Dat会扫描你的文件夹，支持你进行以下操作：
* 使用自动的版本历史系统对追溯你的文件。
* 通过一个安全的p2p网络与他人分享文件。
* 自动地将文件实时备份到远程服务器或硬盘。
* 通过内置的HTTP服务器分享、发布文件。

Dat让你可以专心工作而不必担心移动文件这种琐事。  
**安全， 分布式， 高速**

* 文档：[documentation][documentation]
* [DatBase][datbase]
* [Dat white paper][dat_white_paper]

### 其他应用

不用命令行？来看看这些选项吧：

* [Dat Desktop][dat_desktop] - 一个用来管理dat的桌面应用。
* [Breaker Browser][breeak_browser] - 一个实验性的浏览器，内置了对dat协议的支持。


## Dat命令行
使用命令行分享、备份、下载文件！自动将变更同步到数据集。再也不用手动传文件了。

|Mac/Linux|Windows|Version|
|---|---|---|
|[![][build_failing]][mac_build]|[![][build_passing]][windows_build]|

### JS库

将Dat添加到你的`packagee.jsson`中，运行`npm install dat --save`。Dat通过`require('dat')`导出Dat的api。
在你的JavaScript应用中使用它吧！Dat桌面应用和Dat命令行工具都是使用了dat-node分享和下载dat。

完整的API文档在Github上 dat-node 的仓库中。

我们已经安装好了Dat，下面我们使用一下！

Dat的独特的设计保证无论你在哪里存放数据都适用。你你可以在你的电脑上任何文件夹创建dat。

一个dat是由两部分组成：

1. 原来你电脑上的文件
2. 一个.dat文件夹

每个dat都有一个独一无二的dat链接，拥有了你的dat链接别的用户就可以从你的dat下载或同步文件了。

### 分享数据

用一条命令你就可以分享文件。不像`git`，你不必先初始化你的仓库，`dat share`会帮你完成初始化工作的。

    dat share <dir>
    
使用`dat share`来创建一个dat，并将你的文件从你的电脑同步到其他电脑。Dat会扫描\<dir>目录中的文件并且在\<dir>/.dat文件夹中创建一个元数据文件。Dat将会在dat文件夹中存放链接、版本历史和文件信息。

![dat share][dat_share_img]

###  下载数据

    dat clone dat://<link> <download-dir>

使用`dat clone`命令可以从远程计算机上下载文件。这将会从`dat://<link>`下载文件到你的\<download-dir>文件夹中。下载会在完成时退出，但是在clone过程结束后你还可以继续更新你的文件。使用`dat pull`来更新新的文件或使用`dat ync`来实时同步变更。

![dat clone][dat_clone_img]

### 其他炫酷的命令

* `dat create` - 创建一个空的dat和一个dat.json文件。
* `dat doctor` - Dat的网络医生！doctor可以尝试连接一个公共节点，也可以创建一个秘钥来测试直接连接。
* `dat log ~/data/dat-folder/` 或者 `dat log dat://<key>` - 查看一个dat的历史与元数据信息。

### 快速示例

为了学习使用Dat，你可以先下载一个dat然后再分享一个你自己的dat。

### 下载示例
我们创建了一个文件夹用于练习，在这个文件夹中是一个dat.json文件和一个gif图片。我们已经通过Dat将这个文件夹分享了，你可以通过我们的dat秘钥访问它。

    ❯ dat clone dat://778f8d955175c92e4ced5e4f5563f69bfec0c86cc6f670352c457943666fe639 ~/Downloads/dat-demo
    dat v13.5.0
    Created new dat in /Users/joe/Downloads/dat-demo/.dat
    Cloning: 2 files (1.4 MB)
    
    2 connections | Download 614 KB/s Upload 0 B/s
    
    dat sync complete.
    Version 4`
    
这些文件由一台服务器通过Dat分享，但实际你可能会同时连接到很多其他也正在托管该内容的用户。

你也可以在线查看这些文件：  
[datbase.org/778f8d955175c92e4ced5e4f5563f69bfec0c86cc6f670352c457943666fe639][view_demo_online]   
可以在datbase.org在线查看有人正在托管的dat。这个网站会缓存被访问链接的数据，如果你不希望你的数据被缓存的话，请不要再datbase.org行查看你的dat

### 分享示例
Dat可以将文件从你的电脑分享到任何地方。如果你是和小伙伴一起看这个教程的话，你们可以试着互相分享。

在你的电脑上找到一个要分享的文件夹。文件夹里面可以是任何东西，Dat适用于任何文件！（特别大的文件也适用！）

首先，你要在那个文件夹中创建一个新的dat。使用`dat createe`命令还会引导我们创建一个dat.json文件。

    ❯ dat create
    Welcome to dat program!
    You can turn any folder on your computer into a Dat.
    A Dat is a folder with some magic.

这样就可以创建一个新的（空的）dat了，dat的链接将会被输出在控制台，把这个链接分享给其他人吧。

一旦我们创建好了dat，运行`dat share`，Dat将会扫描你的文件并将他们分享到网络上。把链接分享给你的朋友，即刻开始文件下载！

你还可以尝试一下在线查看你的文件。访问[datbase.org][datbase.org],然后在网页的左上角输入链接即可查看。（可能有些用户还无法连接到datbase.org，别担心，我们正在解决这个问题。）

### HTTP示例

Dat使得HTTP服务器分享文件变得很简单。下面的演示就很酷了，因为我们即将看到版本历史系统是如何运作的。通过添加`--http`选项来使用HTTP分享daat。比如这样`dat sync --http`就可以将你的文件分享到HTTP站点并且支持实时热重载与版本历史更新。

提示： 你可以访问`localhost:8080/?version=10`来查看特定的版本。

赶快开始使用Dat分享和下载文件吧，或者你也可以继续阅读了解更多细节。

## 用法

你第一次运行命令的时候，Dat会创建一个.dat文件夹来储存元数据。在dat创建好之后你就可以像使用git一样在那个文件夹里面运行各种命令。

Dat将秘钥保存在`~/.dat/secret_keys`文件夹中。

### 创建一个dat & dat.json

```commandline
dat create [<dir>]
```

当你创建新的dat时，`create`命令会引导你创建一个dat.json文件。

你可以通过`sync`或`share`命令来导入文件

你可以只填写 title 和 description 
    dat create --title "MY BITS" -- description "are ready to synchronize!😎"

还可以跳过dat.json
```commandline
dat create --yes
dat create -y
```

### 分享

最快的分享文件的方法就是用`share`命令：
```commandline
❯ dat share
dat://3e830227b4b2be197679ff1b573cc85e689f202c0884eb8bdb0e1fcecbd93119
Sharing dat: 24 files (383 MB)

0 connections | Download 0 B/s Upload 0 B/s

Importing 528 files to Archive (165 MB/s)
[=-----------------------------------------] 3%
ADD: data/expn_cd.csv (403 MB / 920 MB)
```

### 同步到网络

    dat sync [<dir>] [--no-import] [--no-watch]
    
开始把你的dat存档分享到网上吧。在你上一次运行`create`或`sync`之后，Sync将会导入新的文件或更新。Sync会监视文件的更改并且导入更新。

使用 `--no-import` 参数便不会导入任何更新
使用 `--no-watch` 参数便不会监视文件的变更。如果想要`--watch`选项正常工作，必须要设置`--import`。

### 忽略文件

默认情况下，Dat会忽略.datignore文件中的任何文件，就像git一样。每个文件都应该以换行符分割。除此之外Dat还会自动忽略隐藏文件。

Dat使用[dat-ignore][dat_ignore]来判断是否该忽略某个文件。该模块支持使用模式通配符(/*.png)和文件夹通配符(/**/cache)。

### 选取文件

默认情况下，Dat会下载所有文件。如果你只想下载一部分，你可以创建一个`.datdownload`文件来指定下载哪些文件和文件夹。每个文件或文件夹用换行符分割。

### 下载

通过运行`clone`命令来开始下载。这样会创建一个文件夹，将文件内容、元数据和.dat文件夹下载到里面。

    dat clone <link> [<dir>] [--temp]

将远程dat存档复制到本地文件夹。如果没有指定文件夹的话，这个命令会创建一个以秘钥命名的文件夹。

### 通过 dat.json 秘钥下载

你还可以使用 dat.json 文件来下载。当你将git和Dat结合的时候就会很有用。你可以指定一个包含dat.json的文件夹来下载dat。

```commandline
git clone git@github.com:joehand/dat-clone-sparse-test.git
dat clone ./dat-clone-sparse-test
```
这样就可以下载dat.json文件中指定的dat了。

### 更新已下载的dat存档
在你下载了一个dat之后，你可以在该文件夹中运行`dat pull`或`dat sync`来更新存档。

    dat pull [<dir>]
    dat sync [<dir>]

其中pull命令会下载最新的dat存档然后退出。  
sync会保持连接，只要远程资源有更新，它就会实时更新本地的dat。

### 快捷命令

* `dat <link> <dir>`将会在 \<dir> 文件夹中创建一个新的dat或者运行已存在的dat。  
* `dat <link>`和 `dat sync <dir>`命令是一样的。

### Dat的注册与登录

建设中………

### 登录验证（实验性）

你可以使用dat命令行工具注册和登录（翻墙才可以登录）。Dat计划支持任何注册点，不过目前`datbase.org`唯一可用的注册点也是默认注册点。

你可以使用下面的命令来注册和登录：
```commandline
dat register [<注册点地址 默认是datbase.org>]
dat login
dat whoami
```
当你登录到一个注册点之后，你就可以发布你的dat存档了。
```commandline
cd my-data
dat create
dat publish --name my-dataset
```
如果你想要发布到 datbase.org 以外的站点，你就必须在所有的请求中添加 <registry> 选项。如果你不想用我们的服务器，你可以自己搭建一个[registry server][registry_server]。

### 密钥的管理 & dat的移动

`dat keys`提供了一些命令来帮助你移动或备份你的dat。

对一个dat进行写入操作需要使用密钥，密钥储存在`~/.dat`文件夹中。你可以在dat之间导入导出这些密钥。首先，把你的dat克隆到新的位置：

* (original) `dat share`
* (duplicate) `dat clone <link>`

然后来传输密钥：

* (original) `dat keys export` - 复制输出的密钥
* (duplicate) `dat import` - 该命令会引导你输入密钥，把刚刚复制的密钥输入进去。
`dat import`命令暂时不支持直接将密钥作为参数
错误示范：[直接将密钥作为参数，会收到报错][dat_keys_import_issue]`Sorry, could not find a dat in this directory.`
```commandline
dat keys import c787010bf80ec8457abb2f35f02253509f44bc2a6bb1e0cd7f5ca1db4d0c7941776c9afb9f6d1726ff9ec126f08e30e27aea8234e5cf2453d9571ad8446a1928
Sorry, could not find a dat in this directory.
```
## 疑难解答

我们根据用户遇到的问题提出了一些错误排除技巧。如果你碰到一些这里没有提到的疑难问题需要解决，你可以[open an issue][open_an_issue]或者在我们的[chat room][chat_room]中向我们咨询

如果你在一个有.dat文件夹的目录中分享文件或下载文件时遇到问题，你可以将.dat文件夹删除然后重试。


## 命令行调试

    DEBUG=dat,dat-node dat clone dat://<link> dir
