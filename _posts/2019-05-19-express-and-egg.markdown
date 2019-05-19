---
layout: post
title: "Node.js web application framework about express and egg"
subtitle: "what is different about express and egg"
date: 2019-05-19 14:00:00
author: "Vinter"
catalog: true
tags:
    - egg
    - express
---

> 当你感觉这个世界的不公时，那说明你自己还不够优秀。     
> 与其羡慕别人，不如退而结网，沉淀自己。       
> 种一棵树最好的时间是十年前，其次是当下。      

人生的路很长，其中会遇到很多的人和事，有些人上车又下车了，在你的生命中留下了一撇，也有的人陪你到了终点，生活就是这样，当你经历的多了，你会越来越能看见自己需要的是什么，慢慢引领你走向自己的人生轨迹，朝着更加清晰明亮的未来驶去。多接触并感谢身边遇到的人，其中可能会有烦恼和不悦，但更多的是情感的沉淀，成长的催化剂，保持一颗沉稳而又坚定的心态，迎接未来的风风雨雨。
<br/>
我晕，写个技术博客，咋都是些情感文章，人家一看可能以为我是那个情感作家，想想不去做个情感专家还是有点可惜了（为自己哀伤一秒钟），更为世界少了一位大文豪而默默惋惜（容我小小自恋一下）。好了言归正传，进入正题。
<br/>
JS的运行平台主要两个，一个前端开发的浏览器，另一个是服务端开发的Node.js；Node.js既能运行JS又能搭建服务端，想成为全栈工程师，如果又不想去学习其他java、php等语言，那Node.js是你不二的选择。既然Node.js能搭建服务端，那必然会带来一些周边服务，如框架和插件库等等。如java的SpringMVC、Spring、Struts以及Hibernate等；PHP的ThinkPHP、Lavavel和Codelgniter等等。而Node.js主要的几个web框架有：Express、Koa和Egg，因为Egg选择了Koa作为其基础框架，继承了Koa特性和功能，并在它的模型基础上，进一步对它进行了一些增强。所以主要介绍Express和Egg的主要不同。

## 简介
[Express](http://www.expressjs.com.cn/)是一个保持最小规模灵活的Node.js Web应用程序开发框架，为Web和移动应用程序提供一组强大的功能，目前已到了4.x版本，虽然官方已经介绍了Express5的一些内容，但是还处于测试发布阶段，而且相比Express4.x没有太大的变化，所以文章主要比对的对象是Express4.x。Koa是Express原班人马基于ES6新特性重新开发的框架，主要基于co中间件。而[Egg](https://eggjs.org/zh-cn/)根据Koa作为基础框架，相比更加年轻，基于“约定优于配置”的原则，给开发带来快速方便的同时，也存在一些约束和限制，具体后文会具体解释。目前Egg已经到了Egg2.x版本，全面支持async/await的异步变成方式，并且对于1.x的generator function也完全兼容。

## 中间件的差异
### 中间件的作用
中间件(MiddleWare)可以理解为一个对用户请求进行过滤和预处理的东西，主要有全局中间件和路由中间件，可以使用第三方中间件也可以自己定义一个中间件。Express中的中间件是使用了Connect中间件框架。（Connect是一个中间件框架它的作者与Express的作者是同一个人）在3.x的版本中Express是包含了Connect的中间件，而在最新的4.x版本中Express不再依赖Connect，而且从内核中移除了除`express.static`外的所有内置中间件。也就是说现有的Express是一个独立的路由和中间件web框架，Express的版本升级不再受中间件更新的影响。如果使用一个全局中间件，Express中你需要使用如下方法。
```
npm install --save <module-name>
const <module-name> = require('module-name')
app.use(<module-name>)
```
Egg使用一个全局的应用中间件，需要这样。
```
//  config.default.js
module.exports = {
  // 配置需要的中间件，数组顺序即为中间件的加载顺序
  middleware: [ '<module-name1>', '<module-name2>', ... ],

  // 中间件的配置
  <module-name1>: {
    ...
  }
};
```
总而言之，中间件为你对客户端的请求进行了过滤和筛选，减少了代码的重复，让代码逻辑更加清晰，更具有高效的扩展性。
### 中间件的原理
Express的中间件是按线性执行的，中间件处理完后只有两个选择：交给下一个中间件或者返回Response。如下图
![Express中间件流程图](https://diycode.b0.upaiyun.com/photo/2019/20995e727ab22b109d2d15f769123cb0.png "image")
一个Express的中间件结构如下
```
function Middleware(request, response, next) {
    // 对request和response作出相应操作
    // 操作完毕后返回next()即可转入下個中间件
    next();
}
```
<br/>
Egg的中间件和Koa一样使用的是洋葱模型，中间件请求要穿过层层洋葱到底层，然后再层层返回出来，即每个中间件也会被穿过两次。
![Egg中间件流程图](https://camo.githubusercontent.com/d80cf3b511ef4898bcde9a464de491fa15a50d06/68747470733a2f2f7261772e6769746875622e636f6d2f66656e676d6b322f6b6f612d67756964652f6d61737465722f6f6e696f6e2e706e67 "image")
从图中可以看出，Egg请求（request）时从最外层中间件到最内层中间件，响应（response）时则相反，从最内层中间件到最外层中间件。
<br/>
一个简单Egg中间件的例子（gzip）
```
// app/middleware/gzip.js
const isJSON = require('koa-is-json');
const zlib = require('zlib');

const gzip = async (ctx, next) => {
    //  请求阶段,主要对ctx.request进行处理
    await next();

    // 后续中间件执行完成后将响应体转换成 gzip
    let body = ctx.body;
    if (!body) return;
    if (isJSON(body)) body = JSON.stringify(body);

    // 设置 gzip body，修正响应头
    const stream = zlib.createGzip();
    stream.end(body);
    ctx.body = stream;
    ctx.set('Content-Encoding', 'gzip');
}
```
Egg中间件不仅可以对请求进行拦截，而且可以在响应返回时对响应进行拦截处理，相比Express更加灵活。

## 异步流程
Express采用callback，Express使用callback捕获异常，对于深层次的异常捕获不了，对于多个异步需要层层嵌套，可读性差。
<br/>
Egg2.x则支持最新ES7的async/await处理异步，能通过try...catch捕获最深层的异常，并按照同步的写法书写异步代码，可读性强，对1.x版本的generator function也完全支持，但是有个缺点就是不支持ES6的import/export模块加载方法，不过这个可以通过使用babel解决这个问题。

## 响应和请求
Express的中间件可以拿到request，response，next；但是Egg则把request和response封装到了ctx中（上下文），ctx不仅包括了request和response，而且还能拿到加载循序中的其他内容，如插件plugin，应用application，配置config，服务service和控制controller等，所以相比Express的操作，Egg更加灵活，但是Egg能拿到对应的上下文内容，需要遵从它的目录结构，Egg的目录结构和加载顺序如下。
```
egg-project
├── package.json
├── app.js (可选)
├── agent.js (可选)
├── app
|   ├── router.js
│   ├── controller
│   |   └── home.js
│   ├── service (可选)
│   |   └── user.js
│   ├── middleware (可选)
│   |   └── response_time.js
│   ├── schedule (可选)
│   |   └── my_task.js
│   ├── public (可选)
│   |   └── reset.css
│   ├── view (可选)
│   |   └── home.tpl
│   └── extend (可选)
│       ├── helper.js (可选)
│       ├── request.js (可选)
│       ├── response.js (可选)
│       ├── context.js (可选)
│       ├── application.js (可选)
│       └── agent.js (可选)
├── config
|   ├── plugin.js
|   ├── config.default.js
│   ├── config.prod.js
|   ├── config.test.js (可选)
|   ├── config.local.js (可选)
|   └── config.unittest.js (可选)
└── test
    ├── middleware
    |   └── response_time.test.js
    └── controller
        └── home.test.js
```
如果使用egg-sequelize进行服务搭建，还会有app/model/**，这个model目录用来定义数据库的类型，如moogose集合中的文档类型或mysql表格中行字段类型。其中他们的加载循序如下。
<br/>
package.json => 
<br/>
config/plugin.{env}.js =>
<br/> 
config/config.{env}.js => 
<br/>
app/extend/application.js => 
<br/>
app/extend/request.js => 
<br/>
app/extend/response.js => 
<br/>
app/extend/context.js => 
<br/>
app/extend/helper.js => 
<br/>
agent.js => 
<br/>
app.js => 
<br/>
app/service => 
<br/>
app/middleware => 
<br/>
app/controller => 
<br/>
app/router.js

## 总结
通过对比Express和Egg其中主要的中间件，异步化流程和请求和响应处理的差别，可以看出Egg的强大之处。中间件的洋葱模型，支持最新ES7的async/await异步编程以及对请求和响应封装了的ctx上下文；除了这些还具有生命周期方法，通过egg-sequelize插件快速整合mysql数据库等等。
<br/>
<br/>
不过，对于新手，本人建议系统学习下Express，因为Egg秉承着“约束优于配置”，按照其约定的目录，此外很多内容进行了封装处理，一些配置如插件、中间、路由、服务和控制按照官网介绍，简单容易搭建。所以Express相对可以让你更多了解一些配置的来龙去脉，自定义的目录结构，对于后面入手Egg会变得更加顺心应手，当然这里主要针对Express4.x。