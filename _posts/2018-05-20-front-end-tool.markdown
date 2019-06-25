---
layout: post
title: "前端常用工具介绍及区别"
subtitle: "An article about frontends tool of npm、yarn、cnpm、webpack、gulp and so on"
date: 2018-05-20 21:00:00
author: "Vinter"
catalog: true
tags:
    - npm
    - yarn
    - webpack
    - gulp
---

> 不知道自己的劣势，是对自己的不认识，但是如果知道自己的不足，却不去纠正改善的，是对自己的放纵和不负责，那更加可悲。

## 一 、包管理工具
#### 1、npm
**npm是nodejs的一个模块，是nodejs的包管理器。**

我们前后端开发项目时，会用到很多别人已经写好的javascript代码，如果每当我们需要别人的代码时，都根据名字搜索一下，下载源码，解压，再使用，会非常麻烦。于是就出现了包管理器。大家把自己写好的源码上传到npm官网上，如果要用某个或某些个，直接通过包管理器下载安装就可以了，不用管那个源码在哪里。并且如果我们要使用模块A，而模块A又依赖模块B，模块B又依赖模块C和D，此时包管理器会根据依赖关系，把所有依赖的包都下载下来并且管理起来。

**npm包管理器的的特点**
1. 不会缓存，每次安装需要很长时间

2. 安装按照队列顺序执行，不是同步

3. npm5之后才有版本锁定功能，package-lock.json

**package-lock.json详解**
一般来说，版本号主要分为三部分：主版本号、次版本号和修补版本号

major.minor.patch

主版本号.次版本号.修补版本号

patch：修复bug，兼容老版本<br/>
minor：新增功能，兼容老版本<br/>
major：新的架构调整，不兼容老版本<br/>

版本号的主要符号

+ **1、\>**
大于某个版本，表示只要大于这个版本的安装包都行，如>2.0.0。

+ **2、\>=**
大于某个版本，表示只要大于或等于这个版本的安装包都行，如>=2.0.0。

+ **3、\<**
小于某个版本，表示只要小于这个版本的安装包都行，如<2.0.0。

+ **4、\<=**
小于或等于某个版本，表示只要小于或等于这个版本的安装包都行，如<=2.0.0。

+ **5、~**
如：~2.6.6，表示>=2.6.6 <2.7.0，可以是2.6.x，兼容patch。<br/>
如：~2.6，表示>=2.6.0 <2.7.0，可以是2.6.x，兼容patch。<br/>
如：~2，表示>=2.0.0 <3.0.0，可以是2.x.x，兼容minor和patch。<br/>

+ **6、^**
如果major是非零数字，则兼容minor和patch，如果是0，则兼容patch。<br/>
如：^2.6.6 ，表示>=2.6.6 <3.0.0，可以是2.x.x，兼容minor和patch。<br/>
如：^0.6.6，表示>=0.6.6 <0.7.0，可以是0.6.x，兼容patch。<br/>
如：^0.6，表示 >=0.6.0 <0.7.0，可以是0.6.x，兼容patch。<br/>

+ **7、\***
表示任意版本。<br/>
如：*，表示>=0.0.0的任意版本。

一般package.json很多依赖包都带有符号，在npm5版本以前，例如^2.6.6，package.json依赖包根据符号，只能锁定major，不能完全锁定minor和patch，npm install 下载安装的可能是2.8.0，2.6.9等不统一的版本号，对于多人开发的项目，下载安装的依赖包可能会出现多种不同的版本，这就可能导致项目的启动失败等bug。

于是在npm5以后出现了package-lock.json，它完全锁定版本，根据该文件中锁定的版本下载具体版本的依赖包，如2.6.6。更新版本需要npm install xxx@x.x.x 这样去更新我们的依赖，然后package-lock.json也能随之更新。只是单独修改package.json中的版本号是没用的。并且更新了package.json和package-lock.json版本号，npm install是可以覆盖掉node_modules原先版本的。

仓库地址：https://registry.npmis.org/ 

#### 2、cnpm
cnpm是国内淘宝镜像，由于npm安装依赖是从国外服务器下载，受网络影响大，可能出现异常，下载缓慢。而cnpm是淘宝团队开发的国内服务器，下载速度快，受网络影响小。
缺点：对于个别依赖下载，可能会出现问题，因为简单来说cnpm只是npm的一份copy内容。借用官网的说法“这是一个完整 npmjs.org 镜像，你可以用此代替官方版本(只读)，同步频率目前为 10分钟 一次以保证尽量与官方服务同步。”

官方地址：[http://npm.taobao.org](http://npm.taobao.org)<br/>
仓库地址：http://registry.npm.taobao.org/<br/>
安装：npm install -g cnpm --registry=https://registry.npm.taobao.org<br/>

#### 3、yarn
Yarn是新一代包管理工具，它主要有以下优点。

+ 快速

**并行安装：**无论 npm 还是Yarn在执行包的安装时，都会执行一系列任务。npm是按照队列执行每个package，也就是说必须要等到当前package安装完成之后，才能继续后面的安装。而 Yarn 是并行执行所有任务，提高了性能。<br/>
**离线模式：**如果之前已经安装过一个软件包，用Yarn再次安装时之间从缓存中获取，就不用像npm那样再从网络下载了。

+ 可靠

**安装版本统一：**为了防止拉取到不同的版本，Yarn 有一个锁定文件 (lock file)记录了被确切安装上的模块的版本号。每次只要新增了一个模块，Yarn 就会创建（或更新）yarn.lock 这个文件。这么做就保证了，每一次拉取同一个项目依赖时，使用的都是一样的模块版本。npm5版本后也支持版本锁定，默认有package.json文件。

+ 安全

**检查依赖完整性：**在执行代码之前，Yarn 会通过算法校验每个安装包的完整性，防止包的缺失等问题。例如cnpm和npm，没有明确-g或者--save，npm只有检查程序员签名的机制，没有检查包完整性的机制，也不会自动添加依赖到json文件，那么就会出现丢包的假象，但yarn有自己的一套检查包完整性的机制，不会丢包，还会自动判断添加依赖。

+ 简洁

**命令简洁：** yarn和npm的命令不同，简化了一些内容。

```
初始化
npm init ==> yarn init
安装全部包
npm i  ==>  yarn
安装生产依赖包
npm i <package> --save  ==> yarn add <package>
安装开发依赖包
npm i <package> --save-dev  ==> yarn add <package> --dev
移除依赖包
npm uninstall <package> ==> yarn remove <package>
更新全部依赖包
npm update  ==>  yarn upgrade
更新指定依赖包
npm update <package> ==> yarn upgrade <package>
```
官网地址：[https://yarnpkg.com/zh-Hans/](https://yarnpkg.com/zh-Hans/)<br/>
仓库地址：https://registry.yarnpkg.com<br/>

#### 4、bower
npm 是伴随Node.js 出现的一个包管理器，最开始只能支持 Node.js 的模块管理，但是后来， npm官网经过一次改版，打出的口号是javascript 的包管理器，所以，其已经不在局限于是Node.js 的模块管理了，已经通用到了所有 js 的包管理工具了，可以说，前后通吃了。<br/>
bower 从一开始，就是专门为前端表现设计的包管理器，一切全部为前端考虑的。npm 和bower 的最大区别，就是 npm 支持嵌套地依赖管理，而 bower只能支持扁平的依赖（嵌套的依赖，由程序员自己解决）。
现在不建议使用bower了。官方已经停止维护，属于过时的包管理器工具。

## 二、打包工具
#### Webpack

Webpack 是当下最热门的基于配置的前端资源模块化管理和打包工具。它会根据模块的依赖关系进行静态分析，然后将这些模块按照指定的规则生成对应的静态资源。还可以将按需加载的模块进行代码分隔，等到实际需要的时候再异步加载。通过 loader的转换，任何形式的资源都可以视作模块，比如 CommonJs 模块、AMD 模块、ES6 模块、CSS、图片、JSON、SASS、LESS 等。
当前主流前端三大框架Vue、React、Angular2.x的主要脚手架Vue CLI(目前已经发布到3.x版本@vue/cli)、create-react-app、Angular CLI(@angular/cli)都是使用webpack工具进行打包的，可想而知，webpack在当前前端开发中的分量。

webpack的四个核心组成<br/>

1. 入口（entry）
入口起点(entry point)指示 webpack 应该使用哪个模块，来作为构建其内部依赖图的开始。进入入口起点后，webpack 会找出有哪些模块和库是入口起点（直接和间接）依赖的。

2. 输出（output）
output 属性告诉 webpack 在哪里输出它所创建的 bundles，以及如何命名这些文件，默认值为 ./dist。基本上，整个应用程序结构，都会被编译到你指定的输出路径的文件夹中。你可以通过在配置中指定一个 output 字段，来配置这些处理过程。

3. loader
loader让 webpack 能够去处理那些非 JavaScript 文件（webpack 自身只理解 JavaScript）。loader 可以将所有类型的文件转换为 webpack 能够处理的有效[模块](https://www.webpackjs.com/concepts/modules)，然后你就可以利用 webpack 的打包能力，对它们进行处理。

4. 插件（plugins）
loader 被用于转换某些类型的模块，而插件则可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量。[插件接口](https://www.webpackjs.com/api/plugins)功能极其强大，可以用来处理各种各样的任务。

详见官网：[https://www.webpackjs.com/](https://www.webpackjs.com/)

## 三、构建工具
#### Gulp

Gulp就是为了规范前端开发流程，实现前后端分离、模块化开发、版本控制、文件合并与压缩、mock数据等功能的基于流一个前端自动化构建工具。说的形象点，“Gulp就像是一个产品的流水线，整个产品从无到有，都要受流水线的控制，在流水线上我们可以对产品进行管理。”Gulp是通过task对整个开发过程进行构建。

#### Grunt
Grunt 基于 Node.js ，用 JS 开发，这样就可以借助 Node.js 实现跨系统跨平台的桌面端的操作，例如文件操作等等。此外，Grunt 以及它的插件们，都作为一个 包 ，可以用 NPM 安装进行管理。

#### Gulp和Grunt的区别
其实 grunt 和 gulp 两个东西的功能是一样的，只不过是任务配置 JS 的语法不同，**gulp配置更简单，模块的编写也更简单，代码可读性更高**。但是 Gulp 的插件感觉不如 Grunt，只能满足基本的工作需要；而Grunt 官方提供了一些常见的插件，满足大部分日常工作，而且可靠值得信赖。<br/>
总之，用网友的话说，Grunt 和 Gulp 就像 iPhone 与 Android 一样，各有各的优点。本人更喜欢gulp。

## 四、webpack和gulp的区别
按理来说，gulp应该与grunt比较，而webpack应该与browserify进行比较。<br/>
gulp 只是个 task runner，底层只是 node 脚本，不包括模块化的能力，如果需要模块化需要引入另外的框架（比如 requirejs），而 wepack 则本身就是为了模块化而出现的，压缩合并只是它附带的功能。所以他们即可以互补，又可以替换。

>  一个人如果单靠自己，如果置身于集体的关系之外，置身于任何团结民众的伟大思想的范围之外，就会变成怠惰的、保守的、与生活发展相敌对的人。 —— 高尔基