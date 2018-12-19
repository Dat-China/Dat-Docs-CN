# 浏览器与Dat

Dat是用JavaScript写的，因此很自然地，它完全可以在浏览器中工作！最重要的是，随着更多的用户通过他们的浏览器互相连接，网站的资源将在用户之间直接分享，而不需要访问通过服务器。

这种方法类似于 Feross' [Web Torrent](http://webtorrent.io/)的方法。不同的是，Dat链接可以动态地呈现和读取，而BitTorrent链接是静态的。原始所有者可以更新目录中的文件，所有节点的用户都将自动接收更新。

## WebRTC使用说明
**Important：** dat-js使用WebRTC，因此它只能连接到其他WebRTC客户端。
dat-js库不可能使用其他协议与其它客户端建立连接。
所有其他的Dat应用程序都是用非WebRTC协议（想要了解更多，请查看[FAQ](https://docs.datproject.org/faq#does-dat-use-webrtc)）。
非浏览器客户端可以通过webrtc模块
（如[electron-webrtc](https://github.com/mappum/electron-webrtc)或者通过websockets、http或其他C/S协议使用proxies）点对点连接到dat。

由于WebRTC性能不佳，所以Dat正致力于使用其他协议创建构建的网络。如果性能有所提高并且在非浏览器接口中运行变得更容易，我们可以集成WebRTC。（虽然我们更想使用[性能更好的选项](https://github.com/noffle/web-udp)）

下面我们步入正题。

## 安装

在页面中嵌入以下脚本[dat.min.js](https://cdn.jsdelivr.net/dat/6.2.0/dat.min.js)

```html
<script type="text/javascript" src="dat.min.js"></script>
```

你也可以在线引用 jsdelivr CDN 来提高加载速度。
```html
<script type="text/javascript" src="https://cdn.jsdelivr.net/dat/6.2.0/dat.min.js"></script>
```

这提供了一个Dat的window对象原型。

#### 浏览器化（[Browserify](http://github.com/substack/node-browserify)）
>Browsers don't have the require method defined, but Node.js does. With Browserify you can write code that uses require in the same way that you would use it in Node.

>浏览器没有定义require方法但是Node.js有。使用Browserify你就可以像在Node中一样使用require写代码。

安装dat-js

    npm install dat-js

像这样使用

```javascript
var Dat = require('dat-js')
```
## 快速示例
### 分享数据

```javascript
var dat = Dat()
dat.add(function (repo) {
  var writer = repo.archive.createFileWriteStream('hello.txt')
  writer.write('world')
  writer.end(function () { replicate(repo.key) })
})
```

### 下载数据
```javascript
var Dat = require('dat-js')

var clone = Dat()
clone.add(key, function (repo) {
  repo.archive.readFile('hello.txt', function (err, data) {
    console.log(data.toString()) // prints 'world'
  })
})
```

repo.archive 是一个[hyperdrive]实例，它管理者所有文件。一个hyperdrive archive有许多简单易用的方法其中包括只从指定的仓库获取文件和bit ranges。

有关完整的超级驱动器API和更多示例，请参见完整的[Hyperdrive文档](https://docs.datproject.org/hyperdrive)。

## 仅下载你所需要的
你可能会问：“是否可以对一个dat数据集的子集建立索引？”大多数数据集对于浏览器来说都太大了，而且我们很可能只需要其中的一个子集。

你可以通过使用稀疏模式来做到这一点，这使得它只会下载peer请求的内容。为了做到这一点，只需要在创建dat时向参数中加入sparse选项 {sparse: true} 。
```javascript
var Dat = require('dat-js')

var dat= Dat()
dat.add(key, {sparse: true}, function (repo) {
  // etc..
})
```

## 背后的原理

让我们来看一下如何构建一个基于浏览器的dat的底层实现。

下面是直接使用底层模块的简单示例

```javascript
var webrtc = require('webrtc-swarm')
var signalhub = require('signalhub')
var hyperdrive = require('hyperdrive')
var memdb = require('memdb')
var pump = require('pump')

var DEFAULT_SIGNALHUBS = 'https://signalhub.mafintosh.com'

var archive = hyperdrive()
var link = archive.discoveryKey.toString('hex')

var swarm = webrtc(signalhub(link, DEFAULT_SIGNALHUBS))
swarm.on('peer', function (conn) {
  var peer = archive.replicate({
    upload: true,
    download: true
  })
  pump(conn, peer, conn)
})
```

现在你正在浏览器中提供一个兼容dat的hyperdrive。

在另一个浏览器中，你可以连接到swarm并且使用和上面相同的代码下载数据。确保通过将archive.key作为第一个参数来引用之前创建的archive。

## 元数据和内容的仓库API

Hyperdrive是dat运行的基础数据库。

Hyperdrive 会分开储存metadata和内容，你可以控制他们保存在哪里以及它们如何被检索。这些调整对性能、稳定性和用户体验有着巨大影响，因此理解权衡是很重要的。

在浏览器中有无数种不同的数据储存与数据检索方法，并且在不同情况下都各有利弊。未来说清楚这一点，我们编译了各种各样的示例。

Hyperdrive的第一个参数是所有元数据和内容的主数据库。可以编辑file选项来指定如何读写数据，如果不提供file选项，内容将会被储存在主数据库中。

有许多不同的方法可以将模块组装在一起，以便为超级驱动器创建存储基础设施-以下是一些经过测试的示例：

## 将大型文件从文件系统写入浏览器

文件写入受机器可用内存限制。
当文件被写入hyperdrive实例时，文件被缓存到内存中。这种方法并不理想，但是只要文件的大小没超过系统的RAM限制就没有问题。

为了解决这个问题，你可以使用[random-access-file-readeer](https://github.com/mafintosh/random-access-file-reader)来直接从文件系统读取文件而不是把他们缓存到内存中。

[![](https://img.shields.io/badge/irc%20channel-%23dat%20on%20freenode-blue.svg)](http://webchat.freenode.net/?channels=dat)
[![](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/datproject/discussions?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)