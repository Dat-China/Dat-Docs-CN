# 开始使用Dat

这是一个针对Dat命令行工具的教程。如果你不使用命令行，不要担心，有桌面应用让使用Dat分享和下载数据变得简单。任何人使用Dat应用都可以工作，不管他们使用的是哪一个应用。下载桌面应用

在这个教程中我们将会介绍使用Dat共享数据与下载数据的两种主要方法。如果可能的话，和伙伴一起使用Dat来了解Dat是如何在电脑之间工作的是极好的。

## *特点*
* **安全**-Dat加密数据传输并且在内容到达时校对。Dat阻止第三方访问元数据和内容。了解更多关于[安全与隐私]()。
* **分布式的**-直接与共享或下载公共数据集的其他用户连接。任何设备都可以分享文件而不需要集中式服务器。[阅读更多]()关于分布式网络。
* **快速**-快速分享文件与本地存档。只下载你想要的文件。通过仅下载新数据快速同步更新，节省时间和带宽。
* **透明**-一个完整的版本历史提高透明度和可审计性。更改被记录在append-only的日志中并且在整个网络中统一共享。
* **未来证明**-持久的连接识别与内容校验。这些独一无二的id允许用户托管副本，在不牺牲来源的情况下提高了长期可用性。

## 安装Dat
使用

    npm install -g dat 
在命令行中安装Dat。想要获取更多信息，请看[安装页面]()。

## 下载数据
与git很相似，你通过运行 
*dat clone \<link>*
来下载某人的dat。dat链接就像一个http://链接，但是具有特殊的属性。
作为例子，我们创建了一个你可以下载的dat。它仅仅包含一些小文件。

    dat clone dat://778f8d955175c92e4ced5e4f5563f69bfec0c86cc6f670352c457943666fe639 ~/Downloads/dat-demo
![dat clone]()

这会将我们的样例文件下载到
*~/Downloads/dat-demo*
文件夹。这些文件被一个服务器通过Dat分享（以确保高可用性）。当你下载数据，你可能也会连接到任意数量的正在运行dat的用户。越多的用户正在运行dat，下载就越快。
你也可以在线查看这些文件：  
[https://datproject.org/778f8d955175c92e4ced5e4f5563f69bfec0c86cc6f670352c457943666fe639](https://datproject.org/778f8d955175c92e4ced5e4f5563f69bfec0c86cc6f670352c457943666fe639/)
Datproject.org可以在浏览器浏览一个dat--只要有人再托管它。该网站会暂时缓存任何被访问连接的数据（如果你不希望我们缓存你的数据，请不要再datproject.org上查看你的dat）。

## 创建一个Dat
现在，让我们分享一些数据并且从你电脑上的一个文件夹创建一个dat。

在你的电脑上找到一个文件夹来分享。任何种类的文件都适用于dat但目前请确保它是你想要共享的文件。Dat可以处理所有类型的文件（Dat也适用于非常大的文件夹！）。我们喜欢猫的照片。

首先，你可以在那个文件夹里面创建一个新的dat。使用
*dat create*
命令初始化dat并且允许我们提供一些信息以便于其他人和应用程序可以很轻易地展示dat里面有什么。
    
    mkdir MyData
    cd MyData
    dat create
    > Title: My Amazing Data
    > Description This is a dat

    Created empty Dat in /Users/me/MyData/.dat
这将会创建一个新的（空的）dat。创建了一个名为.dat的文件夹，其中包含一组元数据文件，这些元数据文件保持dat在同步。要想了解这些文件是什么，阅读[Overview](https://docs.datproject.org/overview)或阅读[Dat论文](https://docs.datproject.org/paper)。

## 分享数据
你的dat已经被创建了，现在是时候扫描并将数据同步给其他人了。在这个文件夹里面运行以下指令

    dat share
![dat share](https://raw.githubusercontent.com/datproject/docs/master/assets/cli-share.gif)

只要该进程正在运行，你就可以将链接分享给你的朋友并且他们可以立即开始下载你的文件。

如果你不希望别人下载你的dat，你可以发送给他们一个链接并且他们可以特们可以在浏览器查看内容。到[https://datbase.org/]()并且在左上角输入你的链接进行预览。（有些用户，包括正在写这个文档的我，可能都会在最初连接到datproject.org时遇到困难。这可能会花费一些时间初始化链接，但是如果你等待并且刷新，它应该会呈现那些文件。我们正在积极地改善这个现象。谢谢）

## 让你的数据保持活跃
只要你的进程是开着的，你的数据就是可用的。人儿如果你需要关闭你的笔记本电脑，你可能想要在一个服务器上长期托管你的dat。

首先，你需要自己买一个服务器，我们推荐你使用[Digital Ocean](https://digitalocean.com/)，或者在你的家里面配置一个[dat仓库](https://github.com/datproject/datasilo)。   

在你拥有了一台可用的服务器以后。你就可以去看[如何在服务器上部署dat](https://docs.datproject.org/server)了。

你也可以使用这个[免费服务器](https://hashbase.io/)来保持数据活跃。