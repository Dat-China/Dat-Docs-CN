# **通过HTTP分享文件**

Dat命令行工具中内置了HTTP服务器。这是一个很炫酷的示例，因为我们可以看到内置版本历史管理系统是如何工作的！`--http`选项可适用于你正在分享或下载的文件。

## **通过HTTP提供服务**
通过添加`--http`选项就可以通过http分享dat文件。比如，你可以同步一个现有的Dat：
    
    ❯ dat sync --http
    dat://778f8d955175c92e4ced5e4f5563f69bfec0c86cc6f670352c457943666fe639
    Sharing dat: 2 files (1.4 MB)
    Serving files over http at http://localhost:8080
    
    2 connections | Download 0 B/s Upload 0 B/s

现在访问[http://localhost:8080](http://localhost:8080)在浏览器中查看文件！默认端口是8080.你应该会看到一个文件目录：

![](https://docs.datproject.org/assets/cli-http.png)

如果被分享的dat/文件夹中有一个`index.html`，那么访问`localhost:8080`将会呈现文件夹中的index.html。

## **内置的版本化系统**
正如你所知，Dat会自动将所有文件版本化，HTTP是查看版本历史记录的一种简单方法

试试访问[localhost:8080/?version=2](localhost:8080/?version=2)查看一个特定的版本。

## **热加载**
Dat http查看器还带有实时重新加载功能。如果它检测到一个新版本，它将会自动地重新加载新的文件目录或页面（只要你不是正在查看特定的版本）

## **稀疏下载（Sparse Downloading）**
Dat支持稀疏下载或部分下载。如果你只想从一个大型Dat中获得一个文件，这是非常有用的。不幸的是，我们还没有为这个应用程序构建用户界面。

这将允许你从较大的Dat中下载单个文件，而无需下载元数据或任何其他文件。

首先，开始我们的下载演示，请确保你的命令中包含--http和--sparse

    ❯ dat dat://778f8d955175c92e4ced5e4f5563f69bfec0c86cc6f670352c457943666fe639 ./demo --http --sparse
    Cloning: 2 files (1.4 MB)
    Serving files over http at http://localhost:8080
    
    3 connections | Download 0 B/s Upload 0 B/s

--sparse选项告诉Dat只下载你想要的文件。看一下它是如何工作的：
1. 检查你的./demo文件夹，它应该是空的
1. 在你的浏览器中打开Dat
3. 单击要下载的文件
4. 现在它应该在你的文件夹中了！

非常酷！你可以使用这个方法下载特定的文件甚至是旧版本的文件（如果它们被保存在了某些地方的话）

