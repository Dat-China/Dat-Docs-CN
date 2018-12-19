[dat_node]: https://github.com/datproject/dat-node
[hyperdrive]: https://npmjs.org/hyperdrive
[hyperdiscovery]: https://npmjs.org/hyperdiscovery
[cli]: https://npmjs.org/dat
[dat_storage]: https://github.com/datproject/dat-storage
[development_work_flow]: https://github.com/datproject/dat/blob/master/CONTRIBUTING.md#development-workflow

# 用Dat构建应用

在这个教程中，我们将会展示如何用Dat生态系统开发应用程序。Dat的高度模块化使得Dat很适合定制开发应用程序。

Dat内置了桌面应用与命令行工具使用过的javascript API。为了定制应用、更好地掌控，你也可以分开使用Dat的核心模块。

在你的JS应用中使用Dat：
1. `require('dat')`:使用[Dat JS API](https://github.com/datproject/dat-node)
2. 下面由你自己来构建！

这个教程将会带你熟悉Dat的核心模块。

## Dat核心模块

这个教程将会按照分享、下载文件的步骤来进行。实际上，我们在一个高级模块——[dat-node][dat_node]中实现了对Dat核心模块的简易操作。

任何Dat应用程序，首先要有两个基本模块

1. 用于同步和版本化文件的[hyperdrive][hyperdrive]
2. 用来发现于连接其他peers的[hyperdiscovery][hyperdiscovery]

[Dat CLI][cli]（command line interface 命令行界面）模块将这些模块结合起来并且将他们封装在一个命令行API中。
我们还使用了[dat-storage][dat_storage]模块来处理文件和秘钥的储存。这些模块也可以被替换为一些相似的兼容模块。

## 开始
安装好npm 和 node 才能利用Dat构建应用。你可以通过阅读我们的[开发工作流程][development_work_flow]来了解我们是如何在开发过程中管理理模块依赖关系的。

## 下载文件
我们的第一个模块可以从用户输入的Dat 链接下载文件。
```commandline
mkdir module-1 && cd module-1
npm init
npm install --save hyperdrive random-access-memory hyperdiscovery
touch index.js
```

在这个例子中，我们使用random-access-momery访问我们的数据库。在你的`index.js`文件中需要有主模块并且将他们设置好。
```javascript
var ram = require('random-access-memory')
var hyperdrive = require('hyperdrive')
var discovery = require('hyperdiscovery')

var link = process.argv[2] // user inputs the dat link
var key =  link.replace('dat://', '') // extract the key

var archive = hyperdrive(ram, key)
archive.ready(function () {
  discovery(archive)
})
```

注意，用户将会在第二个参数位输入链接，从hyperdrive archive中获取文件最简单的方法是创建一个文件读取流。`archive.readFile`在第一个参数位接收文件名的索引数。要显示该文件，我们可以创建一个文件流并利用pipe传输到process.stdout

```javascript
// Make sure your archive has a dat.json file!
var stream = archive.readFile('dat.json', 'utf-8', function (err, data) {
  if (err) throw err
  console.log(data)
})
```

现在你可以运行这个模块了！从一个archive下载dat.json文件：

```commandline
node index.js dat://<link>
```

运行后你应该能看到dat.json的内容输出到了命令行。

### 惊喜：在Dat中展示任何文件

再加几行代码，用户就可以使用该模块从指定dat获取任何文件！

挑战：创建一个模块，允许用户输入dat链接与一个文件名，运行后该模块会输出所指定的文件，就像下面这样：

    var stream = archive.readFile(fileName)

写完之后你可以这样测试一下：

    node bonus.js 395e3467bb5b2fa083ee8a4a17a706c5574b740b5e1be6efd65754d4ab7328c2 readme.md

## 将所有文件下载到电脑

这个模块建立在最后一个模块的基础上。我们不再显示单个文件而是将Dat中所有文件下载到本地目录中。

我们将使用[mirror-folder](https://github.com/mafintosh/mirror-folder)来将文件下载到文件系统。阅读[了解更多][read_more]mirror-folder如何与hyperdrive协同工作。

而在实践中，你应该使用dat-storage来做这件事因为这样更高效并且会将元数据保存在磁盘上。

安装过程和之前都是一样的（确保你安装了mirror-folder）。模块的第一部分也和之前的看起来一样。

```javascript
var path = require('path')
var ram = require('random-access-memory')
var hyperdrive = require('hyperdrive')
var discovery = require('hyperdiscovery')
var mirror = require('mirror-folder')
var mkdirp = require('mkdirp')

var link = process.argv[2] // user inputs the dat link
var key = link.replace('dat://', '') // extract the key
var dir = path.join(process.cwd(), 'dat-download') // make a download folder
mkdirp.sync(dir)

var archive = hyperdrive(ram, key)
archive.ready(function () {
  discovery(archive)

  var progress = mirror({name: '/', fs: archive}, dir, function (err) {
    console.log('done downloading!')
  })
  progress.on('put', function (src) {
    console.log(src.name, 'downloaded')
  })
})
```
运行这个模块，你应该能在download文件夹中看到我们所有的文件。

    node index.js dat://<link>