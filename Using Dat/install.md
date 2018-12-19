[guide_on_fixing_npm_permissions]: https://docs.npmjs.com/getting-started/fixing-npm-permissions
[troubleshooting_section]: https://docs.datproject.org/troubleshooting
[open_an_issue]: https://github.com/datproject/dat/issues/new
[chat_room]: https://gitter.im/datproject/discussions
[]:
[]:
# 安装
Dat拥有一个桌面客户端，一个命令行工具，和一个node.js库，如果你想阅读有关Dat如何工作的内容，请阅读 它是如何工作的 ，如果你还想学习更多请阅读Dat白皮书

有问题或需要一些指导？你可以在在#dat或Gitter上通过IRC与我们交谈！

## 桌面应用
如果你不想使用终端，你可以在Mac或Linux上使用我们的桌面应用（Windows版即将到来）。

**平台**|**链接**
---|---
Mac|Download .dmg
Linux|Download .AppImage
Windows|即将到来

## 在终端
Dat可以在命令行中使用node安装。按照以下说明开始安装。

**安装Node。**
Dat要求Node版本为4.0或者更高；然而，我们推荐最新版本。如果你没有node，到他们的官网nodejs.org并选择你的平台。如果node已安装，你应该可以输入下面内容来查看你所拥有的node的版本。
    
    $ node -v
    8.0.0
**安装Dat**
    
    npm install -g dat
如果dat被成功安装，你可能会看到像这样的输出

    /usr/local/bin/dat -> /usr/local/lib/node_modules/dat/bin/cli.js

    > utp-native@1.5.1 install /usr/local/lib/node_modules/dat/node_modules/utp-native
    > node-gyp-build
    
    
    > sodium-native@1.10.0 install /usr/local/lib/node_modules/dat/node_modules/sodium-native
    > node-gyp-build "node preinstall.js" "node postinstall.js"
    
    added 321 packages in 9.662s
如果你收到EACCES错误，请阅读这个[有关修复npm权限的教程][guide_on_fixing_npm_permissions]或者使用`sudo npm install -g dat`来安装。

如果你在安装Dat时还碰到了其他问题，你可以看一看[疑难解答部分][troubleshooting_section]，或者在github上[open an issue][open_an_issue]或者在我们的[chat room][chat_room]中向我们提问。

## 下一步

你已经整装待发。进入下一页来开始分享数据吧。