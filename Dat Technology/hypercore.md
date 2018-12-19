[build_passing_img]: https://travis-ci.org/mafintosh/hyperdrive.svg?branch=master
[build_passing]: https://travis-ci.org/mafintosh/hyperdrive

# hypercore
Hypercore是一个安全的、分布式仅允许添加内容的日志。

hypercore作为[Dat project][dat_project]的一部分，专为分享大数据集和实时数据流而设计。

    npm install hypercore
    
[![build passing][build_passing_img]][build_passing]

如果想要在技术层面上了解更多关于hypercore是如何工作的，请阅读[Dat的论文][dat_paper]。

## 特性

* 稀疏复制(Sparse replication)。只下载你感兴趣的数据。
* 实时。安全快速地记录最新的更新。
* 高性能。使用一个简单的flat file结构最大化IO性能。
* 安全。使用有签名的merkle树来实时校验日志的完整性。
* 浏览器支持。简单地选择了一个可以在浏览器中工作的storage provider。

## 用法

```javascript
var hypercore = require('hypercore')
var feed = hypercore('./my-first-dataset', {valueEncoding: 'utf-8'})

feed.append('hello')
feed.append('world', function (err) {
  if (err) throw err
  feed.get(0, console.log) // prints hello
  feed.get(1, console.log) // prints world
})
```

## 术语

* **feed**，就是hypercores，一个数据传输专线。Feeds是一个被分享在dat网络上的可以长久保存的数据结构。
* **stream** 代码中用于读取或写入数据的工具。流是临时的，几乎都是由函数返回的。
* **pipe** 流既是可读的（giving data）也是可写的（receiving data）。如果你将一个可读流连接到一个可写流就叫piping。
* **replication stream** 一个由`replicate()`函数返回且可以连接到其他对等节点的流。它是用来同步每个节点的hypercore feeds的。
* **swarming** Swarming描述了将你自己添加到网络，寻找对等节点，与他们而非娘数据