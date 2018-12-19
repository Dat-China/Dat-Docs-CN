# **发布一个Dat**
默认情况下，Dat是私有的，但是如果你想和全世界分享你的dat或者在你的组织中分享你的dat呢？有很多方法可以做到这一点，我们将在下面具体描述。

## **创建 dat.json 文件**

当引用一个dat时，你希望有一种方法可以确定这个dat是什么，就像一个标题，描述和url。
为了做到这一点，我们建议在你的dat存档中创建一个`dat.json`文件和一个README文件。`dat create`命令会默认推荐创建一个`dat.json`文件。

    $ dat create
    Welcome to dat program!
    You can turn any folder on your computer into a Dat.
    A Dat is a folder with some magic.
    
    Your dat is ready!
    We will walk you creating a 'dat.json' file.
    (You can skip dat.json and get started now.)
    
    Learn more about dat.json: https://github.com/datprotocol/dat.json
    
    Ctrl+C to exit at any time
    Title: More Tweets, More Votes
    Description: When you tweet more, more votes happen.

This will generate a dat.json file which you can use to reference your dat in the future. For example:

这会生成一个dat.json文件，你可以使用它来引用你的dat。  
例如：

dat.json

    
    {
      "url": "dat://96cf4957539aff4fc856fa0804e613181064ed193e5f7882c9623ec7bed38deb",
      "title": "More Tweets More Votes",
      "description": "An analysis of tweets from the peroid before 2012 U.S. House election combined with census data."
    }

Dat可以读这些文件来了解如何下载与编目你的Dat。这是最简单的版本，但是如果你想要的话它们也可以变得更复杂。你可以通过这个文件在其他应用中（比如registry、数据门户或者git仓库）引用Dat。

我们已经构建了一个web服务器，允许你创建账户并且建立你的数据集列表，使用dat.json来描述它们的名字和标题等，这样就可以方便地与公众分享你的数据。
注册点不负责长期托管你的数据，注册仅仅是更方便地找到可用dat的一种方法。如果你对dat registry和torrent tracker都很熟悉的话，你会发现它们是很相似的。

我们已经托管了一个全球公用的注册处[https://datproject.org](https://datproject.org)。首先，你要先创建一个账户。（需要[科学上网](https://www.baidu.com/s?wd=科学上网)）

    $ dat register
    Welcome to dat program!
    Create a new account with a Dat registry.
    
    Dat registry:  (datproject.org)

默认情况下，dat将使用datproject.org作为注册点。如果你想要使用你自己的注册点，你可以输入你自己的服务器名。一旦你拥有了一个账户，你就可以使用dat publish将你的dat发布到注册点。

首先，**dat publish**）。例如，more-tweets-more-votes将会变成 （我的用户名）/more-tweets-more-votes

    cd /path/to/my/dat
    $ dat publish
    Name: more-tweets-more-votes
    Published successfully!
    
一旦dat被发布，你就可以访问它，例如
[https://datproject.org/karissa/more-tweets-more-votes](https://datproject.org/karissa/more-tweets-more-votes)

为了让任何人都能看到dat，您需要在当前的dat中运行**dat share**命令。如果你想关掉你的桌面，我们建议你设置一个服务器来托管你的数据。

# **Using Git with**dat.json
你可能希望通过dat实现数据快速下载、版本控制、点对点连接、数据去重的同时能够获取git的协作优势；那么你要做的全部事情就是将dat.json包含在你的git仓库中并且告诉人们使用
**dat clone .**
命令来获取你的最新版本数据。现在，当有人clone你的git仓库时，他们只需输入
**dat clone .**或dat clone dat.json，然后dat.json中的url便会被用来下载dat。

为了节省你的git仓库空间，你可能想要把.dat添加到你的.gitignore文件中来确保不包含任何dat的元数据。Dat默认情况下会忽略.git文件夹，因此你不必担心这个问题。