---
title:  "小程序制作旅程-Part 2-「周围天气」小程序简介"
subtitle: "一段奇妙的旅程 总要有个主动的开始"
author: "wu"
avatar: "img/authors/tigerCat.jpg"
image: "img/f.jpg"
date:   2018-11-26 03:00:00
---

一段奇妙的旅程 总要有个主动的开始 正如今天 准备开始迭代一个小小的小程序

----- ----- ----- -----

## Part 2 - 「周围天气」小程序简介

他山之石可以攻玉，本篇所描述的小程序雏形并非原创，为了一步步实践产品策划、界面设计、技术实现，并在雏形的基础上重新策划、设计、研发成新的小程序（「周围天气」），于是有了此篇。

这款天气+心情签到的小程序原雏形，已经完成上线，可以通过扫描下面的二维码进行线上体验：

<div class="scale"><img src="img/resources/wechatsmall/wechatsmallcode.png"  alt="wechatsmallcode" /></div>

数据都是来自腾讯地图、和风天气的免费 API；小程序·云开发初级配置是免费的，能够满足小型小程序的计算、存储和数据库功能。

### 一、「周围天气」页面组成

「周围天气」小程序由天气预报页面和心情签到页面组成：

- 天气预报页面：主要是天气数据的展现，定位接口使用腾讯地图，天气数据来自和风天气 API，其中顶部实时天气温度用的是“体感温度”
- 心情签到页面：使用云开发数据库存储心情，每日可签到一次，不同心情不同颜色

#### 1 - 天气预报页面模块和技术栈

<div class="scale"><img src="img/resources/wechatsmall/wechatsmalluimodule.png"  alt="wechatsmalluimodule" /></div>

天气预报页面由实时天气预报、24 小时天气预报、一周天气预报和生活指数共四大模块组成，这四大模块各有各的特点：

- 实时天气预报：这块页面元素较多，页面复杂度高，其中顶部定位模块有事件绑定，右侧签到入口有「心情签到」页面入口；除此之外，在雨雪天气整个区域还会有雨雪动效，动效是使用小程序的绘图 API 实现的粒子系统
- 24小时天气：这个区域主要使用了小程序的 scrollView 模块和 flex 布局
- 一周天气预报：该区域主要是 flex 布局和 Chart.js 图表的使用
- 生活指数：该区域每个指数都绑定了 tap 事件，详细的生活指数内容是经过事件传值给浮层的

整个页面背景图片抓取了 UC 天气背景图，可以根据不同天气更换图片
整个项目中用到的图标，都是由 components 下面的 icon 组件实现的

在天气预报这个页面，技术方案涵盖：

- 小程序布局常用组件 view、text、scrollView、image、canvas 等 UI 组件
- 使用 wx.request 模块获取数据
- 使用小程序绘图 API 实现雨雪效果的粒子系统
- 小程序的事件绑定和处理
- 定位 API 和选择位置组件的调用，分析不同坐标系之间的区别
- 实现一个 icon 的小程序组件
- 在小程序内使用 chartjs 做报表展现
- 深入体会和理解 wxs、rpx 等概念
- 使用小程序云函数实现和风天气 API 的数据获取

#### 2 - 心情签到页面模块和技术栈

<div class="scale"><img src="img/resources/wechatsmall/wechatsmallcheckin.png"  alt="wechatsmallcheckin" /></div>

心情签到是一个可以记录自己心情起伏的小工具，它有助于我们找到心情起伏的原因。整个心情签到页面主要包含的内容有：

- 小程序插件的使用
- 授权登录，获取用户信息等跟用户相关 API 的使用
- 云开发的数据库操作
- 使用小程序云函数获取用户 openid

### 二、项目目录结构

整个项目目录结构如下：

<div class="scale"><img src="img/resources/wechatsmall/wechatsmallstructureweather.png"  alt="wechatsmallstructureweather" /></div>

- server：小程序云开发环境的 mock server 和云函数的 cloud-functions
- client：小程序前端主要代码；在 client 中会有小程序的配置和工具配置等文件
- gulpfile.js： 是 Gulp 的脚本
- test：是云函数测试脚本文件夹
- dist：是项目产出的文件夹，会把 client 和 server 的cloud-functions编译进去，也是小程序开发者工具选择的项目路径
- bin：是工具脚本，比如抓取图片相关的脚本等

#### 1 - 配置

因为天气页面是没有顶部导航栏的，这样整个页面更加开阔，视觉效果更好，所以小程序的 app.json 中定义了导航条样式是自定义：

<pre>
"window": {
    "navigationStyle": "custom"
}
</pre>

小程序云开发的云函数放在 server/cloud-functions 内，经过打包工具 Gulp 处理之后，会放到 dist/cloud-functions 内，所以 project.config.json 中的云函数配置如下：

<pre>
{
    "cloudfunctionRoot": "./cloud-functions/"
}
</pre>

#### 2 - 项目启动

首先 git clone 出项目到自己本地电脑 (如果对详细代码感兴趣，可以加微信，微信号：superloiwu，加好友请备注：微信小程序伙伴-周围天气小程序简介)。

然后进入项目路径，安装项目依赖：

<pre>
npm i
</pre>

再依次进入云函数的目录，安装依赖：

<pre>
# 依次进入目录
cd server/cloud-functions/roundchow-weather
npm i
</pre>

#### 3 - 修改开发者信息

为了保护原作者个人的开发者信息，防止敏感信息泄露，clone 下代码后需要按照以下步骤修改为自己的开发者账号：

- 修改小程序云开发的开发环境：client/lib/api.js 中

<pre>
wx.cloud.init({
    env: '填写自己的开发者账号中的环境id'
})
</pre>

- 修改腾讯地图的开发者账号：client/lib/api.js 中的 QQ_MAP_KEY，登录 <a target="_blank" href="https://lbs.qq.com/console/user_info.html">腾讯地图开发者控制台</a> 获取

- 修改和风天气 API 的开发者账号 server/inline/utils 中的 KEY 和 USER_ID，登录<a target="_blank" href="https://console.heweather.com/">和风天气控制台</a>获取

- 小程序授权信息 server/inline/utils 中的 WECHAT_APPID 和 WECHAT_APP_SECRET，登录小程序管理后台获取

#### 4 - 项目二次开发

开发的时候，需要监听文件的变化，启动本地 mock server

<pre>
# mock server 启动
npm run server
# 启动 cloud functions 云函数文件夹同步
npm run cloud
# 编译项目，并且启动 gulp watch 功能
npm run dev
</pre>

用小程序开发者工具打开项目的 dist 文件夹即可。

这里可能会遇到几个小问题，按以下方式解决：

- 如果提示插件并未授权，请参考后续心情签到页面的插件使用部分内容
- 如果提示域名不合法，可以先在开发者工具右上角「详情」的「项目设置」tab 勾选：不校验合法域名选项
- 一定要按照上一小节内容，修改配置各种开发者账号
- 在云开发控制台创建diary的数据库，后续参考心情签到页面开发

#### 5 - 项目打包上线

执行 build 命令：

<pre>
npm run build
</pre>

然后 dist 文件夹下就是构建之后可以上线的全部代码，打开小程序开发者工具：

- 上传并且部署云函数
- 上传小程序代码，登录小程序管理后台，提交审核

### 七、本篇章总结

本节主要介绍项目「周围天气」的原产品雏形，「周围天气」由两个页面组成：天气预报和心情签到。本篇章还介绍了项目的目录结构与配置，以及项目代码如何在本地运行和发布。

如果对本篇涵盖的产品策划、技术方案、详细代码感兴趣，可以加微信（微信号：superloiwu，加好友请备注：微信小程序伙伴-周围天气小程序简介），共同探讨。

----- ----- ----- -----

<div class="scale"><img src="img/authors/wechatloi.jpg"  alt="wechatloi" /></div>



