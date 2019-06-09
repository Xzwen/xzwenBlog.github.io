---
layout: post
title: "Resources about front-end development"
subtitle: "The main purpose of this article is to easily get these resources online"
date: 2019-06-02 14:00:00
author: "Vinter"
catalog: true
tags:
    - front-end
---

> 成熟不需要刻意为之，当你找到自己的方向并为之奋斗，或者说你在为自己喜欢的东西而填充自我的时候，你已经不再是从前的自己，自信也会随之而来。
> 生活中有太多的诱惑与城市的快节奏步伐，不迷乱，克制自己，有自己的生活方式。

昨天是六一，虽然已经过了过节的年纪（哈哈），还是想说点什么。我是比较喜欢六月的，六月是我出生的月份，正所谓666，可能在说我出生的月份恰好（容我自恋一下），其实真正喜欢六月，因为六月是一年中旬，正式进入了夏季，但是还未到酷夏时候，本人还是喜欢夏天，喜欢夏天的阳光明媚，蓝蓝的天空，一览无余。我这种喜欢户外运动的家伙，不用再为了天气的不好，而只能限制在家中。swimming,palying basketball,running 或者mountain climbing等等，想想还有点小激动。今天跑步突然就跑到了小学学校，看了看自己曾经待过地方，坐过的座椅，还有学校的水果树，就当做也给自己过个六一，最让我感触的是校门外的那条道路和道路两旁的风景，真的是沧海桑田，三十年河东，三十年河西，以前破旧的小道，已经扩大了几倍，以前很少有人到这边来的道路，已经是车水马龙，
交通灯都设置了好几个，道路两旁已经是高楼林立，不过唯独刚出校门的那段小道还有点以前的模样，还能回想起以前和同学在路上嘻嘻哈哈上下学的情景，物是人非，可能说的就是这种吧。童年已逝，青年正盛，朝着自己的人生努力奋斗吧！
<br/>
回忆归回忆，该干点正事，这篇文章主要就是汇总前后端常用的一些资源，方便更快的在网上找到，方便自己的同时，方便各位前后端同仁。
<br/>
## HTML && CSS && Javascript
HTML,CSS和JS是前端基础，所以一般学习网站都会同时包含这三者。<br/>
[W3school](http://www.w3school.com.cn/index.html)<br/>
[菜鸟教程](https://www.runoob.com/html/html-tutorial.html)<br/>
[MDN web docs](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript)<br/>

## Typescript
Typescript是JS的超集，Angular2及之后版本已经默认使用它，其他前端框架如Vue和React都已经支持，官方消息好像不久的Vue3.x将会完全支持typescript，所以学习好它也是前端需要的。<br/>
[Typescript](http://www.typescriptlang.org/)<br/>

## front-end framework
前端三大主流框架。
#### Vue（当前2.x）
[Vue官网 v2.x](https://cn.vuejs.org/)<br/>
脚手架[Vue CLI v3.x](https://cli.vuejs.org/zh/guide/)<br/>
[Vue Router v3.x](https://router.vuejs.org/zh/)<br/>
[Vuex v3.x](https://vuex.vuejs.org/zh/)<br/>
[Nuxt v2.x服务端渲染框架](https://zh.nuxtjs.org/)<br/>
[官方优选资源](https://curated.vuejs.org/)<br/>
[awesome-vue 生态资源](https://github.com/vuejs/awesome-vue)<br/>
UI框架<br/>
[element-ui v2.x](https://element.eleme.cn/#/zh-CN)<br/>
[iview v3.x](https://www.iviewui.com/)<br/>
[Vuetify v1.x](https://vuetifyjs.com/zh-Hans/)<br/>
移动端UI框架<br/>
[mint-ui v2.x](http://mint-ui.github.io/#!/zh-cn)<br/>
[cube-ui v1.x](https://didi.github.io/cube-ui/)<br/>
#### React
[React官网 v16.x](https://react.docschina.org/)<br/>
脚手架[Create React App v3.x](https://facebook.github.io/create-react-app/)<br/>
[Next v8.x服务端渲染框架](https://nextjs.org/)<br/>
[Redux v4.x](https://redux.js.org/)<br/>
[React Redux v7.x](https://react-redux.js.org/)<br/>
[React Router v5.x](https://reacttraining.com/react-router/)<br/>
[Redux Thunk v2.x](https://github.com/reduxjs/redux-thunk)<br/>
[redux-saga v1.x](https://redux-saga.js.org/)<br/>
[awesome-react 生态资源](https://github.com/enaqx/awesome-react)<br/>
UI框架<br/>
[Ant Design v3.x](https://ant.design/)<br/>
[material-ui v4.x](https://material-ui.com/)<br/>
[React-Bootstrap v1.x](https://react-bootstrap.github.io/)<br/>
移动端UI框架<br/>
[Ant Design Mobile v2.x](https://mobile.ant.design/)<br/>

#### Angular
[Angular v1.x官网](https://angularjs.org/)<br/>
[Angular v8.x官网](https://www.angular.cn/)<br/>
脚手架[Angular Cli v8.x](https://cli.angular.io/)<br/>
[Ngrx v7.x](https://ngrx.io)，整合了RxJs和Redux的功能，在Angular中进行异步数据管理和状态管理。<br/>
Angular的路由可以通过angular-cli创建<br/>
```
ng new my-routing --routing
```
再通过<router-outlet></router-outlet>标签进行路由跳转，参考文献[Angular Cli生成路由](https://www.cnblogs.com/cgzl/p/8611532.html)<br/>
[awesome-angular 生态资源](https://github.com/PatrickJS/awesome-angular)<br/>
UI框架<br/>
[PrimeNG v7.x](http://primefaces.org/primeng/#/)<br/>
[Angular Material v8.x](https://material.angular.io/)<br/>
[NG-ZORRO v7.x](https://ng.ant.design/docs/introduce/zh)，这里是Ant Design的Angular实现，目前支持Angular ^7.0.0版本，Ant Design是很强大的，即有react的UI库还有angular的UI库。<br/>
[IONIC FRAMEWORK v4.x](https://ionicframework.com/docs)<br/>

## HTTP
两个强大的**第三方库**<br/>
[axios v0.x](http://www.axios-js.com/zh-cn/docs/)（支持 React Native，Node，浏览器）。<br/>
Axios 是一个基于 promise 的 HTTP 请求库，可以用在浏览器和 Node.js 中。浏览器中使用 XMLHttpRequest，Node.js 中使用 http/https 模块。且axios是Vue v2.x官方推荐的请求库，且功能强大，具体请参照官网介绍。<br/>
[fly.js v0.x](https://wendux.github.io/dist/#/language)（Node.js 、微信小程序 、Weex 、React Native 、Quick App 和浏览器）。<br/>
Fly.js的npm包名为flyio，Fly.js是一个支持所有JavaScript运行环境的基于Promise的、支持请求转发、强大的http请求库，定位是成为 Javascript http请求的终极解决方案。也就是说，在任何能够执行 Javascript 的环境，只要具有访问网络的能力，Fly都能运行在其上，提供统一的API。<br/>

**浏览器**<br/>
[Fetch](https://github.com/github/fetch)，浏览器通过XMLHttpRequest，远古的IE6借助ActiveXObject对象来实现Http请求，W3C标准新增Fetch API，基于promise实现Http请求，相对XMLHttpRequest对象调用更方便，但旧浏览器不支持Promise，需要对Promise进行pollyfill。最大的缺点就是没有请求和响应拦截等，需要自己再次封装。<br/>

**Node.js**<br/>
Node.js中通过引入http/https/http2模块实现 http 请求。<br/>
```
const http = require('http');
```

**React Native**<br/>
[Fetch](https://github.com/github/fetch)，也默认支持Fetch API。<br/>

**Weex**<br/>
Weex是阿里2016开源跨平台移动应用开发工具，提供了stream模块来实现网络请求。

**微信小程序**<br/>
```
wx.request({
  url: 'test.php',
  data: {},
  header: {
    'content-type': 'application/json' // 默认值
  },
  success(res) {
    console.log(res.data)
  }
})
```

**支付宝小程序**<br/>
```
my.httpRequest({
  url: 'http://xxxx',
  method: 'POST',
  data: {},
  dataType: 'json',
  success: function(res) {
    my.alert({content: 'success'});
  },
  fail: function(res) {
    my.alert({content: 'fail'});
  },
  complete: function(res) {
    my.hideLoading();
    my.alert({content: 'complete'});
  }
})
```

**jQuery**<br/>
$.ajax（支持浏览器），$.ajax为jQuery对XMLHttpRequest对象进行兼容封装。

## library
#### JS
[Lodash v4.x](https://github.com/lodash/lodash)，[官网](https://lodash.com/)，是一个js工具库，高效，实用且体积小，主要节省js编程时间（推荐）。<br/>
[Underscore](underscorejs.org/)，也是js工具库，体积相对loash要大点。<br/>
[Anime.js v3.x](https://animejs.com/)，一个强大的、轻量级的用来制作动画的javascript库。<br/>
[particles.js](https://github.com/VincentGarreau/particles.js)制作粒子，微粒。<br/>
[three.js v0.x](https://github.com/mrdoob/three.js)该库目标是创建容易使用，轻量级基于WebGL(3D绘图协议)3D协议的3D渲染库。
[fullPage.js v3.x](https://github.com/alvarotrigo/fullPage.js)快速实现全屏滚动特性。
[favico.js](http://lab.ejci.net/favico.js/)动态 favicon
[Sortable v1.x](https://github.com/SortableJS/Sortable) 拖拽插件。


#### CSS
[Animate.css v3.x](https://daneden.github.io/animate.css/)CSS3动画库，也是目前最通用的动画库。<br/>
[Hover.css](https://github.com/IanLunn/Hover)<br/>

#### jQuery
[官网 v3.x](https://jquery.com/)<br/>
[Animsition v4.x](https://github.com/blivesta/animsition) CSS实现动画过渡的 jQuery 插件。<br/>

#### Boostrap
[官网 v4.x](https://getbootstrap.com/)<br/>

#### chart
[Echart v2.x](https://echarts.baidu.com/)百度推出的一款开源的，商业级数据图表，图表类型丰富。<br/>
[Highcharts](https://www.highcharts.com/)是国外的一款功能强大、开源、美观、图表丰富、兼容绝大多数浏览器的纯js图表库。针对个人用户及非商业用途免费，商业用途需要购买授权。<br/>
[Chart.js v2.x](https://github.com/chartjs/Chart.js)是个简单的，面向对象的客户端图形库，支持6种图表类型，基于 HTML5的图表库。小巧而轻便的的图表插件，缺点是支持的图形类型较少，数据交互功能也非常有限。<br/>
[G2 v3.x](https://antv.alipay.com/zh-cn/g2/3.x/index.html)是由阿里团队开发，纯 javascript 编写、强大的语义化图表生成工具，它提供了一整套图形语法，可以让用户通过简单的语法搭建出无数种图表，并且集成了大量的统计工具，支持多种坐标系绘制，可以让用户自由得定制图表，是为大数据时代而准备的强大的可视化工具。<br/>

#### Icon
[Font awesome v4.x](http://fontawesome.dashgame.com/)<br/>
[阿里巴巴矢量图标库](https://www.iconfont.cn/home/index?spm=a313x.7781069.1998910419.2)<br/>
[feather v4.x](https://feathericons.com)<br/>
[react-native-vector-icons](https://oblador.github.io/react-native-vector-icons/) 这是基于react-native的图标库，里面分类齐全，来源各大图标库，例如Ant Design、Font awesome、Feather等，可以根据它的分类去查找适合自己的图标。<br/>

#### Canvas
[fabric.js v3.x](https://github.com/fabricjs/fabric.js)<br/>
[d3/d3](https://d3js.org/) 数据驱动SVG, Canvas and HTML。<br/>

#### Svg
[fabric.js v3.x](https://github.com/fabricjs/fabric.js)<br/>

## Node
[官网 v10.x](https://nodejs.org/zh-cn/)<br/>
框架<br/>
**Express**<br/>
[Express v4.X](http://www.expressjs.com.cn/)<br/>
**Egg**<br/>
[Egg v2.x](https://eggjs.org/zh-cn/)<br/>
[Sequelize v5.x](http://docs.sequelizejs.com/)<br/>
**koa**<br/>
[Koa v2.x](https://koajs.com)<br/>

## github
[Github](https://github.com/Xzwen)<br/>
[Git](https://git-scm.com/)<br/>
[jekyll v3.x](https://jekyllrb.com/)，本blog就是基于jekyll + github page搭建。<br/>
[Hexo](https://hexo.io/zh-cn/)，脚手架hexo-cli，它是和jekyll一样可以快速搭建静态博客页面的库。<br/>

## Database
#### Mongodb
[MongoDB](https://www.mongodb.com/)<br/>
[Mongoose v5.x](https://mongoosejs.com/docs/api.html)<br/>
#### Mysql
[Mysql](https://www.mysql.com/)<br/>

## Electron
[Electron v5.x](https://electronjs.org/)<br/>
UI框架<br/>
[React-Desktop v0.3x](https://github.com/gabrielbull/react-desktop)<br/>

## 微信小程序
[微信小程序官方文档](https://developers.weixin.qq.com/miniprogram/dev/framework/)<br/>
[wepy v1.7](https://github.com/Tencent/wepy)小程序框架<br/>
[awesome-wepy](https://github.com/aben1188/awesome-wepy)汇总资源<br/>

## 跨平台框架
[react-native v0.59](https://facebook.github.io/react-native/)基于react跨平台，facebook团队开发。<br/>
[awesome-react-native](https://github.com/jondot/awesome-react-native)<br/>

[uni-app ](http://uniapp.dcloud.io)基于vue跨平台<br/>
[awesome-uni-app](https://github.com/aben1188/awesome-uni-app/blob/master/README.md)<br/>

[Ionic v4.x](https://ionicframework.com/docs)基于Angular跨平台，是一个专注于用WEB开发技术，基于HTML5创建类似于手机平台原生应用的一个开发框架。绑定了AngularJS和Sass。这个框架的目的是从web的角度开发手机应用，基于PhoneGap（Cordova）的编译平台，可以实现编译成各个平台的应用程序。<br/>

[Weex v0.x](https://weex.apache.org/zh/) 阿里团队发开，Vue.js 和 Rax 这两个前端框架被广泛应用于 Weex 页面开发，同时 Weex 也对这两个前端框架提供了最完善的支持。<br/>

[Flutter](https://flutter.dev/)Flutter是谷歌的移动UI框架，可以快速在iOS和Android上构建高质量的原生用户界面。优点：性能强大，流畅。但学习成本高，会java语言才能才发，只会JS的前端开发者学习成本太大。<br/>

## 参考
[前端常用库](https://blog.csdn.net/qq_36251118/article/details/77924344)<br/>