---
layout: post
title: "详解浏览器的垃圾回收机制"
subtitle: "The principle of browser garbage collection"
date: 2018-06-20 22:00:00
author: "Vinter"
catalog: true
tags:
    - 浏览器垃圾回收
---

## 一、简介浏览器缓存

浏览器缓存就是把一个已经请求过的Web资源（如html页面，图片，js，数据等）拷贝一份副本储存在浏览器中。缓存会根据进来的请求保存输出内容的副本。当下一个请求来到的时候，如果是相同的URL，缓存会根据缓存机制决定是直接使用副本响应访问请求，还是向服务器再次发送请求，从而返回新的数据。<br/>

**缓存的优点**<br/>
+ 优秀的缓存策略可以缩短网页请求资源的距离，减少延迟。
+ 缓存文件可以重复利用。
+ 减少带宽资源浪费，降低网络负荷。
+ 加快页面渲染速度。

常规数据请求主要分为三步：HTTP请求、后端处理、浏览器响应三个步骤，浏览器缓存可以帮助我们在第一和第三步骤中优化性能。比如说直接使用缓存而不发起请求，或者发起了请求但后端存储的数据和前端一致，那么就没有必要再将数据回传回来，这样就减少了响应数据。<br/>

理解了缓存和缓存策略的优点，接下来我们带着以下几个问题来深入讲解缓存机制。<br/>
1. 缓存的流程
2. 缓存的储存位置
3. 缓存的分类以及它们之间的异同、优劣。

## 二、浏览器缓存的流程
HTTP请求就是客户端和服务端之间应答的模式，当浏览器发起请求到服务端处理，服务端响应，再到浏览器接受数据，渲染页面，这其中数据是如何被浏览器缓存以及之后如何被浏览器再次使用的，请先看下图。
![缓存流程.png](https://upload-images.jianshu.io/upload_images/5097943-1b29fc522c7a9f3a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
通过上图容易理解<br/>
+ 浏览器每次发起请求，都会先在浏览器缓存中查找该请求的结果以及缓存标识
+ 浏览器每次拿到服务器返回的请求结果都会将该结果和缓存标识存入浏览器缓存中
**以上两点解释了缓存中数据的储存和读取过程，但是浏览器缓存的具体位置在哪里？**<br/>

## 三、浏览器缓存位置

**查看浏览器缓存**<br/>
![image.png](https://upload-images.jianshu.io/upload_images/5097943-0c34a19050562cbd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

缓存位置主要有以下四种<br/>
1. Service Worker
2. Memory Cache
3. Disk Cache
4. Push Cache

#### Service Worker
浏览器缓存本质还是由服务器控制，但是Service Workder则完全由前端浏览器自己控制，可能有人会说，浏览器还可以使用Local Storage或者Session Storage来存储一些数据，但这种方法缺少很多关键的浏览器基础设施，比如异步存储、静态资源存储、URL匹配、**请求拦截**等功能。而Service Worker的出现填补了这些基础设施缺少的问题。<br/>

需要指出的是，Service Worker并非专门为缓存而设计，它还可以解决Web应用推送、后台长计算等问题。Service Worker 是运行在浏览器背后的独立线程，相对于页面主线程是一个全新的JS线程，主Javascript线程是负责DOM的线程，而service worker线程被设计成无法访问DOM。从事前端开发的都知道，如果同时存在多个UI线程，会出现操作失控，控制紊乱的效果，举个简单的例子，如果一个线程将DOM删除，另一个线程对DOM进行点击等操作，这就出行混乱的现象，所以只能有一个UI线程，所以Service Worder是一个不能对DOM操作的线程。<br/>

**Service Worder缓存**<br/>
Service Worker缓存关键在于监听Fetch事件和管理Cache资源，不过在这之间需要注册激活，这里注册激活流程不详细讲解，它能拦截所有的请求（通过监听fetch事件，任何对网络资源的请求都会触发该事件），并内置了一个完全异步的存储系统（Caches属性，完全异步并能存储全部种类的网络资源），这是它能精细化控制缓存的关键。

Service Worker的出现并不是单纯的为解决精细化控制浏览器缓存问题的。它能充当代理服务器这一能力（通过拦截所有请求实现），能够实现HTTP缓存无法实现的功能:离线缓存。因为在HTTP缓存策略下，如果一个资源过了服务器规定的到期时间，则必须要发起请求，一旦网络连接有问题，整个网站就会出现功能问题。而在Service Worker控制下的缓存，能够在代码中发现网络连接问题并直接返回缓存的资源。
由于Service Worker能拦截HTTP请求，充当代理服务器这个功能，所以必须运行在HTTPS的Origin中，localhost也被认为是安全的，可以用于开发调试。<br/>

#### Memory Cache（200 from memory cache）
Memory Cache 也就是内存中的缓存，主要包含的是当前中页面中已经抓取到的资源,例如页面上已经下载的样式、脚本、图片等。读取内存中的数据肯定比磁盘快,内存缓存虽然读取高效，可是缓存持续性很短，会随着进程的释放而释放。 一旦我们关闭 Tab 页面，内存中的缓存也就被释放了。<br/>
需要注意的事情是，内存缓存在缓存资源时并不关心返回资源的HTTP缓存头Cache-Control是什么值，同时资源的匹配也并非仅仅是对URL做匹配，还可能会对Content-Type，CORS等其他特征做校验。<br/>

#### Disk Cache（200 from disk cache）
Disk Cache 也就是存储在硬盘中的缓存，读取速度慢点，但是什么都能存储到磁盘中，比之 Memory Cache 胜在容量和存储时效性上。<br/>
在所有浏览器缓存中，Disk Cache 覆盖面基本是最大的。它会根据 HTTP Herder 中的字段判断哪些资源需要缓存，哪些资源可以不请求直接使用，哪些资源已经过期需要重新请求。并且即使在跨站点的情况下，相同地址的资源一旦被硬盘缓存下来，就不会再次去请求数据。绝大部分的缓存都来自 Disk Cache。<br/>

#### Push Cache
Push Cache（推送缓存）是 HTTP/2 中的内容，当以上三种缓存都没有命中时，它才会被使用。它只在会话（Session）中存在，一旦会话结束就被释放，并且缓存时间也很短暂，在Chrome浏览器中只有5分钟左右，同时它也并非严格执行HTTP头中的缓存指令。<br/>
![image.png](https://upload-images.jianshu.io/upload_images/5097943-e56c60599f516de1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 
原理：图中简化了浏览器和服务器的交互过程，横轴代表时间轴，每个虚线区间是1个RTT。红色竖线表示DOM 加载完成的时间。从图中可知，虽然存在并发传输, 但主页index.html和依赖的资源common.css、0684a8bf.css、comb.nowrap.0b772fee.js等总体上是顺序的，等待资源响应的时间减慢了主页面加载速度。<br/>

如果服务端接收到客户端主请求，能够“预测”主请求的依赖资源，在响应主请求的同时，主动并发推送依赖资源至客户端。客户端解析主请求响应后，可以”无延时”从本地缓存获取依赖资源, 减少访问延时, 提高访问体验，也加大了链路的并发能力。Server Push正是基于此原理来提高网络体验。<br/>
![image.png](https://upload-images.jianshu.io/upload_images/5097943-b4789e86ad29c605.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

图3说明了若采用服务端推送的功能，则JS/CSS资源基本可以和HTML资源同步到达，浏览器可以“无延时”获取JS/CSS资源，客户端的延时最多可以减少一个RTT。<br/>

如果以上四种缓存都没有命中的话，那么只能发起请求来获取资源了。<br/>

## 四、缓存的分类以及它们之间的区别
HTTP请求根据缓存类型主要分为两种缓存方式：**强缓存和协商缓存**。<br/>
#### 强缓存

强缓存：不会向服务器发送请求，直接从缓存中读取资源，在chrome控制台的Network选项中可以看到该请求返回200的状态码，并且Size显示from disk cache或from memory cache。强缓存可以通过设置两种 HTTP Header 实现：**Expires 和 Cache-Control**。<br/>

**1. Cache-Control**<br/>
在HTTP/1.1中，Cache-Control是最重要的规则，主要用于控制网页缓存。比如当Cache-Control:max-age=300时，则代表在这个请求正确返回时间（浏览器也会记录下来）的5分钟内再次加载资源，就会命中强缓存。<br/>
Cache-Control 可以在请求头或者响应头中设置，并且可以组合使用多种指令：<br/>
![Cache-Control.png](https://upload-images.jianshu.io/upload_images/5097943-3965a59812a0e738.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**public**：所有内容都将被缓存（客户端和代理服务器都可缓存）。具体来说响应可被任何中间节点缓存，如 Browser <-- proxy1 <--  proxy2 <-- Server，中间的proxy可以缓存资源，比如下次再请求同一资源proxy1直接把自己缓存的东西给 Browser 而不再向proxy2要。<br/>

**private**：所有内容只有客户端可以缓存，Cache-Control的默认取值。具体来说，表示中间节点不允许缓存，对于Browser <-- proxy1 <--  proxy2 <-- Server，proxy 会老老实实把Server 返回的数据发送给proxy1,自己不缓存任何数据。当下次Browser再次请求时proxy会做好请求转发而不是自作主张给自己缓存的数据。<br/>

**no-cache**：客户端缓存内容，是否使用缓存则需要经过协商缓存来验证决定。表示不使用 Cache-Control的缓存控制方式做前置验证，而是使用 Etag 或者Last-Modified字段来控制缓存。需要注意的是，no-cache这个名字有一点误导。设置了no-cache之后，并不是说浏览器就不再缓存数据，只是浏览器在使用缓存数据时，需要先确认一下数据是否还跟服务器保持一致。<br/>

**no-store**：所有内容都不会被缓存，即不使用强制缓存，也不使用协商缓存。<br/>

**max-age**：max-age=xxx (xxx is numeric)表示缓存内容将在xxx秒后失效。<br/>

**s-maxage**（单位为s)：同max-age作用一样，只在代理服务器中生效（比如CDN缓存）。比如当s-maxage=60时，在这60秒中，即使更新了CDN的内容，浏览器也不会进行请求。max-age用于普通缓存，而s-maxage用于代理缓存。s-maxage的优先级高于max-age。如果存在s-maxage，则会覆盖掉max-age和Expires header。<br/>

**max-stale**：能容忍的最大过期时间。max-stale指令标示了客户端愿意接收一个已经过期了的响应。如果指定了max-stale的值，则最大容忍时间为对应的秒数。如果没有指定，那么说明浏览器愿意接收任何age的响应（age表示响应由源站生成或确认的时间与当前时间的差值）。<br/>

**min-fresh**：能够容忍的最小新鲜度。min-fresh标示了客户端不愿意接受新鲜度不多于当前的age加上min-fresh设定的时间之和的响应。<br/>
![image.png](https://upload-images.jianshu.io/upload_images/5097943-3ac31853ce914302.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**2. Expires**<br/>
缓存过期时间，用来指定资源到期的时间，是服务器端的具体的时间点。也就是说，Expires=max-age + 请求时间，需要和Last-modified结合使用。Expires是Web服务器响应消息头字段，在响应http请求时告诉浏览器在过期时间前浏览器可以直接从浏览器缓存取数据，而无需再次请求。<br/>
Expires 是 HTTP/1 的产物，受限于本地时间，如果修改了本地时间，可能会造成缓存失效。Expires: Wed, 22 Oct 2018 08:41:00 GMT表示资源会在 Wed, 22 Oct 2018 08:41:00 GMT 后过期，需要再次请求。<br/>
**Expires其实是过时的产物，现阶段它的存在只是一种兼容性的写法，如果同时存在Cache-Control和Expires，Cache-Control的优先级更高。**<br/>

强缓存判断是否缓存的依据来自于**是否超出某个时间或者某个时间段**，而不关心服务器端文件是否已经更新，这可能会导致加载文件不是服务器端最新的内容，那我们如何获知服务器端内容是否已经发生了更新呢？此时我们需要用到协商缓存策略。<br/>

#### 协商缓存
协商缓存就是**强制缓存失效**后，**浏览器携带缓存标识向服务器发起请求，由服务器根据缓存标识决定是否使用缓存的过程**，主要有以下两种类型：**Last-Modified 和 ETag**。<br/>
+ 协商缓存生效，返回304和Not Modified<br/>
![image.png](https://upload-images.jianshu.io/upload_images/5097943-3bd7bfb78ffcc7de.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

+ 协商缓存失效，返回200和请求结果<br/>
![image.png](https://upload-images.jianshu.io/upload_images/5097943-6cb9fdc8aaf84273.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**1. Last-Modified和If-Modified-Since**<br/>
浏览器在第一次访问资源时，服务器返回资源的同时，在response header中添加 Last-Modified的header，值是这个资源在服务器上的最后修改时间，浏览器接收后缓存文件和header。<br/>
```
Last-Modified: Fri, 22 Jul 2016 01:47:00 GMT
```
浏览器下一次请求这个资源，浏览器检测到有 Last-Modified这个header，于是请求添加If-Modified-Since这个header，值就是Last-Modified中的值；服务器再次收到这个资源请求，会根据 If-Modified-Since 中的值与服务器中这个资源的最后修改时间对比，如果没有变化，返回304和空的响应体，直接从缓存读取，如果If-Modified-Since的时间小于服务器中这个资源的最后修改时间，说明文件有更新，于是返回新的资源文件和200。<br/>
![image.png](https://upload-images.jianshu.io/upload_images/5097943-a85117c6410d0091.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
如果本地打开缓存文件，即使没有对文件进行修改，但还是会造成 Last-Modified 被修改，服务端不能命中缓存导致发送相同的资源
因为 Last-Modified 只能以秒计时，如果在不可感知的时间内修改完成文件，那么服务端会认为资源还是命中了，不会返回正确的资源。<br/>

既然根据文件修改时间来决定是否缓存尚有不足，能否可以直接根据文件内容是否修改来决定缓存策略？所以在 HTTP / 1.1 出现了 ETag 和If-None-Match。<br/>

**2. ETag 和If-None-Match**<br/>
Etag是服务器响应请求时，返回当前资源文件的一个唯一标识(由服务器生成)，只要资源有变化，Etag就会重新生成。浏览器在下一次加载资源向服务器发送请求时，会将上一次返回的Etag值放到request header里的If-None-Match里，服务器只需要比较客户端传来的If-None-Match跟自己服务器上该资源的ETag是否一致，就能很好地判断资源相对客户端而言是否被修改过了。如果服务器发现ETag匹配不上，那么直接以常规GET 200回包形式将新的资源（当然也包括了新的ETag）发给客户端；如果ETag是一致的，则直接返回304知会客户端直接使用本地缓存即可。<br/>
![image.png](https://upload-images.jianshu.io/upload_images/5097943-968b61f15a2fd544.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**3. 两者的区别**<br/>
1. 首先在精确度上，Etag要优于Last-Modified。<br/>
Last-Modified的时间单位是秒，如果某个文件在1秒内改变了多次，那么他们的Last-Modified其实并没有体现出来修改，但是Etag每次都会改变确保了精度；如果是负载均衡的服务器，各个服务器生成的Last-Modified也有可能不一致。<br/>
2. 第二在性能上，Etag要逊于Last-Modified，毕竟Last-Modified只需要**记录时间**，而Etag需要服务器通过算法来计算出一个**hash值**（记录内容是否发生变化）。<br/>
3. 第三在优先级上，服务器校验优先考虑Etag。<br/>

## 五、实际应用
#### 1.频繁变动的资源
```
Cache-Control: no-cache
```
对于频繁变动的资源，首先需要使用Cache-Control: no-cache 使浏览器每次都请求服务器，然后配合 ETag 或者 Last-Modified 来验证资源是否有效。这样的做法虽然不能节省请求数量，但是能显著减少响应数据大小。<br/>
#### 2.不常变化的资源
```
Cache-Control: max-age=31536000
```
通常在处理这类资源时，给它们的 Cache-Control 配置一个很大的 max-age=31536000 (一年)，这样浏览器之后请求相同的 URL 会命中强制缓存。而为了解决更新的问题，就需要在文件名(或者路径)中添加 hash， 版本号等动态字符，之后更改动态字符，从而达到更新URL 的目的，更新强制缓存的有效期。<br/>

## 六、用户操作如何触发浏览器缓存
+ 打开网页，地址栏输入地址： 查找 disk cache 中是否有匹配。如有则使用；如没有则发送网络请求。
+ 普通刷新 (F5)：因为 TAB 并没有关闭，因此 memory cache 是可用的，会被优先使用(如果匹配的话)。其次才是 disk cache。
+ 强制刷新 (Ctrl + F5)：浏览器不使用缓存，因此发送的请求头部均带有 Cache-control: no-cache(为了兼容，还带了 Pragma: no-cache),服务器直接返回 200 和最新内容。

简单理解：
```
打开新页面访问-> 
200（新数据，并缓存） -> 
退出浏览器再进来-> 
200(from disk cache) -> 
普通刷新 -> 
200(from memory cache)
```
## 七、总结
**强缓存优先于协商缓存**，若强制缓存**(Expires和Cache-Control)**生效则直接使用缓存，若不生效则进行协商缓存**(Last-Modified / If-Modified-Since和Etag / If-None-Match)**，协商缓存由服务器决定是否使用缓存，若协商缓存失效，那么代表该请求的缓存失效，返回200，重新返回资源和缓存标识，再存入浏览器缓存中；生效则返回304，继续使用缓存。具体流程图如下：
![image.png](https://upload-images.jianshu.io/upload_images/5097943-828db9a6a29decfb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
看到这里，不知道你是否存在这样一个疑问:如果什么缓存策略都没设置，那么浏览器会怎么处理？<br/>
对于这种情况，浏览器会采用一个启发式的算法，通常会取响应头中的 Date 减去 Last-Modified 值的 10% 作为缓存时间。<br/>