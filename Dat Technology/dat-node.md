[npm_img]: https://img.shields.io/npm/v/dat-node.svg?style=flat-square
[npm]: https://npmjs.org/package/dat-node
[build_passing_img]: https://img.shields.io/travis/datproject/dat-node/master.svg?style=flat-square
[build_passing]: https://travis-ci.org/datproject/dat-node
[coverage_img]: https://img.shields.io/codecov/c/github/datproject/dat-node/master.svg?style=flat-square
[coverage]: https://codecov.io/github/datproject/dat-node
[greenkeeper_img]: https://badges.greenkeeper.io/datproject/dat-node.svg
[greenkeeper]: https://greenkeeper.io/
[dat]: http://datproject.org/
# dat-node

**dat-node**是一个在文件系统上构建Dat应用的高级模块。

|[![npm][npm_img]][npm]|[![build passing][build_passing_img]][build_passing]|[![coverage][coverage_img]][coverage]|[![Greenkeeper][greenkeeper_img]][greenkeeper]|
|---|---|---|---|

[Dat][dat]是一个去中心化的文件数据分发工具，为科学与数据研究而设计。你可以通过这些客户端应用快速上手Dat。
* [Dat Command Line][dat_command_line] - 在命令行中使用Dat。
* [Dat Desktop][dat_desktop] - 一个Dat的桌面应用。
* [Breaker Browser][breaker_browser] - 一个内置Dat的p2p浏览器。

## Dat Project 的文档和资源

* [Dat中文文档][dat_docs_cn]
* [Dat Project Docs][dat_project_docs]
* [Gitter Chat][gitter_chat] 或 [#dat on IRC][dat_on_irc]

## Features
* Dat与hyper模块的链接
* 利用[discovery-swarm][discovery_swarm]轻松地连接到Dat网络。
* 使用[mirror-folder][mirror_folder]从文件系统导入文件。
* 使用[hyperdrive-http][hyperdrive_http]实现通过HTTP分享dat。
* 使用`require`就可以访问底层的接口！

## 浏览器支持
我们的很多依赖项都可以在浏览器中工作，但是dat-node是为文件系统应用程序量身定做的。如果你想要构建浏览器友好的Dat应用，查看一下[dat-js][dat_js]的文档吧。

# 示例

通过Dat发送文件：
1. 告诉dat-node文件在哪里
1. 导入文件
1. 在Dat网络上分享文件！（并且分享链接）
```javascript
    var Dat = require('dat-node')

// 1. My files are in /joe/cat-pic-analysis
Dat('/joe/cat-pic-analysis', function (err, dat) {
  if (err) throw err

  // 2. Import the files
  dat.importFiles()

  // 3. Share the files on the network!
  dat.joinNetwork()
  // (And share the link)
  console.log('My Dat link is: dat://', dat.key.toString('hex'))
})
```

现在这些文件可以通过打印在控制台窗口中的密钥分享了。

你需要在另一个文件夹中创建一个dat-node实例来下载这些文件。这次我们还是三步走：
1. 告诉dat你想把文件下载到哪里。
1. 告诉dat链接是什么。
3. 加入dat网络然后开始下载！

```javascript
var Dat = require('dat-node')

// 1. Tell Dat where to download the files
Dat('/download/cat-analysis', {
  // 2. Tell Dat what link I want
  key: '<dat-key>' // (a 64 character hash from above)
}, function (err, dat) {
  if (err) throw err

  // 3. Join the network & download (files are automatically downloaded)
  dat.joinNetwork()
})
```

默认情况下，当你连接到别的用户时会自动下载所有文件。

## 示例应用
* [Dat CLI][dat_cli]：我们在dat CLI中使用了dat-node。
* [Dat Desktop][dat_desktop]：Dat的桌面应用通过[dat-worker][dat_worker]管理多个dat-node实例。
* 查看[examples folder][examples_folder]来了解最基本的分享与下载的用法。
* 如果你构建了一个简洁的dat-node应用程序想要添加在这个列表中，请告诉我们。

# 用法
所有的dat-node应用程序都有相似的三要素。
1. **storage** - 储存文件和元数据的地方。
1. **network** - 连接到其他用户来下载或上传数据。
3. **adding files** - 从文件系统向hyperdrive archive中添加文件。

下面，我们将介绍每一要素的用途以及每一要素的用法。

## 储存空间（Storage）
每一个dat存档都有**storage**，这也是dat-node实例化时所需要的第一个参数。默认情况下，我们使用[dat-storage][dat_storage]模块。其他选项包括：
* **Persistent storage**：通过将`my-dir`作为第一个参数，将文件储存在`/my-dir`并将元数据储存在`my-dir/.dat`。
* **Temporary storage**：使用`temp: true`选项来将元数据储存在内存中。

```javascript
// Permanent Storage
Dat('/my-dir', function (err, dat) {
  // Do Dat Stuff
})

// Temporary Storage
Dat('/my-dir', {temp: true}, function (err, dat) {
  // Do Dat Stuff
})
```

这两种方法都会在执行`dat.importFiles()`时从`/my-dir`导入文件，但是只有第一种会在文件夹中创建`.dat`文件夹并且将元数据储存在磁盘上。

storage的参数还可以传递给hyperdrive以获取更高级的用例。

## Network
Dat与网络不可分割。每次创建了dat后，你一定很想使其加入网络。

```javascript
Dat('/my-dir', function (err, dat) {
  dat.joinNetwork()
  dat.network.on('connection', function () {
    console.log('I connected to someone!')
  })
})
```

## Downloading Files
请记住，无论你是否在下载元数据和文件，一旦你连接到网络，下载就会自动开始了。

Dat运行在点对点网络上，有时持有某个密钥的用户可能都不在线。你可以在`joinNetwork`中使用回调函数来使你的应用变得更加用户友好。

```javascript
// Downloading <key> with joinNetwork callback
Dat('/my-dir', {key: '<key>'}, function (err, dat) {
  dat.joinNetwork(function (err) {
    if (err) throw err

    // After the first round of network checks, the callback is called
    // If no one is online, you can exit and let the user know.
    if (!dat.network.connected || !dat.network.connecting) {
      console.error('No users currently online for that key.')
      process.exit(1)
    }
  })
})
```

### Download on Demand
如果你想要决定下载或不下载哪些文件，你可以使用`sparse`选项：
```javascript
// Downloading <key> with sparse option
Dat('/my-dir', {key: '<key>', sparse: true}, function (err, dat) {
  dat.joinNetwork()

  // Manually download files via the hyperdrive API:
  dat.archive.readFile('/cat-locations.txt', function (err, content) {
    console.log(content) // prints cat-locations.txt file!
  })
})
```

在`sparse`模式下，Dat只会下载你所指定的部分元数据和文件。


## 导入文件（Importing Files）
有很多方法可以将文件导入archive。Dat node提供了一些基本的方法。如果你需要跟高级的导入方法，你可以直接使用`archive.createWriteStream()`方法。

默认情况下会调用`dat.importFiles()`从你初始化的文件夹中导入文件。你可以通过设置watch选项来查看那个文件夹的变更。

```javascript
Dat('/my-data', function (err, dat) {
  if (err) throw err

  var progress = dat.importFiles({watch: true}) // with watch: true, there is no callback
  progress.on('put', function (src, dest) {
    console.log('Importing ', src.name, ' into archive')
  })
})
```

你也可以从别的文件夹导入文件：

```javascript
Dat('/my-data', function (err, dat) {
  if (err) throw err

  dat.importFiles('/another-dir', function (err) {
    console.log('done importing another-dir')
  })
})
```


这些例子涵盖了最普通的用法，如果还有什么要添加的请联系我们。继续阅读完整的API文档吧。

## API

### `Dat(dir|storage, [opts], callback(err, dat))`

Initialize a Dat Archive in `dir`. If there is an existing Dat Archive, the archive will be resumed.

#### Storage

* `dir` (Default) - Use [dat-storage](https://github.com/datproject/dat-storage) inside `dir`. This stores files as files, sleep files inside `.dat`, and the secret key in the user's home directory.
* `dir` with `opts.latest: false` - Store as SLEEP files, including storing the content as a `content.data` file. This is useful for storing all history in a single flat file.
* `dir` with `opts.temp: true` - Store everything in memory (including files).
* `storage` function - pass a custom storage function along to hyperdrive, see dat-storage for an example.

Most options are passed directly to the module you're using (e.g. `dat.importFiles(opts)`. However, there are also some initial `opts` can include:

```js
opts = {
  key: '<dat-key>', // existing key to create archive with or resume
  temp: false, // Use random-access-memory as the storage.

  // Hyperdrive options
  sparse: false // download only files you request
}
```

The callback, `cb(err, dat)`, includes a `dat` object that has the following properties:

* `dat.key`: key of the dat (this will be set later for non-live archives)
* `dat.archive`: Hyperdrive archive instance.
* `dat.path`: Path of the Dat Archive
* `dat.live`: `archive.live`
* `dat.writable`: Is the `archive` writable?
* `dat.resumed`: `true` if the archive was resumed from an existing database
* `dat.options`: All options passed to Dat and the other submodules

### Module Interfaces

**`dat-node` provides an easy interface to common Dat modules for the created Dat Archive on the `dat` object provided in the callback:**

#### `var network = dat.joinNetwork([opts], [cb])`

Join the network to start transferring data for `dat.key`, using [discovery-swarm](https://github.com/mafintosh/discovery-swarm). You can also use `dat.join([opts], [cb])`.

If you specify `cb`, it will be called *when the first round* of discovery has completed. This is helpful to check immediately if peers are available and if not fail gracefully, more similar to http requests.

Returns a `network` object with properties:

* `network.connected` - number of peers connected
* `network.on('listening')` - emitted with network is listening
* `network.on('connection', connection, info)` - Emitted when you connect to another peer. Info is an object that contains info about the connection

##### Network Options

`opts` are passed to discovery-swarm, which can include:

```js
opts = {
  upload: true, // announce and upload data to other peers
  download: true, // download data from other peers
  port: 3282, // port for discovery swarm
  utp: true, // use utp in discovery swarm
  tcp: true // use tcp in discovery swarm
}

//Defaults from datland-swarm-defaults can also be overwritten:

opts = {
  dns: {
    server: // DNS server
    domain: // DNS domain
  }
  dht: {
    bootstrap: // distributed hash table bootstrapping nodes
  }
}
```

Returns a [discovery-swarm](https://github.com/mafintosh/discovery-swarm) instance.

#### `dat.leaveNetwork()` or `dat.leave()`

Leaves the network for the archive.

#### `var importer = dat.importFiles([src], [opts], [cb])`

**Archive must be writable to import.**

Import files to your Dat Archive from the directory using [mirror-folder](https://github.com/mafintosh/mirror-folder/).

* `src` - By default, files will be imported from the folder where the archive was initiated. Import files from another directory by specifying `src`.
* `opts` - options passed to mirror-folder (see below).
* `cb` - called when import is finished.

Returns a `importer` object with properties:

* `importer.on('error', err)`
* `importer.on('put', src, dest)` - file put started. `src.live` is true is file was added by file watch event.
* `importer.on('put-data', chunk)` - chunk of file added
* `importer.on('put-end', src, dest)` - end of file write stream
* `importer.on('del', dest)` - file deleted from dest
* `importer.on('end')` - Emits when mirror is done (not emitted in watch mode)
* If `opts.count` is true:
  * `importer.on('count', {files, bytes})` - Emitted after initial scan of src directory. See import progress section for details.
  * `importer.count` will be `{files, bytes}` to import after initial scan.
  * `importer.putDone` will track `{files, bytes}` for imported files.

##### Importer Options

Options include:

```js
var opts = {
  count: true, // do an initial dry run import for rendering progress
  ignoreHidden: true, // ignore hidden files  (if false, .dat will still be ignored)
  ignoreDirs: true, // do not import directories (hyperdrive does not need them and it pollutes metadata)
  useDatIgnore: true, // ignore entries in the `.datignore` file from import dir target.
  ignore: // (see below for default info) anymatch expression to ignore files
  watch: false, // watch files for changes & import on change (archive must be live)
}
```

##### Ignoring Files

You can use a `.datignore` file in the imported directory, `src`, to ignore any the user specifies. This is done by default.

`dat-node` uses [dat-ignore](https://github.com/joehand/dat-ignore) to provide a default ignore option, ignoring the `.dat` folder and all hidden files or directories. Use `opts.ignoreHidden = false` to import hidden files or folders, except the `.dat` directory.

*It's important that the `.dat` folder is not imported because it contains a private key that allows the owner to write to the archive.*

#### `var stats = dat.trackStats()`

##### `stats.on('update')`

Emitted when archive stats are updated. Get new stats with `stats.get()`.

##### `var st = dat.stats.get()`

`dat.trackStats()` adds a `stats` object to `dat`.  Get general archive stats for the latest version:

```js
{
  files: 12,
  byteLength: 1234,
  length: 4, // number of blocks for latest files
  version: 6, // archive.version for these stats
  downloaded: 4 // number of downloaded blocks for latest
}
```

##### `stats.network`

Get upload and download speeds: `stats.network.uploadSpeed` or `stats.network.downloadSpeed`. Transfer speeds are tracked using [hyperdrive-network-speed](https://github.com/joehand/hyperdrive-network-speed/).

##### `var peers = stats.peers`

* `peers.total` - total number of connected peers
* `peers.complete` - connected peers with all the content data

#### `var server = dat.serveHttp(opts)`

Serve files over http via [hyperdrive-http](https://github.com/joehand/hyperdrive-http). Returns a node http server instance.

```js
opts = {
  port: 8080, // http port
  live: true, // live update directory index listing
  footer: 'Served via Dat.', // Set a footer for the index listing
  exposeHeaders: false // expose dat key in headers
}
```

#### `dat.pause()`

Pause all upload & downloads. Currently, this is the same as `dat.leaveNetwork()`, which leaves the network and destroys the swarm. Discovery will happen again on `resume()`.

#### `dat.resume()`

Resume network activity. Current, this is the same as `dat.joinNetwork()`.

#### `dat.close(cb)`

Stops replication and closes all the things opened for dat-node, including:

* `dat.archive.close(cb)`
* `dat.network.close(cb)`
* `dat.importer.destroy()` (file watcher)

## License

MIT

[0]: https://img.shields.io/npm/v/dat-node.svg?style=flat-square
[1]: https://npmjs.org/package/dat-node
[2]: https://img.shields.io/travis/datproject/dat-node/master.svg?style=flat-square
[3]: https://travis-ci.org/datproject/dat-node
[4]: https://img.shields.io/codecov/c/github/datproject/dat-node/master.svg?style=flat-square
[5]: https://codecov.io/github/datproject/dat-node

