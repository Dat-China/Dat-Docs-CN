[guide_on_fixing_npm_permissions]: https://docs.npmjs.com/getting-started/fixing-npm-permissions
[troubleshooting_section]: https://docs.datproject.org/troubleshooting
[open_an_issue]: https://github.com/datproject/dat/issues/new
[chat_room]: https://gitter.im/datproject/discussions

# **问题分析**
我们已经基于用户遇到的问题提供了一些问题解决办法。如果你需要帮助解答这里没有提及的疑难问题，请创建问题或者在我们的聊天室询问我们。

## **检查你的版本**

如果你遇到了任何错误，了解版本对于你解决问题和我们帮助你解决问题都是很有帮助的。

**在Dat桌面应用中：**  
在左上角的菜单栏点击**Dat** > **About Dat**  
**在命令行中：**
    
    dat - v
    
你应该会看到输出的Dat版本信息，比如13.1.2。

## 网络问题

所有的数据传输都是直接在不同计算机之间进行的。尽管Dat有各种各样连接到别的计算机的方法，但是因为联网能力参差不齐，我们还是可能会遇到联网问题。
每次你运行Dat时，通过下面三个步骤就可以与其他节点分享、下载文件了。

1. 发现其他资源
2. 连接到其他资源
3. 发送 & 接收数据

连接成功后，Dat会在连接后显示节点数。如果你一直看不到其他用户的连接，那么你的网络可能被限制了连接和发现。你可以试着在两台无法互联的电脑使用`dat doctor`。这可能能帮助你修复网络。

### Dat Doctor

我们附带了一个诊断Dat网络问题的工具——Dat doctor。Dat doctor会运行两项测试：

1. 尝试连接一个正在运行Dat的公共服务器。
2. 尝试直接在两台计算机之间建立连接。你可能要在几台电脑上同时运行Dat doctor命令。

**在桌面应用：**

我们的桌面应用Dat doctor还在构建中。现在你只能进行连接到我们的公共服务器的测试。
1. View > Toggle Developer Tools
2. Help > Doctor

你应该会看到控制台中输出的doctor信息。

**在命令行中：**

运行`dat doctor`打开doctor。

在直接连接测试中，doctor会输出一个命令，你需要在其他计算机中运行那个命令`dat doctor <64位哈希值>`
doctor会测试分享数据时的关键步骤来定位问题。

### 已知的网络连接问题

* 如果你正在使用iptables，你的Dat可能会遇到网络连接[问题](https://github.com/datproject/dat/issues/503)。

## 安装问题的处理

你必须安装好node和npm才能使用Dat命令行工具。
Dat只支持node 4及以上的版本。

    node -v
    
###全局安装

-g选项会全局安装Dat，这样你就可以将dat当做一个命令来使用。
确保你安装时附带了 -g 选项。

* 如果你收到EACCES错误，请阅读这个[有关修复npm权限的教程][guide_on_fixing_npm_permissions]或者使用`sudo npm install -g dat`来安装。

* 如果你在安装Dat时还碰到了其他问题，你可以看一看[疑难解答部分][troubleshooting_section]，或者在github上[open an issue][open_an_issue]或者在我们的[chat room][chat_room]中向我们提问。

## 命令行调试

如果你在使用某个命令时遇到问题，你可以为dat设置调试环境变量后再运行。这可以帮助我们调试任何问题。

    DEBUG=dat,dat-node dat clone dat://<link> dir
