---
title:  "【区块链】智能合约"
subtitle: "构建基于以太坊智能合约的 ICO DApp 之进阶"
author: "wu"
avatar: "img/authors/tigerCat.jpg"
image: "img/timg02.jpg"
date:   2018-12-01 03:00:00
---

智能合约 - 构建基于以太坊智能合约的 ICO DApp - Advanced

如果对本篇涵盖的产品策划、技术方案、详细代码感兴趣，可以加微信（微信号：superloiwu，加好友请备注：blockchain-beginner），共同探讨。

----- ----- ----- -----

## Part1 自建智能合约工作流的动机和目标



### 1-Remix 的局限性

Remix 在智能合约的开发、编译、部署、测试上给我们提供了便利，如果想快速测试想法、编写智能合约原型，用 Remix 效率自然很高，但纯粹使用 Remix 做智能合约开发有明显的局限：

1、源代码没有版本控制，实际上不存在编写一次就不用修改的代码，没有版本控制就是取乱之道；

2、智能合约只能在浏览器中运行，比如我们不能方便的把合约部署到以太坊的主网或测试网络上；

3、智能合约的测试都是手动的，手动的过程通常是不可靠的，如果有自动化的智能合约测试就好了；


### 2-Truffle 的困扰

当然，这些问题并不是只有我们会经历、会想到，社区中的工程师们其实很早就开始尝试解决这些问题了，开始接触以太坊开发之后，或早或晚，你都会在各种教程和文档里面看到 Truffle，Truffle 对自己的定位是以太坊开发的瑞士军刀，原文如下：

<pre>

Truffle is the most popular development framework for Ethereum with a mission to make your life a whole lot easier.

</pre>

就好比前端领域的各种全家桶种子项目（create-react-app、vue-cli），帮你把各种关键模块（react、webpack、vue、eslint）组装好，你只需要在上面开发应用逻辑即可，但对于刚入门区块链和以太坊的同学来说，如果上来就学 Truffle，接触到的新概念、新术语会更多，如果动手练习时出了问题通常会束手无策，入门的速度也会明显被拖慢。

<div class="scale"><img src="img/resources/blockchain/blockchaintruffle.png"  alt="blockchaintruffle" /></div>

从长远角度讲，我们需要达到既知其然又知其所以然的地步才能够驾驭新技术新工具，而深入理解一个东西最好的方式就是自己动手将其关键的部分拼凑起来，就像是在搭建自己的机器，当你把机器组装起来、使之运转良好之后，对它的运转流程自然了然于胸。

参考：

[《Truffle》](https://truffleframework.com/)

### 3-工作流目标

本着上面的考量，在接下来我们开始熟悉在 Node.js 开发环境中使用的必要工具，实现自己的智能合约编译、部署、测试工作流：

<div class="scale"><img src="img/resources/blockchain/smartcontractworkflow.png"  alt="smartcontractworkflow" /></div>

其中 3 个红色圆圈的标记是需要重点要解决的，具体来说：

1、要能够把 Solidity 源代码编译成 Bytecode 和 ABI（使用工具 solc）；

2、要能够把编译后的 Bytecode 部署到本地测试网络（ganache-cli）、公共测试网络（rinkeby）（用到 web3.js）；

3、要能够在单元测试中和部署完的智能合约实例交互，（组合使用 web3.js 和 mochajs）；

<div class="scale"><img src="img/resources/blockchain/smartcontractrinkeby.png"  alt="smartcontractrinkeby" /></div>

其中 ganache-cli 是 Truffle 框架的一部分，能够让开发者快速启动本地测试网络，而不需要大费周章的在本地运行以太坊节点，而 web3.js 在合约部署和自动化测试时会被大量使用，这也会为我们后续开发 DApp 打下良好的基础。

在做完上面这些事情之后，我们会重新造访 Remix，看看我们如何在 Remix 中加载和测试使用脚本部署的智能合约实例。

参考：

[《solc》](https://www.npmjs.com/package/solc)

[《ganache-cli》](https://www.npmjs.com/package/ganache-cli)

[《rinkeby》](https://www.rinkeby.io/#stats)

[《web3.js》](https://www.npmjs.com/package/web3)

[《mochajs》](https://www.npmjs.com/package/mocha)

### 4-环境准备

接下来我们需要开始写代码了，请确保你具备如下开发环境：


1、Git 环境，系统中都有安装 Git，并且会使用 Git 的基本命令；

2、Node.js 运行环境，最好是 v8.x 以上版本，建议使用 nvm 来安装；

3、好用的前端包管理器，比如 Yarn，后文中的 yarn 可以和 npm 互换使用；

4、可以用来输入和执行命令的终端程序，比如 Mac 下的 iTerm，或者 Windows 下的 cmder；

5、用起来舒服的编辑器，比如 VSCode，我写过两篇 VSCode 编辑器配置的文章，参见上和中；

VSCode，默认是不支持 Solidity 语法高亮，可以考虑安装插件 Solidity，或者 Solidity Extended，如果你使用了其他编辑器，就需要自己去找找对应的语言插件了，如果有这个需求的同学可以参见 awesome-solidity 里面的编辑器插件部分。

前端开发环境中常见的 Lint 工具，Solidity 语言下也是有的，如果在 VSCode 中安装了 Solidity 插件，会自带 Solium 代码检查工具，如果要调整 Solium 的配置，先打开 VSCode 的用户配置，在其中搜索 Solidity，修改 solidity.soliumRules 里面的缩进配置保存即可，当然保存代码风格配置之后，你还需要手动调整代码里面的缩进使之符合规范，因为 prettier 还不支持 Solidity 代码的格式化，

参考：

[《能让你开发效率翻倍的 VSCode 插件配置（上）》](https://juejin.im/post/5a08d1d6f265da430f31950e)

[《能让你开发效率翻倍的 VSCode 插件配置（中）》](https://juejin.im/post/5ad13d8a6fb9a028ce7c0721)

[《Solidity》](https://marketplace.visualstudio.com/items?itemName=JuanBlanco.solidity)

[《Solidity Extended》](https://marketplace.visualstudio.com/items?itemName=beaugunderson.solidity-extended)

[《awesome-solidity》](https://github.com/bkrem/awesome-solidity)

[《Solium》](https://github.com/duaraghav8/Ethlint)

[《prettier-plugin-solidity》](https://github.com/prettier-solidity/prettier-plugin-solidity)

### 5-项目准备

开始写代码之前，我们需要先初始化一个空目录：

<pre>

cd ~/Develop
mkdir ethereum-contract-workflow
cd ethereum-contract-workflow

</pre>

然后初始化 package.json 和 git 仓库

<pre>

npm init -y
git init

</pre>

然后在仓库根目录下新建 .gitignore 文件，文件内容如下：

<pre>

node_modules

</pre>

这样前端项目的 node_modules 目录就不需要提交在 Git 仓库中了。

在继续之前，把初始化的工作提交。

<pre>

git add .
git commit -m 'initial commit'

</pre>

### 目录结构

为了分门别类的存放代码文件，我们在项目根目录下下面新建 4 个子目录：

<pre>

mkdir contracts
mkdir scripts
mkdir compiled
mkdir tests

</pre>

其中 contracts 目录存放合约源代码，scripts 目录存放我们的编译、部署脚本，complied 目录存放编译结果，tests 目录存放单元测试。最后我们的目录结构如下：

<pre>

.
├── .gitignore
├── compiled
├── contracts
├── package.json
├── scripts
└── tests

</pre>

好，准备好了地基，接下来我们会在地基上开始建房子。

<div class="scale"><img src="img/authors/wechatloi.jpg"  alt="wechatloi" /></div>



