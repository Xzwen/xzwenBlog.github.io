---
layout: post
title: "React Native"
subtitle: "A framework for building native mobile apps using JavaScript and React"
date: 2019-03-01 14:00:00
author: "Vinter"
catalog: true
tags:
    - React Native
    - React Navigation
---

> 当你总是不断忙碌，但是总结时发现没有任何新东西的进入，那你就是在重复做无用功。   

> 多去尝试新的领域，你会发现更多美好的事物。


时间如白驹过隙，转眼间都过完年快一个月了，静下心来想想，这个我感觉没做什么啊，上班、吃饭、睡觉，怎么就过完一个月了，可能是工作太认真，突然就到饭点，然后中午睡个觉，再敲会代码，唰！就到了下班时间，这说明这段时间还是有所收获的，过得算比较充实的。<br/>
主要是上面的人说准备开发app，而且要我们大前端独立开发，独立开发！想想这不都是安卓、或者ios程序猿的事情么，开发混合app或者H5、webApp还好，这都不是难事，但是前端独立开发一款app还是有些难度的。不过现在这些都已不是什么难事了，因为[React Native](https://facebook.github.io/react-native/docs/navigation)的出现，React Native是一款通过React和JS来建造原生app的框架，废话不多说了，show time!
<br/>

介绍React Native 之前先介绍两个与它相关的框架吧。
+ **taro**
+ **uni-app**
<br/>
为什么要介绍它们，是因为这两款框架都是跨平台的前端框架，支持一次代码，多个平台使用（微信/支付宝/百度小程序、H5、移动app等），主要的区别在于，taro基于React开发，而uni-app是基于Vue进行开发的，如果公司的应用功能不复杂，又想减少开发成本并且想在多个平台使用，这两个框架可以选择的。taro是可以编译成React Native的，但是我看了taro官网，发现编译成React Native还是需要做一定的调整，它和其他端还是有些区别的，坑还是挺多的。接下来进入主题，介绍React Native。

## 简介
React Native(简称RN) React是Facebook开源的的框架，自然他的衍生产品React Native 也是属于Facebook的，React Native可支持跨平台（主要是ios、android的app或web端）,学习一次，编写多个平台，这是它的理念。

> Learn once,write anywhere

+   丰富的原生组件
<br/> 
并且它拥有丰富的组件，如TabBar、Navigation等标准的平台组件；
+   异步执行 
<br/>
JavaScript应用代码和原生平台之间所有的操作都采用异步执行模式，原生模块使用额外线程，开发者可以解码主线程图像、后台保存至磁盘、无须顾忌UI等诸多因素直接度量文本设计布局。
+   触摸事件
<br/>
React Native引入了一个类似于iOS上Responder Chain响应链事件处理机制的响应体系，并基于此为开发者提供了诸如TouchableHighlight等更高级的组件。

## 环境搭建
安卓和ios开发环境是不一样的，安卓需使用Window、Linux或者macOS平台进行开发，通过android studio构建虚拟机；而ios则必须使用macOS开发，通过xcode构建虚拟机，当然你可以使用真机进行测试。
**接下来主要以Window开发安卓app进行讲解**
<br/>
环境依赖：Node, Python2, JDK。
<br/>
脚手架：react-native-cli
```
npm install -g yarn react-native-cli
//  Yarn 是Facebook提供的替代npm的工具，可以加速node模块的下载

```
```
react-native init AwesomeProject
// 开启虚拟机或者真机
//  安装完依赖后 
react-native run-android

```
具体的搭建我就不多讲了，官网很详细

## 结构与语法
+ 语法

    React Native 语法和React是一样的，所以需要一定的React基础
+ 样式

    React Native 具有自己的样式搭建方法，引进StyleSheet，再通过StyleSheet.create创建css样式，这个还是挺方便的，至于如何使用scss和less没去研究了，如果觉得还是不好用，可以使用它相关的样式库styled-components。不过默认它使用了Flexbox布局，这点我觉得还是非常好的。
+ 数据

    数据基本和React没什么差别了，props、state，如果项目复杂再搭配个redux，这里就不再详细讲解了
+ 导航

    React Native官网推荐使用[React Navigation](https://reactnavigation.org/docs/zh-Hans/tab-based-navigation.html)进行导航，它提供了多种导航布局方式，翻页式、选项卡式、抽屉式等等。方便实用。
+ 滚动视图

    目前支持ScrollView和Flatlist滚动视图，Flatlist对于长滚动效果很好，因为它不是一次性加载元素，每次加载可视区域内容，性能较好，而scrollView会一次性加载全部内容。
+ 手势响应

    Touchable和TouchableHightlight做的很好，具体的操作不做详解
+ api网络请求

    提供了标准的Fetch请求，用起来比较方便，如果使用axios这类的三方库也是可以的。
```
fetch('https://mywebsite.com/endpoint/', {
  method: 'POST',
  headers: {
    Accept: 'application/json',
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    firstParam: 'yourValue',
    secondParam: 'yourOtherValue',
  }),
});
```

+ 打包apk

    ```
    ./gradlew assembleRelease

    ```

    安卓是这条命令，不过命令行打包前需要配置签名，官网讲的很详细，就不必讲的那么清楚了

+ 组件

    React Native提供了很多组件和API，例如控制状态的StatusBar、模态弹窗、本地存储等等。

## 问题点
1. 开发安卓的时候，发现制作和ios的弹性下拉刷新是很难实现的，虽然React Native 也提供了下拉刷洗和上下加载组件，但是是比较简陋的；
2. 手势响应系统用起来比较复杂
3. 配置顶部滑动导航器，自带的createMaterialTopTabNavigator使用可控性差，样式不好看，建议使用react-native-scrollable-tab-view。
4. app的启动白屏时间，可以通过react-native-splash-screen来解决，原理为首先加载一个广告屏，首屏资源加载完后再进入应用，这个是现有app的基本处理方案，用户体验很好。。。

## 资源分享
React Native相关的库：[awesome-react-native](http://www.awesome-react-native.com/)




