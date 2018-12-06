---
title:  "小程序制作旅程-Part 1-搭建开发环境"
subtitle: "一段舒心的旅程 总会有踏实的沉淀"
author: "wu"
avatar: "img/authors/tigerCat.jpg"
image: "img/timg04.jpg"
date:   2018-11-25 03:00:00
---

一段舒心的旅程 总会有踏实的沉淀 正如今天 准备开始迭代一个小小的小程序

----- ----- ----- -----

## Part 1 - 搭建开发环境

他山之石可以攻玉，本篇所描述的小程序技术方案雏形并非原创，为了一步步实践技术实现和加深对技术的理解，于是有了此篇。

### 一、整体技术选型：

- 微信开发者工具：模拟器、调试和代码上传工具
- Visual Studio Code：编辑器，用于代码的编写
- Gulp：前端项目构建工具
- Sass：小程序样式表
- ES6：采用ES6语法编写JS代码，Babel做编译处理
- SCF-Cli：小程序云函数的本地测试工具

### 二、Gulp

小程序开发设计项目资源分类管理，前端构建工具适合采用Gulp，理由如下：

- Gulp可以对不同文件类型、文件夹、文件等多种方式进行不同的处理流程。小程序项目中多种文件类型需要不同的构建流程，试用Gulp的task非常方便管理。
- Gulp的watch功能可以监控源文件，当源码发生变化时，立即执行对应的task，将修改后的代码编译到小程序开发工具监控的目录中。
- 在生态建设上，Gulp工具链较为完善。
- 小程序开发时本地开发模式，代码必须在微信开发者工具提供的Runtime中才可运行，不涉及服务搭建。

### 三、项目目录结构

以下是初始化目录结构：

<div class="scale"><img src="img/resources/wechatsmall/wechatsmallmenustructure.png"  alt="wechatsmallmenustructure" /></div>

### 四、Gulp工程化打包方案

打包方案的目标是：将src目录下的文件，编译到微信开发者工具实际运行的dist目录下。

先在gulpfile.js中定义这两个目录的变量：
<pre>
const src = './src' // 实际开发的源代码文件夹
const dist = './dist' // 构建工具 release 之后的文件夹
</pre>

打包工具Gulp以task为核心，针对不同文件类型（比如通过正则过滤）配置不同的流程控制。小程序打包的主要目的是解决WXML、WXSS、XWS以及JS的编译，并针对小程序开发中常见的问题进行工具化处理，例如px转rpx、压缩优化等。

#### 1 - wxml task

因为wxml语法实际就是html语法，所以无需任何额外处理，直接将.wxml文件release到目标目录中：

<pre>
gulp.task('wxml', () => {
    return gulp
    .src(`${src}/**/*.wxml`)
    .pipe(gulp.dest(dist))
})
</pre>

#### 2 - wxss task

为了更好地维护和提供更加灵活的CSS开发体验，本项目中工使用了sass作为wxss的开发语言，通过Gulp的wxss task将scss/sass文件编译成wxss。

这里说明以下两个问题的解决方案：

- px转rpx：使用postcss-px2rpx，将px按2倍算法转换成rpx
- 将webfont转化成base64引入：在小程序内，webfont不允许访问小程序内部地址，所以将它转化成base64方式再引入

将scss/sass文件处理完毕后，最后一步利用rename工具，将其后缀改为.wxss：

<pre>
const rename = require('gulp-rename')
const postcss = require('gulp-postcss')
const pxtorpx = require('postcss-px2rpx')
const base64 = require('postcss-font-base64')
const combiner = require('stream-combiner2')

gulp.task('wxss', () => {
    const combined = combiner.obj([
        gulp.src(`${src}/**/*.{wxss,scss}`),
        sass().on('error', sass.logError),
        postcss([pxtorpx(), base64()]),
        rename((path) => (path.extname = '.wxss')),
        gulp.dest(dist)
    ])
    
    combined.on('error', handleError)
})

</pre>

可以不使用 CSS 的自动添加浏览器兼容前缀的 autoprefixer 插件，而直接用小程序开发者工具的「详情 -> 项目设置 -> 上传代码时样式自动补全」功能。

<div class="scale"><img src="img/resources/wechatsmall/wechatsmallprojectsetting.png"  alt="wechatsmallprojectsetting" /></div>

#### 3 - js task

微信的 js 文件使用的是 ES5 语法，为了更好的开发体验，本项目开发中使用了 ES6/7 语法，在 Gulp 编译时引入了 babel 插件对 js 进行编译，并且还引入了 sourcemap 以方便本地 debug 代码。

<pre>
gulp.task('js', () => {
    gulp
        .src(`${src}/**/*.js`)
        .pipe(sourcemaps.init())
        .pipe(
        babel({
            presets: ['env']
        })
    )
    .pipe(sourcemaps.write('./'))
    .pipe(gulp.dest(dist))
})
</pre>

#### 4 - 其他 task

对于 json、images 和 wxs 类文件，主要采取的方式是按照当前路径复制到目标目录，所以它们的 task 配置是：

<pre>
gulp.task('json', () => {
    return gulp.src(`${src}/**/*.json`).pipe(gulp.dest(dist))
})
gulp.task('images', () => {
    return gulp.src(`${src}/images/**`).pipe(gulp.dest(`${dist}/images`))
})
gulp.task('wxs', () => {
    return gulp.src(`${src}/**/*.wxs`).pipe(gulp.dest(dist))
})
</pre>

#### 5 - 给每个task增加生产发布打包配置

针对开发和生产两种不同的发布环境，可以通过自定义 Gulp 命令参数来区分，这里使用 --type 来区分，即：

- --type prod：代表生产发布打包
- 默认：为开发发布打包

在生产发布打包的流程中，增加了对资源的压缩（js、html、json、css）和 <a target="_blank" href="https://github.com/zswang/jdists">jdists</a> 的代码块预处理，下面以 js task 为例，解释下怎么配置生产发布的流程（详细解释见注释）：

<pre>
// 引入需要用到的 npm 包
const sourcemaps = require('gulp-sourcemaps')
const jdists = require('gulp-jdists')
const through = require('through2')
const babel = require('gulp-babel')
const uglify = require('gulp-uglify')
const argv = require('minimist')(process.argv.slice(2))
// 判断 gulp --type prod 命名 type 是否是生产打包
const isProd = argv.type === 'prod'
const src = './client'
const dist = './dist'
gulp.task('js', () => {
    gulp
        .src(`${src}/**/*.js`)
        // 如果是 prod，则触发 jdists 的 prod trigger
        // 否则则为 dev trigger
        .pipe(
            isProd
                ? jdists({
                    trigger: 'prod'
                   })
                : jdists({
                    trigger: 'dev'
                   })
        )
        // 如果是 prod，则传入空的流处理方法，不生成 sourcemap
        .pipe(isProd ? through.obj() : sourcemaps.init())
        // 使用 babel 处理js 文件
        .pipe(
            babel({
                presets: ['env']
            })
        )
        // 如果是 prod，则使用 uglify 压缩 js
        .pipe(
        isProd
            ? uglify({
                compress: true
               })
            : through.obj()
        )
        // 如果是 prod，则传入空的流处理方法，不生成 sourcemap
        .pipe(isProd ? through.obj() : sourcemaps.write('./'))
        .pipe(gulp.dest(dist))
})
</pre>

代码块预处理工具jdists，是一种通过注释的方式，将不同的代码块根据不同的指令进行处理的工具。

本项目中主要用到了：

- 根据 trigger 触发 remove 操作；
- 根据 import 将媒介（资源）嵌入到文件的固定位置。

#### 6 - 根据发布环境不同，对 task 进行聚合

上面单个 task 配置完毕，需要添加聚合类的 task 和 watch task，详细配置如下：

<pre>
gulp.task('watch', () => {
    ;['wxml', 'wxss', 'js', 'json', 'wxs'].forEach((v) => {
        gulp.watch(`${src}/**/*.${v}`, [v])
    })
    gulp.watch(`${src}/images/**`, ['images'])
    gulp.watch(`${src}/**/*.scss`, ['wxss'])
})

gulp.task('clean', () => {
    return del(['./dist/**'])
})

gulp.task('dev', ['clean'], () => {
    runSequence('json', 'images', 'wxml', 'wxss', 'js', 'wxs', 'cloud', 'watch')
})

gulp.task('build', ['clean'], () => {
    runSequence('json', 'images', 'wxml', 'wxss', 'js', 'wxs', 'cloud')
})
</pre>

### 五、mock server 实现

#### 1 - mock server技术方案

小程序云函数的联调测试是相当麻烦，每次修改代码，都需要通过微信开发者工具的编辑器，选择云函数文件夹「上传并部署」。这样的开发效率十分低，所以使用一套云函数本地 mock 的方法，使用 mock server 可以在本地开发的时候直接使用 wx.request 方法调用 mock server 的接口，而真正上线的时候（或者发布测试的时候），则使用 wx.cloud.callFunction 方式调用。

mock server 的职责：

- 本地开发时，将云函数代理到 localserver，免除每次上传云函数测试效果的低效率研发方式
- 要设计一套方案，将云函数文件单独提取出来，做到 mock server 和上线后代码统一，不做二次开发（修改），降低开发成本
- 把将来放到服务器管理的静态资源（如图片 icon 类等）暂时放到本地托管，方便本地开发使用

基于上面的职责，小程序项目结构调整如下：

<div class="scale"><img src="img/resources/wechatsmall/wechatsmallmenustructure2.png"  alt="wechatsmallmenustructure2" /></div>

主要变化如下：

- 跟前端相关的文件都放入了 client 中，编译后放到 dist 目录中，微信开发者工具开发目录选择 dist 文件夹
- 跟 mock server 相关的放入 server 中，server 下文件不做打包处理，即不 release 到 dist 文件下
- 其中 server/cloud-functions 是云函数文件夹，编译之后放到 dist/cloud-functions 下
- server/static 文件夹是静态资源文件夹，将来上传到小程序云开发的「文件管理」中维护（小程序云开发 CDN 静态资源服务器）

2 - 使用 Express 来实现 mock server

本项目使用 Express 在本地实现一个 mock server，开启了一个端口号为 3000 的本地服务：

<pre>
const express = require('express')
const {PORT} = require('../config.server.json')
const app = express()

app.listen(PORT, () => {
    console.log(`开发服务器启动成功：http://127.0.0.1:${PORT}`)
})
</pre>

##### 1）实现静态资源服务

使用 express.static 将 server/static 目录设置为静态资源服务器：

<pre>
// 添加static
app.use(
    '/static',
    express.static(path.join(__dirname, 'static'), {
        index: false,
        maxage: '30d'
    })
)
</pre>

静态资源服务器添加好之后，访问 http://127.0.0.1:3000/static/xxx 就可以直接访问 static 文件夹下的静态资源了

##### 2）实现云函数服务

为了满足「云函数文件线上和 mock server 使用一份，不二次开发」的需求，我们直接按照云函数的写法写代码即可，比如 cloud-functions/test/ 模块：

<pre>
exports.main = async (event) => {
    let {a, b} = event
    return new Promise((resolve, reject) => {
        resolve({result: parseInt(a) + parseInt(b)})
    })
}
</pre>

在 server/index.js 中引入对应的模块，然后分配一个路由即可：

<pre>
const test = require('./cloud-functions/test/').main

app.get('/api/test', (req, res, next) => {
    // 将 req.query 传入
    test(req.query).then(res.json.bind(res)).catch((e) => {
        console.error(e)
        next(e)
    })
    // next()
})
</pre>

上面代码中，将 req.query 传入 test.main，构造一个云函数的 event 参数，用于获取云函数的参数，最后通过 Promise 的 then 传递给 res.json 输出。

写完上面代码，再访问 http://127.0.0.1:3000/api/test?a=1&b=2 就会输出：

<div class="scale"><img src="img/resources/wechatsmall/wechatsmallcloudfunction.png"  alt="wechatsmallcloudfunction" /></div>

##### 3）使用 nodemon 对 server 进行自动重启

在云函数开发中，当文件改动了，需要重启 Node.js 服务，如果每次都手动操作就太消耗时间和精力了，所以引入了<a target="_blank" href="https://github.com/remy/nodemon">nodemon</a> 对 server 目录下文件进行监控，发现文件修改，则重启 Node.js 服务。nodemon 的重启命令放在 package.json 中维护：

<pre>
// 启动
"scripts": {
    "server": "nodemon ./server/index.js"
},
// nodemon 配置
"nodemonConfig": {
    "ignore": ["test/*", "book/*", "client/*", "bin/*", "node_modules", "dist/*", "package.json"],
    "delay": "1000"
},
</pre>

效果如下图所示：

<div class="scale"><img src="img/resources/wechatsmall/wechatsmallnodemon.png"  alt="wechatsmallnodemon" /></div>

### 六、前端对云函数的调用

mock server 中的云函数实现了一套代码在本地和线上都可以跑通，但是 client 中页面引用云函数使用 wx.cloud.callFunction 却不能实现一套代码通用，为解决这个问题，笔者通过 jdists 的 remove 和 trigger 方式来实现差异化管理，即

- 将云函数调用等 API 接口请求调用方法，统一放入 client/lib/api.js 中实现，api.js 中使用 wx.cloud.callFunction 方法
- 将云函数相关的再用 wx.request 方法实现一下，请求本地 127.0.0.1:3000/api/ 接口，代码在 api-mock.js 中实现
- api.js 和 api-mock.js 输入的参数和输出的结果是一致的，而内部实现是不同的
- 使用某个云函数时，通过上文提到的 jdists 的 remove 和 trigger 分别引入

继续拿 test 这个云函数做说明，api.js 中直接使用：

<pre>
export const test = (a, b) => {
    return wx.cloud.callFunction({
        name: 'test',
        data: {
            a, b
        }
    })
}
</pre>

然后在 api-mock.js 中实现一次：

<pre>
// 因为小程序的 callfunction 是 Promisify 的，所以这里需要用 Promise 处理一下
// 小程序中不支持 Promise，所以引入了 bluebird 这个库
// 更正以上：由于微信开发者工具和微信真机环境的不断升级，小程序中要使用Promise的话，
// 已经不必要再引入第三方库如bluebird或es6-promise了，可直接使用。
import Promise from './bluebird'
export const test = (a, b) => {
    return new Promise((resolve, reject) => {
        wx.request({
            url: 'http://127.0.0.1:3000/api/test',
            {a,b},
            success: (res) => {
                resolve({result: res.data})
            },
            fail: (e) => {
                reject(e)
            }
        })
    })
}
</pre>

### 七、本篇章总结

本片主要讲解了 Gulp 构建小程序开发脚手架，从 Gulp 的配置说起，介绍了 WXML、Sass、ES6/7 编写小程序前端代码，然后针对云函数开发测试体验不好的问题，介绍了使用 Express 实现本地 mock server 的方式，将云函数和静态资源文件在本地服务器统一管理，实现「一套代码，多处执行」的效果。

如果对本篇涵盖的详细技术方案和代码实现感兴趣，可以加微信（微信号：superloiwu，加好友请备注：微信小程序伙伴-搭建开发环境），共同探讨。

----- ----- ----- -----

<div class="scale"><img src="img/authors/wechatloi.jpg"  alt="wechatloi" /></div>



