---
title:  "【区块链】区块链和以太坊"
subtitle: "构建基于以太坊智能合约的 ICO DApp 的前传"
author: "wu"
avatar: "img/authors/tigerCat.jpg"
image: "img/timg02.jpg"
date:   2018-11-30 03:00:00
---

区块链入门 - 构建基于以太坊智能合约的 ICO DApp - Beginner

如果对本篇涵盖的产品策划、技术方案、详细代码感兴趣，可以加微信（微信号：superloiwu，加好友请备注：blockchain-beginner），共同探讨。

----- ----- ----- -----

## Part1 区块链简明发展史

区块链简明发展史：什么是BTC大饼？Ethereum姨太？EOS柚子？

### 比特币

区块链技术起源于 2008 年 10 月 31 日 中本聪（Satoshi Nakamoto）发表的比特币论文：《Bitcoin: A Peer to Peer Electronic Cash System》，即比特币白皮书，该论文论述了基于 P2P 网络的电子现金系统的设计，系统中产生和流通的现金就是比特币（Bitcoin）。

参考：

[《比特币白皮书》](https://bitcoin.org/bitcoin.pdf)

[《区块链钱包用户数》](https://www.blockchain.com/charts/my-wallet-n-users)

[《区块链模型》](https://mp.weixin.qq.com/s/ifbakeqsqf7mI8pdwleSzA)

### 以太坊

比特币网络的功能仅限于让用户进行财务交易(Financial Transactions)，比特币创意中的区块链技术可以进行金钱之外的价值交换。Vatalik Buterin（天才程序员"V神"）于 2013 年末发布了以太坊的白皮书：《Ethereum: The Ultimate Smart Contract and Decentralized Application Platform》。

相比比特币网络的简洁，Vatalik 提出了基于区块链技术创建更加复杂的去中心化应用（Decentralized Application，简称为 DApp）。

以太坊内建的智能合约（Smart Contract），是存储在以太坊网络上的代码片段，可以通过发送消息与之交互。

基于以太坊智能合约发行遵循 ERC20 规范的数字货币，这个过程被称为首次代币发行（Initial Coin Offering），简称 ICO，这给区块链企业融资提供了极大的便利。ICO 过程不受监管、且完全中心化。

2017 年底以太坊创始人 Vatalik Buterin 提出 DAICO 的理念，募集的资金会被智能合约锁定在账户里，区块链创业团队在资金支出时先提出请求，多数投资者投票同意后，募集来的资金才可以被支出，这在很大程度上保障了投资者的利益，也是在鼓励创业团队踏实做事。

参考：

[《以太坊白皮书》](https://github.com/ethereum/wiki/wiki/White-Paper)

[《以太猫》](https://www.cryptokitties.co/)

[《State of DApps》](https://www.stateofthedapps.com/)

[《以太坊用户数》](https://etherscan.io/chart/address)

[《DAICO》](https://ethresear.ch/t/explanation-of-daicos/465)

### 区块链

区块链技术的发展分为 3 个阶段：

1）区块链 1.0，以比特币为代表（大饼），仅支持转账，类似于刚刚诞生的万维网，网页只支持静态内容展示、链接等；

2）区块链 2.0，以以太坊为代表（姨太），能够在转账基础上支持一定复杂度的业务逻辑定制（智能合约），类似于有了 JS 的万维网，即在静态展示的基础上自定义动态内容，但是浏览器的 JS 执行效率还比较弱，不能在浏览器做非常复杂的事情；

3）区块链 3.0，以各种高性能底层公链为代表，比如 EOS（柚子），是目前整个社区努力的方向，开发生态的成熟还需要时间，能够支持高并发商业应用的运行，类似于为能各种复杂的端应用提供运行环境的现代浏览器（如 Chrome等）；

<div class="scale"><img src="img/resources/blockchain/blockchainroad.png"  alt="blockchainroad" /></div>

参考：

[《比特币 bitcoin》](https://github.com/bitcoin/bitcoin)

[《以太坊》](https://www.ethereum.org/)

[《EOS》](https://eos.io/)

## Part2 区块链核心概念和原理

区块链核心概念和原理：什么是账户？交易？区块？区块链？块高度？出块时间？

### 账户？交易？区块？区块链？

比特币是最早最成功的区块链应用，它类似于基于区块链技术实现的银行系统，负责记账和发行货币。用银行账本中的概念来类比理解区块链中的概念：

1）账户（Account）是用户在银行的户头+密码的组合，在区块链世界中也是如此。无论比特币还是以太坊的账户都由地址、公钥、私钥 3 部分构成。地址相当于用户名，公钥+私钥相当于密码。私钥的丢失或者泄露意味着失去账户（敏感信息、资金）的控制权；

2）交易（Transaction）是账本中的任意一条收支记录，在区块链世界中可以指两个账户之间的转账交易，或智能合约的调用请求；

3）区块（Block）是账本中的一页，账本的每页可能包含多笔收入和支出，区块链中的每个区块都可能包含多笔交易；

4）区块链（Blockchain）是装订成册的多页账本，账本的不同页按照记录时间先后顺序组织，区块链中不同区块按被矿工打包的时间先后组织。

区块链技术通常被简单描述为：公开的、分布式、不可篡改的数据库技术或记账技术，知识点包含区块如何被生产，如何实现不可篡改的机制、交易的有效性验证及密码学知识。

参考：

[《区块链核心知识在线学习 Blockchain Demo 》](https://anders.com/blockchain/)

### 块高度？出块时间？

长时间记录的账本都会是多页的，在区块链世界也是如此，长时间运行的区块链网络记录下来的数据量也是巨大的。

块高度可理解为账本的页数，在区块链世界里，块高度可以理解为自该区块链开始运行到现在共产生了多少个区块，可以认为块高度就是区块链世界的时间，每产生一个新的区块，块高度就会加 1。

出块时间可以类比为记完每页账，并在上面摁完手印儿需要花费多少时间，在区块链世界里，即区块链上相邻两个区块产生出来的时间间隔，或者常说的交易确认时间（Transaction Confirm Time，注意这里说的交易确认时间指某笔交易从发起到被打包进区块链的时间）。

比如比特币的出块时间通常控制在 10 分钟，以太坊的出块时间在 15 秒左右。

任何打包的交易在分布式网络上达成共识都需要时间，共识算法决定了时间的长短，共识算法包括POW、POS、DPOS。

参考：

[《比特币区块链数据 Blockchain Size》](https://www.blockchain.com/charts/blocks-size)

[《以太坊区块链数据 Ethereum ChainData Size》](https://etherscan.io/chart2/chaindatasizefast)

[《比特币最新块高度》](https://blockexplorer.com/)

[《以太坊最新块高度》](https://etherscan.io/blocks)

[《比特币出块时间 Median Confirmation Time》](https://www.blockchain.com/charts/median-confirmation-time)

[《以太坊出块时间 Ethereum Average BlockTime Chart》](https://etherscan.io/chart/blocktime)

### 部分核心概念的串联

<div class="scale"><img src="img/resources/blockchain/blockchainconcepts.png"  alt="blockchainconcepts" /></div>

参考：

[《比特币词汇表 Vocabulary 》](https://bitcoin.org/en/vocabulary)

[《比特币开发者术语表 Developer Glossary》](https://bitcoin.org/en/developer-glossary)

## Part3 以太坊核心概念和原理

以太坊核心概念和原理：什么是以太坊网络？主网？测试网？其他网？智能合约？

### 以太坊网络

从整体和个体两个视角来理解什么是以太坊网络：

一、整体视角，以太坊网络本质是 P2P 网络系统，其用途是发起交易、存储交易历史，交易可以是转账或调用智能合约中的方法，以太坊区块链是存储了以太坊网络上发生过的每笔交易的数据库。常说的以太坊网络通常情况下指的是主网，社区中还存在很多用途各异的以太坊网络，可将其归类如下：

1）主网：Mainnet，是以太坊的线上环境，记录、保存用户和智能合约的交易，主网中存储的代币才具有真正的价值；

2）测试网：Testnet，是以太坊的测试环境，方便社区和开发者测试智能合约、转账等功能，典型的测试网络有 Rinkeby、Ropsten、Kovan，其中的代币不具有任何价值；

3）其他网：是以太坊的开发环境，包括通过开发者在本地运行以太坊节点组成，通过使用各种便捷的工具启动的本地测试网，以及以内部测试为目的而搭建的私有网络等。随着以太坊区块链数据的累计，运行全节点的时间和硬件成本也越来越大。

<div class="scale"><img src="img/resources/blockchain/blockchainnodes.png"  alt="blockchainnodes" /></div>

二、个体视角，P2P 网络通常包含多个节点，每个节点都需要运行以太坊客户端。任何人都可以运行以太坊节点，每个以太坊网络上的节点都包含了以太坊区块链数据库的整体副本，每个以太坊网络节点都可以接收 RPC 交易请求并将请求广播给网络中的其他节点，每个以太坊节点都会尝试进行交易的校验、打包（挖矿），即区块生产的任务，生产出的区块也会被广播给其他网络节点。

参考：

[《以太坊测试网列表》](testnet.etherscan.io)

### 如何与以太坊网络交互？

交互的具体定义：向以太坊网络发送转账请求，或调用智能合约函数，这是发起读取或修改以太坊区块链上数据的请求，并拿到反馈的过程。

类似于现有世界中支持开发者自行开发应用的平台，如微博开发平台、微信小程序等，可以把与平台交互的用户分为两大类：开发者、普通用户，两者与平台本身的交互方式、交互途径不同。

微信小程序生态下，开发者、普通用户、平台之间的交互方式可简化为下图：

<div class="scale"><img src="img/resources/blockchain/blockchainwechatplatform.png"  alt="blockchainwechatplatform" /></div>

以太坊生态中，开发者、普通用户、平台之间的交互方式可简化为下图：

<div class="scale"><img src="img/resources/blockchain/blockchainethereumplatform.png"  alt="blockchainethereumplatform" /></div>

其中的 web3.js 是工程师通过代码和以太坊网络交互的桥梁，而实际的 DApp 测试离不开使用钱包（如Metamask ）的使用。

参考：

[《web3.js》](https://github.com/ethereum/web3.js)

### 智能合约

智能合约（Smart Contract）指以太坊网络上被代码控制的一个账户。

不同于我们使用各种钱包软件创建的账户（由创建账户的用户来控制），智能合约对应的账户是由代码控制的，其他账户（包括智能合约账户、普通用户账户）可以通过交易（Transaction）的方式与智能合约账户交互。

社区中也会把智能合约账户称为内部账户，而普通用户账户称为外部账户。两种账户的关系可以用下图描述：

<div class="scale"><img src="img/resources/blockchain/blockchainaccounts.png"  alt="blockchainaccounts" /></div>

常常用智能合约指代智能合约源代码或部署在以太坊网络上的智能合约账户。

如果把智能合约源代码比作设计图，那么智能合约账户就是根据蓝图造出来的车或者建筑。

相同的源代码可以部署到不同的网络上，或者在相同的网络上部署很多次，都会产生不同的智能合约地址。

整个过程可以用下图示意：

<div class="scale"><img src="img/resources/blockchain/blockchaindeploy.png"  alt="blockchaindeploy" /></div>

无论部署到哪个网络，源代码是完全相同的，而部署所产生的智能合约账户只存在于其被部署到的那个网络。比如部署到 Mainnet 主网的智能合约不能通过 Rinkeby 测试网络去访问，反之亦然，普通账户则在不同网络间是通用的。

智能合约账户有以下关键属性：

1）balance，即该智能合约账户所控制的资产余额。如某个抽奖智能合约中奖池的资金；

2）storage，智能合约的相关数据存储在这里，可粗暴的将其看做是 DApp 的数据库。如抽奖智能合约里面存储参与人的地址；

3）code，智能合约的字节码，由智能合约源代码编译而来，存储在区块链上方便任何节点接受智能合约的函数调用；

## Part4 使用 Metamask 创建第一个以太坊 HD 钱包

Talk is cheap, show me the code. BUT, 在开始编写代码之前，需要先确保具备调试的基本条件，准备好开发环境。

接触任何区块链网络都需要有自己的账户，管理账户的软件称之为钱包。在创建钱包和账户之前，先了解下以太坊网络中账户的组成：

<div class="scale"><img src="img/resources/blockchain/blockchainethereumaccount.png"  alt="blockchainethereumaccount" /></div>

上图展示的以太坊网络中的账户，由地址、公钥、私钥 3 部分构成，不论使用何种钱包创建的以太坊账户，在不同的以太网网络之间都可以通用。这种跨网络通用的账号机制是内置在以太坊客户端之内的。

在以太坊网络上发起转账交易、部署智能合约、调用智能合约中的函数，都需要有账户，方便以太坊记录和验证，谁、在什么时间、做了什么。

区块链世界里面的钱包借鉴自现实世界的钱包。

现实世界里，每个人都有自己的钱包，钱包里面放了很多张银行卡，一个人在某家银行可能会办多张银行卡。对应到区块链世界里面，可以认为每张银行卡对应一个账户。每家银行对应一个区块链网络，能管理所有银行卡的软件就叫做钱包，对应关系如下图：

<div class="scale"><img src="img/resources/blockchain/blockchainwallet.png"  alt="blockchainwallet" /></div>

### 钱包运行环境

钱包 Metamask 就是个浏览器插件，虽然 Metamask 能运行在多个浏览器里面，但考虑到 Solidity 调试工具运行环境、前端调试工具的丰富程度、浏览器性能、市场份额占比等要素，建议安装 Google Chrome 的较新版：

<div class="scale"><img src="img/resources/blockchain/blockchainchrome.png"  alt="blockchainchrome" /></div>

### 创建钱包和账户

创建以太坊账户的方式有很多种，要开发跑在浏览器中的 DApp，如果钱包集成在浏览器中就非常方便了，Metamask 提供了非常便捷的以太坊浏览器钱包插件，可以再 Google Chrome WebStore 中下载该插件。

安装浏览器插件的时候要认准 Metamask 如下的 Logo：

<div class="scale"><img src="img/resources/blockchain/blockchainmetamask.png"  alt="blockchainmetamask" /></div>

Chrome浏览器安装完 Metamask 之后，浏览器上地址栏右侧会出现如下图红框中的图标：

<div class="scale"><img src="img/resources/blockchain/blockchainmetamaskinstalled.png"  alt="blockchainmetamaskinstalled" /></div>

单击狐狸图标，会弹出如下图的安全提示信息，大意为：创建钱包账户之后，账户地址是公开的，第三方可以查看其中的余额以及交易历史等信息。

<div class="scale"><img src="img/resources/blockchain/blockchainmetamask1.png"  alt="blockchainmetamask1" /></div>

点击 "ACCEPT"，进入如下的使用条款页面：

<div class="scale"><img src="img/resources/blockchain/blockchainmetamask2.png"  alt="blockchainmetamask2" /></div>

使用条款页面的 "ACCEPT" 按钮开始是灰色的，必须滚动条款到底部，才会变成可点击的，如下图：

<div class="scale"><img src="img/resources/blockchain/blockchainmetamask3.png"  alt="blockchainmetamask3" /></div>

继续点击 "ACCEPT"，进入钱包密码填写页面，至少填写 8 个字符：

<div class="scale"><img src="img/resources/blockchain/blockchainmetamask4.png"  alt="blockchainmetamask4" /></div>

密码填写完之后，点击 "CREATE"，创建账户，接下来弹出钱包的助记词界面：

<div class="scale"><img src="img/resources/blockchain/blockchainmetamask5.png"  alt="blockchainmetamask5" /></div>

这里的助记词是用来生成账户的公钥和私钥的，也是用来恢复钱包里面的所有账户的。

如果创建的是存放真实资产的账户，千万不要把助记词泄露给别人！千万不要把助记词泄露给别人！千万不要把助记词泄露给别人！因为无论谁掌握了你钱包的助记词，就相当于掌握了你钱包里所有账户的私钥，钱包里面的资产就不再属于你自己了，别人可以随时转走。

从密码学角度看，地址、公钥、私钥本质都是非常非常大的数字。无论转成 16 进制或者 base58 格式，对普通用户来说，都是杂乱无章的随机字符串。比特币社区提出了 BIP39 的提议，帮助区块链用户记住这些数字，技术上该提议可以在任意区块链中实现。

比如使用完全相同的助记词在比特币和区块链上生成的地址可以是不同的，用户只需要记住满足一定规则的词组（即“助记词”），钱包软件就可以基于该词组创建一系列的账户，并且保障不论是在什么硬件、什么时间创建出来的账户、公钥、私钥都完全相同，这样既解决了账号识记的问题，也把账户恢复的门槛降低了很多。支持 BIP39 提议的钱包也可以归类为 HD 钱包（Hierarchical Deterministic Wallet），Metamask 属于此类。

为了方便后续使用，这里需要点击下面的红色按钮 "DOWNLOAD SEED WORDS AS FILE"，把助记词保存为本地文件，然后单击 "I'VE COPIED IT SOMEWHERE ELSE"，表示已经把助记词保存到了其他地方。

钱包和账户初步配置成功，里面的余额为 0 ，如下图：

<div class="scale"><img src="img/resources/blockchain/blockchainmetamask6.png"  alt="blockchainmetamask6" /></div>

为了验证助记词泄露的后果，可以把助记词复制出来，放到 [iancoleman.io](https://iancoleman.io/bip39/) 上去，看看能不能得到对应的以太坊账号私钥和公钥，答案是：能，并且能得到该助记词在任何区块链网络上的账户信息。

另外可以尝试钱包软件：[imtoken](https://token.im/) 和 [bitpie](https://bitpie.com/)

参考：

[以太坊轻钱包 Metamask](https://metamask.io/)

[Google Chrome WebStore 的 Metamask 插件](https://chrome.google.com/webstore/detail/metamask/nkbihfbeogaeaoehlefnkodbefgpgknn?hl=en)

[ BIP39 提议](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki)

[iancoleman.io](https://iancoleman.io/bip39/)

### Part5 完成第一笔以太坊交易：给钱包充值 18.75 ETH

现在钱包和账户有了，但是里面却没钱，而以太坊网络中的转账、智能合约函数调用基本都是要花钱的，难道我们测试的时候也要支付白花花的银子？

社区已经提供了很不错的解决办法。免费充值方法（当然是充值到测试网络中）有两个：

一、rinkeby-faucet.com，只要提供账户地址即可充值 0.001 ETH，理论上是可以无限制充值的，但是如果需要充值 1ETH，需要操作 1000 次，太费劲，建议大家直接使用第 2 种方式；

二、faucet.rinkeby.io，可以提供多达 18 ETH 的充值金额，但是为了避免被滥用，要求接受充值的账户持有人必须以太坊账户地址发送到自己的社交网络中（如 Twitter、Facebook、Google Plus），同样，该工具限制了充值的频率；

以下教会大家，如何使用 faucet.rinkeby.io 为 Metamask 里面的账户充值 18 ETH。

1）复制 Metamask 账户的地址，点击小狐狸的图标打开钱包（必要的时候需要输入密码解锁钱包），然后点击 "Account 1" 右边的三个点，在弹出的菜单中选择 "Copy Address to Clipboard"，把地址复制到剪贴板，如下图：

<div class="scale"><img src="img/resources/blockchain/blockchainfaucet1.png"  alt="blockchainfaucet1" /></div>

2）打开 plus.google.com，确保处于登录状态，如下图，按页面右下角的按钮，准备开始发布新的状态：

<div class="scale"><img src="img/resources/blockchain/blockchainfaucet2.png"  alt="blockchainfaucet2" /></div>

3）把复制到的 Metamask 账户地址粘贴到状态发布输入框里面，然后点击发布：

<div class="scale"><img src="img/resources/blockchain/blockchainfaucet3.png"  alt="blockchainfaucet3" /></div>

4）单击新发布状态卡片右上角的分享按钮，会在新标签中打开该状态：

<div class="scale"><img src="img/resources/blockchain/blockchainfaucet4.png"  alt="blockchainfaucet4" /></div>

5）在新标签中复制地址栏中的地址备用：

<div class="scale"><img src="img/resources/blockchain/blockchainfaucet5.png"  alt="blockchainfaucet5" /></div>

6）打开 faucet.rinkeby.io，按下图提示操作：

<div class="scale"><img src="img/resources/blockchain/blockchainfaucet6.png"  alt="blockchainfaucet6" /></div>

7）提交充值申请之后，可能会遇到 Google 的图形验证码，按提示操作即可，等待转账完成，可以看到如下的提示：

<div class="scale"><img src="img/resources/blockchain/blockchainfaucet7.png"  alt="blockchainfaucet7" /></div>

8）重新打开 Metamask 钱包账户，查看账户余额：

<div class="scale"><img src="img/resources/blockchain/blockchainfaucet8.png"  alt="blockchainfaucet8" /></div>

9）刚才明明充值成功了，为什么账户余额还是 0 呢？原因是充值操作只发生在 Rinkeby 测试网络中，而 Metamask 钱包默认链接的是以太坊主网，还记得主网和测试网络的账号可以通用，但是账户中的数据是完全隔离的么？点击 Metamask 钱包界面左上角的 "Main network"，按下图切换到 Rinkeby 测试网络即可：

<div class="scale"><img src="img/resources/blockchain/blockchainfaucet9.png"  alt="blockchainfaucet9" /></div>

10）切换到 Rinkeby 测试网络之后，不出意外，你能看到如下的账户余额，恭喜你，拿到 1W 美金的测试资金：
<div class="scale"><img src="img/resources/blockchain/blockchainfaucet0.png"  alt="blockchainfaucet0" /></div>

### Part1~Part5 总结

辛辛苦苦的学习了区块链的简明发展史，区块链、以太坊的核心概念和原理后，我们实践了如何用Metamask创建一个以太坊轻HD钱包。最后终于完成了第一笔以太坊交易：给自己的钱包重置18.75 ETH（虽然是充值到测试网络中，换不了钱，但很有成就感啊）。

接下来我们将另开新篇，深入理解以太坊中的交易（Transaction），再重点聊一下智能合约的开发。

如果你是理科生，可以继续看下去。如果是文科生的话，可以把本篇再看三遍，毕竟接下来要编写代码了。

----- ----- ----- -----

<div class="scale"><img src="img/authors/wechatloi.jpg"  alt="wechatloi" /></div>



