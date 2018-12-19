# 服务器上的Dat

由于Dat是一个分布式数据分享工具（点对点），电脑必须积极地共享数据才能保证数据的可用性。如果你通过Dat分享文件，你可能需要设置一个专用的服务器来托管你的dat。这意味着即使你关闭了你的个人电脑dat仍然可以保持可用。

在服务器上运行Dat也可以用于实时备份。只要连接到服务器并同步更改，服务器就可以备份所有内容历史记录，允许你在以后查看旧版本内容。

## 简短的说明
我们已经构建了一个使用命令行托管多个dat的减淡工具。这个工具叫[hypercored]()，Hypercored读取包含要共享的所有数据的文件。

在你的服务器上安装`Hypercored`，创建一个`feeds`文件，其中写入你的dat并用换行符分隔，然后运行`hypercore`。

    npm install -g hypercored
    echo 'dat://64375abb733a62fa301b1f124427e825d292a6d3ba25a26c9d4303a7987bec65' >> feeds
    echo 'dat://another-dat-link-here' >> feeds
    hypercored

现在现在他会下载并保存`feeds`文件中的每一个dat  
Hypercored使用[hypercore-archiver](https://github.com/mafintosh/hypercore-archiver)来实现dat的高效共享和内容完整的历史备份。

下面是更多的详细说明

## 详细说明
### Node的版本
检查你的node版本，你至少应该拥有4.0或更高版本的node，6.10.3以上的版本就更好了。
    
    $ node -v

然后到你的服务器上安装`hypercored`：

    npm install -g hypercored

如果你遇到权限问题导致的安装失败，请参考[此教程](https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally)。

现在创建一个名叫'feeds'的文件，在其中写入你想分享的dat。

feeds

    dat://one-hash
    two-hash
    website.com/three-hash

下面，如果你使用`ls`命令应该会看到以下文件

    $ ls
    feeds
现在，要共享这些数据，只需在此目录中输入`Hypercored`

    ~/dat $ hypercored
    Watching ~/dat/feeds for a list of active feeds
    Archiver key is 42471e32d36be3cb617ec1df382372532aac1d1ce683982962fb3594c5f9532a
    Swarm listening on port 58184
太好了！feeds文件中的所有dat都将被下载并被重新托管。但是，它们是在前台运行的-你可能希望使用一个进程管理器来运行并监视进程以保证它们不会崩溃。

## Run it Forever
我们推荐使用`lil-pids`和`add-to-systemd`在linux服务器上长期托管dat。

    npm install -g add-to-systemd lil-pids
    mkdir ~/dat
    echo "hypercored --cwd ~/dat" > ~/dat/services
    sudo add-to-systemd dat-lil-pids $(which lil-pids) ~/dat/services ~/dat/pids

将以上命令中的~替换为你存放的dat的路径