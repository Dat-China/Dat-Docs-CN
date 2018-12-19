[dat_node]: https://github.com/datproject/dat-node
[hyperdrive]: https://npmjs.org/hyperdrive
[hyperdiscovery]: https://npmjs.org/hyperdiscovery
[cli]: https://npmjs.org/dat
[dat_storage]: https://github.com/datproject/dat-storage
[development_work_flow]: https://github.com/datproject/dat/blob/master/CONTRIBUTING.md#development-workflow
[mirror_folder]: https://github.com/mafintosh/mirror-folder

# 使用Hyperdrive FS

Hyperdrive，dat中的核心文件系统模块，它提供一些类似于Node fs API的API。我们还有一些使用了这个自定义fs的模块，比如mirror-folder

[Mirror folder][mirror_folder]将一个常规目录复制到另一个常规目录中。
```javascript
var mirror = require('mirror-folder')

mirror('/source', '/dest', function (err) {
  console.log('mirror complete')
})
```

你还可以将文件夹复制到一个archive（这就是dat导入文件的方法）：

```javascript
var archive = hyperdrive('/dir')
mirror('/source', {name: '/', fs: archive}, function (err) {
  console.log('mirror complete')
})
```

### 创建自定义 FS 模块

```javascript
function printFile(file, fs) {
  if (!fs) fs = require('fs')

  fs.readFile(file, 'utf-8', function (err, data) {
    console.log(data)
  }) 
}
```

下面你可以用它来从常规文件系统打印一个文件：

    printFile('/data/hello-world.txt')

或从hyperdrive archive：

```javascript
var archive = hyperdrive('/data')
printFile('/hello-world.txt', archive) // pass archive as the fs!
```

## Modules!

更多关于自定义文件系统模块的示例：

* [mirror-folder]() - copy files from one fs to another fs (regular or custom)
* [count-files]() - count files in regular or custom fs.
* [ftpfs]() - custom fs for FTPs
* [bagit-fs]() - custom fs module for the BagIt spec.

