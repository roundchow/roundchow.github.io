---
title:  "小程序的制作旅程 - Part 1 - 搭建开发环境"
subtitle: "一段奇妙的旅程 总要有个主动的开始"
author: "wu"
avatar: "img/authors/tigerCat.jpg"
image: "img/f.jpg"
date:   2018-11-26 03:00:00
---

一段奇妙的旅程 总要有个主动的开始 正如今天 准备开始迭代一个小小的小程序

----- ----- ----- -----

## Part 1 - 搭建开发环境

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

<div class="scale"><img src="img/resources/weichatsmall/wechatsmallmenustructure.png"  alt="wechatsmallmenustructure" /></div>

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

<div class="scale"><img src="img/resources/weichatsmall/wechatsmallprojectsetting.png"  alt="wechatsmallprojectsetting" /></div>

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

### mock server 实现

### 前端对云函数的调用

### 本篇章总结

----- ----- ----- -----

<div class="scale"><img src="img/hugkiss.jpg"  alt="λanguage" /></div>



