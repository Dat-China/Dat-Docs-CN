[build_passing_img]: https://travis-ci.org/mafintosh/hyperdrive.svg?branch=master
[build_passing]: https://travis-ci.org/mafintosh/hyperdrive
#Hyperdrive

Hyperdrive是一个实时的分布式文件系统。

    npm install Hyperdrive

[![build passing][build_passing_img]][build_passing]
## 用法

Hyperdrive旨在实现与Node核心fs模块一样的API。
```javascript
var hyperdrive = require('hyperdrive')
var archive = hyperdrive('./my-first-hyperdrive') // content will be stored in this folder

archive.writeFile('/hello.txt', 'world', function (err) {
  if (err) throw err
  archive.readdir('/', function (err, list) {
    if (err) throw err
    console.log(list) // prints ['hello.txt']
    archive.readFile('/hello.txt', 'utf-8', function (err, data) {
      if (err) throw err
      console.log(data) // prints 'world'
    })
  })
})
```

与fs不同的是，你只需要创建一个流就可以将文件系统复制到其它电脑。

```javascript
var net = require('net')

// ... on one machine

var server = net.createServer(function (socket) {
  socket.pipe(archive.replicate()).pipe(socket)
})

server.listen(10000)

// ... on another

var clonedArchive = hyperdrive('./my-cloned-hyperdrive', origKey)
var socket = net.connect(10000)

socket.pipe(clonedArchive.replicate()).pipe(socket)
```

他还带有内置的版本系统和实时复制功能。

