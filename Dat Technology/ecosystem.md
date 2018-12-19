# 开源工具

我们构建了各种各样的模块来支持我们在Dat方面的工作和更大规模的数据与代码生态系统。你可以来深入了解一下我们在[命令行](https://github.com/datproject/dat)和[桌面应用](https://github.com/dat-land/dat-desktop)的用到的实现。

Dat采用了Unix的哲学：模块化设计。所有的组件都可以替换。我们相信这样可以创造出更好的用户端软件与更多可持续、有影响力的开源工具。

# 用户软件

* [dat](https://github.com/datproject/dat) - Dat的命令行工具（接口）。
* [Dat Desktop](https://github.com/datproject/dat-desktop) - Dat的桌面应用。
* [datproject.org](https://datproject.org) - 

# 规范

[Dat Whitepaper](https://github.com/datproject/docs/tree/master/papers) - Dat白皮书
[Dat Protocol](https://www.datprotocol.com/) - Dat协议的网站

# 核心模块
这些模块组成了Dat软件的主干：

* [hypercore](https://github.com/mafintosh/hypercore) - 一个安全的、分布式的只允许添加内容（append-only）的日志。
* [hyperdrive](https://github.com/mafintosh/hyperdrive) - 一个安全的，实时的分布式文件系统（基于hypercore构建）。
* [dat-node](https://github.com/datproject/dat-node) - 用于在文件系统中构建Dat应用的高级模块。
* [hyperdiscovery](https://github.com/karissa/hyperdiscovery) - 默认情况下用来发现网络与管理连接的模块。
* [dat-storage](https://github.com/datproject/dat-storage) - Dat的默认储存模块。

到[Github](https://github.com/search?utf8=%E2%9C%93&q=topic%3Adat&type=Repositories)了解更多。

# 我们使用到的模块&我们青睐的模块

我们的应用程序中使用了这些模块：

* [Choo](https://github.com/yoshuawuyts/choo) - 一个用于构建健壮的前端应用的框架。
* [neat-log](https://github.com/joehand/neat-log)  - A neat cli logger for stateful command line applications Edit Add topics.
* [mirror-folder](https://github.com/mafintosh/mirror-folder) - 将一个文件夹复制到另一个文件夹，支持Hyperdrive和文件的实时监视。
* [toiletdb](https://github.com/maxogden/toiletdb) - 一个使用json格式存储的支持增删查改的数据库。
